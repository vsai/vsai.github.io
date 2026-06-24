---
layout: post
title: "Observability as a prioritization lever"
date: 2025-10-27 10:00:00 +0800
company: bonkbot
---

BONKbot is a trading bot for low-market-cap Solana tokens, better known as
memecoins. Every trade is a swap, one token for another, executed on-chain as a
single transaction.

These tokens move fast. A price can drop 90% in seconds. So the latency between a
user clicking buy and the transaction landing decides how close their fill is to
the price we quoted. Slower means a worse fill, or a missed entry.

Speed is the product. A faster bot gives better fills, and users notice. So when
swap latency climbed, we set out to bring it down.

## The proposals

We talked it through as a team, and everyone converged on the same stage:
transaction submission, where a signed transaction leaves our infrastructure for
the chain. The question they wanted to answer was how to get RPCs to land our
transactions on-chain faster.

- **Submit to multiple endpoints in parallel.** Send the transaction to
  [Jito](https://www.jito.network/) and a second RPC provider at once and take
  whichever lands first, hedging against a slow or congested path.
- **Raise the default priority fee.** Solana orders pending transactions partly
  by the priority fee attached to them, so a higher fee buys earlier inclusion.
- **Shrink the transaction body.** A smaller serialized transaction moves through
  an RPC faster. This is the most involved option: every change to the transaction
  layout means re-validating it against every DEX we route through, to keep the
  transaction valid everywhere.

I wasn't convinced. Each of these would help, but no one could say by how much, or
rank them against each other. They all rested on the same unproven assumption, that
submission was where the latency piled up. It didn't feel like the real bottleneck
to me, but I had no more data than anyone else, so my doubts didn't carry. We were
about to commit the team on a hunch.

## Measuring where the time goes

To move the conversation, I needed evidence that we were aiming at the wrong stage.
So I audited the whole pipeline, from the user's tap to on-chain confirmation, and
timed every step:

| # | Stage | What happens |
|---|-------|--------------|
| 1 | Button tapped | The user taps buy in the app |
| 2 | Request received | The swap request reaches our backend |
| 3 | Route built | The route builder returns the optimal route for the trade |
| 4 | Transaction signed | The key-gen and signing service signs the transaction ([how our key management works](https://bonkbot.io/blog/a-hardware-wallet-in-the-cloud-introducing-bonkbot-s-next-gen-key-management-system)) |
| 5 | Submitted | The RPC accepts the transaction for submission |
| 6 | Processed | The RPC reports the transaction processed (included in a block) |
| 7 | Confirmed | The RPC reports the transaction confirmed by the cluster |

Our existing setup had two blind spots.

First, Grafana tracked a single number end to end, request received to confirmed.
It told us the swap was slow, but not which stage was responsible. I added a graph
for every hop in between, so each stage carried its own latency.

Second, those graphs only showed aggregates, and aggregates hide outliers. One very
slow swap barely shifts an average, so the worst cases disappeared into the mean
with nothing obvious to act on. To catch them, I logged each stage's timing to a
Postgres table, queryable down to a single swap, or a single user.

Between the per-stage graphs and the per-swap records, we could finally see where
the time went, and for whom.

## What the data changed

The measurements contradicted what the proposals assumed. Submission wasn't the
dominant cost. The largest and cheapest wins sat in the earlier stages and in how
transactions moved between services. That reordered the work: the submission ideas
dropped down the list, and three changes no one had proposed moved to the top.

### Signer transport: HTTPS to NATS

Calls to the signing service ran over HTTPS, opening a new request each time. That
per-request overhead made the calls spiky, and the tail dominated the signing
stage. Moving the communication to [NATS](https://nats.io/), which holds a
persistent connection instead of negotiating a new request per call, cut the
variance sharply and brought the p90 baseline down roughly **20%**. It was the
highest-impact change, and it had nothing to do with submission.

### Act on processed, repair on confirmed

On Solana, a transaction reaches *processed* once it lands in a block, and
*confirmed* slightly later, after the cluster votes on it. We were waiting for the
confirmed event before treating a swap as complete. The data showed that the large
majority of processed transactions go on to confirm, so we started treating
processed as the completion signal and acting on it immediately, with a repair
mechanism on the confirmed event to handle the minority that don't. That cut the
confirmation wait out of the perceived completion time, around **60–120ms**.

### Fewer network hops

The route builder already holds an open websocket to the RPC, but transactions
detoured through another service before reaching it. We let the route builder
submit directly, to the signing service and then to the RPC over that open
connection. Another **60–90ms** gone.

## Results

| Change | Effect |
|--------|--------|
| Signer transport HTTPS → NATS | p90 baseline down ~20%, much lower variance |
| Act on processed, repair on confirmed | ~60–120ms off perceived completion time |
| Direct submission, fewer hops | ~60–90ms off the path |

None of these were the optimizations the team came in wanting to build. The
instrumentation moved them to the top of the list. And because we'd measured the
gains instead of debating them, the team aligned on the order of work quickly.

## Prioritizing what's next

With the pipeline instrumented, we make the next decision from data rather than
intuition. The route builder runs multi-region; pointing the frontend straight at
it would cut real network latency for distant users and drop another inter-service
hop on the way in. We can estimate that payoff now, before building it, and weigh
it against the submission optimizations still on the list.

## The takeaway

The hard part of this work wasn't the engineering. Each of these changes was
straightforward to build. The hard part was being sure we were building the right
ones, in the right order.

That's what the instrumentation gave us. It didn't make anything faster on its own.
It showed where the time actually went, replaced competing hunches with a ranked
list, and kept us from pouring effort into the stage we'd only assumed was slow.
The metrics were cheap to add. Knowing we were working on the right thing was the
real return.
