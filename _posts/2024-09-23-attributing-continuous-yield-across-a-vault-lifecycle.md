---
layout: post
title: "Attributing continuous yield across a vault lifecycle"
date: 2024-09-23 10:00:00 +0800
company: cega
---

Cega builds structured-products vaults. Each vault tokenizes an options product. Banks
sell these off-chain as private notes: Fixed Coupon Notes, Dual Currency Swaps, Shark-Fin
Notes. They're real-world assets, the same products traded in traditional markets. A user
deposits. Their principal backs an options trade run by a market maker. At settlement they
collect a coupon. If the trade goes against them, they get a converted payoff instead.

We wanted deposits to work harder. So we let users deposit
[Pendle](https://www.pendle.finance/) Yield Tokens. Pendle splits a yield-bearing token
in two. One part is a principal token, which you redeem at maturity. The other is a
**yield token (YT)**, which earns the underlying yield until then. Hold the YT and you
collect that yield, block by block. You claim it from Pendle.

![How Pendle splits a yield-bearing token into a principal token (PT) and a yield token (YT). PT accretes to par by maturity; the YT streams the yield and expires worthless.](/assets/diagrams/pendle-split.png)

*See Pendle's [yield-tokenization basics](https://docs.pendle.finance/pendle-academy/pendle-101/chapter-2-yield-tokenization-basics) for the details.*

The YT's underlying was weETH, [Ether.fi](https://www.ether.fi/)'s staked-ETH token.
Holding weETH exposure earns Ether.fi loyalty points. So one YT deposit earned two rewards
at once: Pendle's yield and Ether.fi's points.

Both rewards reduced to one problem: the YTs sat pooled in Cega's treasury, not
attributable to any one depositor. The yield accrued as one balance. To Ether.fi, the
treasury looked like one big holder of weETH. But the rewards belonged to the depositors
underneath. We had to split both back to them, per user. This is the engine we built to do
that.

## Why this was hard

Three things made this hard.

**Rewards are earned continuously, but paid out only at discrete moments.** The YT earns
yield every block. But nothing lands until someone claims. Between claims, value builds up
with no transaction to mark it. A user earned an exact amount between Tuesday and Thursday, but no on-chain event records
it. Points work the same way:
earned continuously, credited on Ether.fi's own schedule.

**The treasury pooled every vault's YT, each vault at a different lifecycle stage.** A
single balance held deposits from all of them. Each vault ran its own lifecycle: deposits
queue, funds lock when the trade goes live, they settle when it ends. Entitlement to the
yield shifted with the stage. While a trade is live, the yield belongs to the vault, not to
any one depositor, until settlement tells us who kept their principal. So the whole treasury
had to be normalized across every vault at once, not read one vault at a time.

![The vault lifecycle. Yield accrues to the held token the entire way through, but who is entitled to it shifts as funds move from queue to locked to settled.](/assets/diagrams/lifecycle.png)

Pendle sees only the token. As long as the YT sits in a Cega address, it keeps accruing.

**The claim, and its timing, belonged to the user.** A claim pulls the pooled yield into
Cega's treasury, with nothing on-chain to say whose it is. Points are Ether.fi's to grant;
it accrues them off the balances we report. Someone can deposit, withdraw, or take their
share at any block. By then we already have to know what they're owed.

A redemption is also all-or-nothing. Pendle redeems for whoever holds the YT. That's the
whole pooled treasury. To pay one user, we redeem the lot, hand them their share, and leave
the rest. Split that against the wrong ownership and the yield lands in the wrong hands.

So at any block, for any user, we have to reconstruct what they're owed from on-chain state
alone:

> For any span of blocks, which users were entitled to the yield that accrued? During that
> span, some were queued, some were locked in a live trade, some had asked to withdraw, and
> a trade may have settled partway through.

Everything below answers that.

## Measuring yield without claiming it

Splitting yield between users needs the amount that accrued in each window. No contract
exposes that number. The only way to read it is to run the redemption.

One option is to claim. Pendle's redeem function pays the accrued yield into the treasury,
and the balance change is the amount. But claiming every few hours just to measure costs
gas, and changes real state on mainnet.

Instead, we measured on a copy. At each measurement point, we **forked mainnet at that exact
block**. We ran the claim on the fork and compared the treasury balance before and after.
The number is exact, and mainnet never moves.

```
measure_accrued_at(block N):
    fork   = fork_mainnet_at(N)       # ephemeral copy of the chain
    before = fork.treasury_balance()
    fork.simulate(redeem_yield())     # claim runs on the fork only
    after  = fork.treasury_balance()
    discard(fork)
    return after - before             # accrued, unclaimed, as of N
```

![Measuring accrued yield on a fork. Read the balance, simulate the claim on the forked chain only, read the balance again. The difference is the yield accrued but not yet claimed as of that block. Then throw the fork away.](/assets/diagrams/fork.png)

This number is a running total: how much sits unclaimed at that block. To get a period's
amount, difference two snapshots and add back anything claimed between them.

## Slicing time into windows

To measure each period, we slide a fixed-size window across the chain. Each window covers
about eight hours. We process them in order, catching up to the chain head.

For one window, the yield that accrued comes from three quantities:

```
accrued during window
    =  claimed during    # real claims that landed inside the window
    +  balance at end    # simulated on a fork at the end block
    -  balance at start  # the previous window's end, reused
```

![The accrual identity for one window: the pool balance at the end, plus what was claimed during the window, minus the balance at the start, equals what accrued during the window.](/assets/diagrams/accrual.png)

The claimed-during term keeps the count honest. Without it, a window with a claim would
look like almost nothing accrued, and everyone in it would be under-credited. Say the pool holds 100 when the window opens.
Another 30 accrues during the window. Midway, someone claims 50. Now the fork at the end
reads `100 + 30 − 50 = 80`. End minus start is `80 − 100 = −20`. Nonsense: the 50 wasn't
lost, it was real yield, already collected. Add it back, `80 + 50 − 100 = 30`, and you get
the true accrual.

Two details keep this cheap. We reuse the previous window's *balance at end* as the next
window's *balance at start*, so windows tile with no gap and no double-count. And *claimed
during* comes straight from the on-chain claim events, indexed by block. One fork per
window, and the arithmetic closes.

## Who held the exposure?

The second input is ownership: each address's share of the exposure at the window's edge,
held fixed across the window. Windows are short, hours not days, so a user who joins
mid-window rounds to the nearest edge and the error stays small. The snapshot comes from a
few buckets, because "holding the exposure" means different things at different points in
the lifecycle.

| Bucket | Who it credits | Why |
|--------|----------------|-----|
| **Queue** | the depositor | Funds in the deposit queue belong to whoever deposited them. |
| **In-vault** | each user, by share | Inside a live vault, a user's exposure is their share of the vault's holdings. |
| **Locked** | the vault itself | While a trade is live, we park the vault's exposure at the vault and redistribute it on settlement. |
| **Excess** | a reconciliation address | Anything the treasury holds beyond what users account for. Swept aside so we never misassign it. |

A vault's status (open, auctioned, locked, settled, converted) decides how much exposure
it holds, and whether the locked bucket parks at the vault or flows back to users. We sum
the buckets, then split the window's accrual across addresses in proportion to their
exposure:

```
user's cut  =  window accrual  ×  ( user's tokens / reconciled total )
```

The reconciled total is every bucket summed: Queue, In-vault, Locked, and Excess. It equals
the treasury's actual YT balance.

A guard sits around that total. The buckets must sum to the treasury's *actual* yield-token
balance. If the treasury holds more than users account for, the surplus goes to a capture
address. If it holds less, that's impossible, so the run fails loudly.

### Settling up

At settlement, the parked yield is distributed by trade outcome. If users kept their
principal, it splits among them by shares held. If the trade converted (the downside hit,
the market maker took the principal), the market maker earned the yield too, and the whole
parked amount goes to it.

Shares are counted as of the window's start, not its end. By settlement, some users may
have withdrawn, which would skew the split. The start captures who actually held the
position while it was locked.

We add each window's result to a running per-user ledger. Users claim on their own
schedule, so this ledger is what makes payouts work. It always reflects what each person is
owed. When someone claims, we subtract what they've already taken and settle the rest.
Persisting it also lets the job restart after a crash without double-counting.

## One engine, two consumers

The points feed reuses the same engine. Attributing Ether.fi's points needs what the yield
engine already produces: each depositor's exposure, computed individually rather than as a
pooled total. Only one thing changes: a flag that governs what happens to locked funds.

![One ownership engine, two consumers. The same per-user exposure model feeds both, switched by one flag: locked funds either park at the vault for later redistribution, or credit straight to the user.](/assets/diagrams/two-consumers.png)

For yield, the flag parks locked funds at the vault and redistributes on settlement. We
don't yet know who will own that yield until the trade resolves. For points, it credits
locked funds straight to the user. Points have no lock and no discrete claim to reconcile.
Ether.fi accrues points off whatever balance you hold, so there's nothing to park.

That one flag changes everything downstream:

- **No fork.** Points accrue on a balance, not a redeemable amount.
- **No sliding window.** We ask what each user holds at a block, not what accrued over a range.
- **No ledger.** It's recomputed on demand, not accumulated.

The yield path is a daily job that walks windows and persists a ledger. The points path is
a stateless endpoint that answers a point-in-time query.

## How it runs

Three pieces carry this in production.

A **subgraph** indexes the chain: deposits, withdrawals, vault locks and settlements, and
Pendle claim events. Any block's ownership and claims become queryable, with no re-scan.

A **cron job** runs the yield path on a daily schedule. Each run splits the day into
~8-hour windows. That's short enough to catch the yield changes as they're earned. For
every window, it pulls ownership and claims from the subgraph, forks mainnet at the
boundary to measure accrual, attributes the yield per user, and appends that window's
result to a persisted **ledger**.

Two **endpoints** serve the output. One reads the ledger and tells the claim flow what each
user is owed. The other is the points endpoint. It computes each user's exposure on demand,
straight from the subgraph, and hands it to Ether.fi.

![The production wiring. A subgraph indexes on-chain events. A daily cron job reads the subgraph and forks mainnet to measure accrual, attributes yield per user, and writes a persisted ledger that a yield endpoint serves to the claim flow. A separate points endpoint computes exposure on demand from the subgraph and serves Ether.fi.](/assets/diagrams/infra.png)

Yield needs history, so it runs on a schedule and builds a ledger. Points need only a
snapshot, so they read live and store nothing.

## Key learnings

The hard part was never the code. It was the many moving pieces:

- **Non-linear accrual.** Yield lands unevenly, block to block.
- **A shared pool.** Every vault and user accrues into one balance, each vault at a different lifecycle stage, each user with a different balance.
- **Claims at any time.** Users can claim at any block, offsetting the balances we track.
- **Claims we don't see.** They land outside our system, on Pendle and Ether.fi.
- **No ground truth.** Nothing to test against, so no way to directly verify a result.

The last one had no clean fix. With no source of truth, the only check was to build each
part carefully and audit real balances against the on-chain totals, again and again, until
the numbers held.

The rest came down to one reduction. Break each concern into its own problem, then solve a
single window exactly: how much yield accrued, and who held it. Chain the windows so each
starts where the last ended. Get one window right, get the seam right, and the whole
timeline follows by induction.
