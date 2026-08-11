---
layout: page
title: Work
permalink: /work/
---

<style>
.cv-timeline { position: relative; margin: 2.2rem 0 1rem; padding-left: 30px; }
.cv-timeline::before {
  content: ""; position: absolute; left: 7px; top: 10px; bottom: 10px;
  width: 2px; background: #e7e7e7;
}
.cv-item { position: relative; margin-bottom: 20px; }
.cv-item:last-child { margin-bottom: 0; }
.cv-dot {
  position: absolute; left: -29px; top: 24px; width: 14px; height: 14px;
  border-radius: 50%; background: #2a7ae2; border: 3px solid #fdfdfd;
  box-shadow: 0 0 0 1px #d6d6d6;
}
.cv-card {
  background: #fff; border: 1px solid #e7e7e7; border-radius: 10px;
  padding: 18px 20px;
  transition: box-shadow .2s ease, transform .2s ease, border-color .2s ease;
}
.cv-card:hover {
  box-shadow: 0 8px 24px -10px rgba(0,0,0,.22);
  border-color: #cfe0f7; transform: translateY(-2px);
}
.cv-head {
  display: flex; justify-content: space-between; align-items: baseline;
  gap: 12px; flex-wrap: wrap;
}
.cv-co { margin: 0; font-size: 1.25rem; line-height: 1.2; }
.cv-co a { color: #2a7ae2; text-decoration: none; }
.cv-co a:hover { text-decoration: underline; }
.cv-dates { color: #828282; font-size: .82rem; white-space: nowrap; font-variant-numeric: tabular-nums; }
.cv-role { color: #2a7ae2; font-weight: 600; font-size: .9rem; margin: 3px 0 10px; }
.cv-lead { margin: 0 0 9px; color: #444; font-weight: 500; }
.cv-points { margin: 0; padding-left: 1.15rem; color: #3a3a3a; }
.cv-points li { margin: 5px 0; padding-left: 2px; }
.cv-topics { margin-top: 14px; display: flex; gap: 7px; flex-wrap: wrap; }
.cv-topic {
  font-size: .72rem; color: #2a7ae2; background: #eef4fd;
  border: 1px solid #dce8fb; border-radius: 999px; padding: 3px 11px;
}
.cv-stats { margin-top: 14px; display: flex; gap: 22px; flex-wrap: wrap; }
.cv-stat { font-size: .8rem; color: #777; line-height: 1.25; }
.cv-stat b { display: block; color: #1a1a1a; font-weight: 700; font-size: 1.05rem; }
.cv-tags { margin-top: 13px; display: flex; gap: 7px; flex-wrap: wrap; }
.cv-tag {
  display: inline-block; font-size: .72rem; color: #555; background: #f2f2f2;
  border: 1px solid #ececec; border-radius: 999px; padding: 3px 11px;
}
.cv-tag a { color: inherit; text-decoration: none; }
.cv-tag a:hover { text-decoration: underline; }
.cv-stack {
  margin-top: 14px; padding-top: 12px; border-top: 1px solid #f0f0f0;
  font-size: .73rem; color: #9a9a9a;
}
.cv-stack .label { color: #b3b3b3; }
.cv-stack-light { display: block; margin-top: 4px; color: #bdbdbd; }
.cv-writing { margin-top: 14px; }
.cv-writing summary {
  cursor: pointer; font-size: .78rem; color: #2a7ae2; font-weight: 700;
  list-style: none; display: inline-flex; align-items: center; gap: 6px;
  user-select: none; padding: 5px 12px; border-radius: 999px;
  background: #eef4fd; border: 1px solid #dce8fb;
  transition: background .15s ease, border-color .15s ease;
}
.cv-writing summary:hover { background: #e2edfb; border-color: #c7dcf5; }
.cv-writing summary::-webkit-details-marker { display: none; }
.cv-writing summary::before {
  content: "›"; display: inline-block; font-size: 1.15em; line-height: 1;
  transition: transform .15s ease;
}
.cv-writing[open] summary::before { transform: rotate(90deg); }
.cv-writing-count {
  background: #eaf2fd; color: #2a7ae2; border-radius: 999px;
  font-size: .68rem; padding: 1px 8px; font-weight: 700;
}
.cv-writing-list {
  margin: 9px 0 0; padding: 0; list-style: none;
  max-height: 128px; overflow-y: auto;
}
.cv-writing-list li { margin: 0 0 2px; }
.cv-writing-list li a {
  display: block; padding: 6px 10px; border-radius: 6px; font-size: .85rem;
  color: #3a3a3a; text-decoration: none; transition: background .15s ease, color .15s ease;
}
.cv-writing-list li a:hover { background: #f4f8fe; color: #2a7ae2; }
.cv-videos { margin-top: 14px; display: flex; gap: 10px; flex-wrap: wrap; }
.cv-video-card { display: block; width: 132px; text-decoration: none; }
.cv-video-thumb {
  position: relative; width: 100%; aspect-ratio: 16 / 9; border-radius: 6px;
  overflow: hidden; background: #eee;
}
.cv-video-thumb img { width: 100%; height: 100%; object-fit: cover; display: block; }
.cv-video-thumb::after {
  content: "▶"; position: absolute; inset: 0; display: flex;
  align-items: center; justify-content: center; color: #fff; font-size: 0.8rem;
  background: rgba(0, 0, 0, 0.18); transition: background .15s ease;
}
.cv-video-card:hover .cv-video-thumb::after { background: rgba(0, 0, 0, 0.32); }
.cv-video-title {
  margin-top: 5px; font-size: 0.72rem; line-height: 1.25; color: #3a3a3a;
}
.cv-video-card:hover .cv-video-title { color: #2a7ae2; }
.cv-video-client { display: block; color: #b3b3b3; font-size: 0.68rem; margin-top: 1px; }
@media (max-width: 600px) {
  .cv-head { flex-direction: column; gap: 2px; }
  .cv-stats { gap: 16px; }
}
</style>

A short history of where I've worked and what I built.

<div class="cv-timeline" markdown="0">

  <div class="cv-item">
    <span class="cv-dot"></span>
    <div class="cv-card">
      <div class="cv-head">
        <h3 class="cv-co"><a href="https://bonkbot.io" target="_blank" rel="noopener">BONKbot</a></h3>
        <span class="cv-dates">Nov 2024 — Jun 2026</span>
      </div>
      <div class="cv-role">Senior Staff Engineer &amp; Engineering Manager</div>
      <p class="cv-lead">Building a low-latency, on-chain crypto trading platform for Solana memecoins on Telegram and web.</p>
      <ul class="cv-points">
        <li>Both led and built — owned core systems hands-on while managing the team behind the platform that drives 80% of revenue.</li>
        <li>Owned the Telegram bot, web user API, authentication, infrastructure, the swap execution layer, and portfolio/PnL paths.</li>
        <li>Project lead for Multiwallet v1.</li>
        <li>Led Auth v2 — passkey authentication integrated with our proprietary signer.</li>
        <li>Drove an observability and team-structure push that cut swap latency (p50 1.1s&nbsp;→&nbsp;0.8s, p90 1.5s&nbsp;→&nbsp;1.1s) and portfolio/PnL latency (p90 5.3s&nbsp;→&nbsp;3.8s).</li>
        <li>Also shipped referrals, internal tooling, and more.</li>
      </ul>
      <div class="cv-topics">
        <span class="cv-topic">low-latency trading</span>
        <span class="cv-topic">observability</span>
        <span class="cv-topic">passkey auth</span>
        <span class="cv-topic">multiwallet</span>
        <span class="cv-topic">infra</span>
      </div>
      <div class="cv-stats">
        <span class="cv-stat"><b>~1.5M</b> lifetime users</span>
        <span class="cv-stat"><b>~$1B</b> peak monthly volume</span>
        <span class="cv-stat"><b>−27%</b> swap latency (p90)</span>
      </div>
      {%- assign bonkbot_posts = site.posts | where: "company", "bonkbot" -%}
      {%- if bonkbot_posts.size > 0 -%}
      <details class="cv-writing" open>
        <summary>Writing from this chapter <span class="cv-writing-count">{{ bonkbot_posts.size }}</span></summary>
        <ul class="cv-writing-list">
          {%- for post in bonkbot_posts -%}
          <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
          {%- endfor -%}
        </ul>
      </details>
      {%- endif -%}
      <div class="cv-stack">
        TypeScript · Node.js · Postgres · GCP · Claude&nbsp;Code · Codex
        <span class="cv-stack-light"><span class="label">Some exposure:</span> ClickHouse</span>
      </div>
    </div>
  </div>

  <div class="cv-item">
    <span class="cv-dot"></span>
    <div class="cv-card">
      <div class="cv-head">
        <h3 class="cv-co"><a href="https://www.agentmanagerhq.com" target="_blank" rel="noopener">Agent Manager HQ</a></h3>
        <span class="cv-dates">Jun 2026 — Present</span>
      </div>
      <div class="cv-role">Founder</div>
      <p class="cv-lead">Helping medium-to-large businesses deploy always available, always within budget, and always recoverable fleets of AI agents.</p>
      {%- assign agentmanagerhq_posts = site.posts | where: "company", "agentmanagerhq" -%}
      {%- if agentmanagerhq_posts.size > 0 -%}
      <details class="cv-writing" open>
        <summary>Writing from this chapter <span class="cv-writing-count">{{ agentmanagerhq_posts.size }}</span></summary>
        <ul class="cv-writing-list">
          {%- for post in agentmanagerhq_posts -%}
          <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
          {%- endfor -%}
        </ul>
      </details>
      {%- endif -%}
      <div class="cv-videos">
        <a class="cv-video-card" href="https://www.youtube.com/watch?v=5bn_Vfun-a8" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/5bn_Vfun-a8/hqdefault.jpg" alt="Agent Manager HQ demo video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">Demo — BUIDL Demo Day @ Google HK</div>
        </a>
      </div>
    </div>
  </div>

  <div class="cv-item">
    <span class="cv-dot"></span>
    <div class="cv-card">
      <div class="cv-head">
        <h3 class="cv-co">Cega</h3>
        <span class="cv-dates">Aug 2022 — Nov 2024</span>
      </div>
      <div class="cv-role">Founding Engineer · First full-time hire</div>
      <p class="cv-lead">On-chain tokenized structured option products — Fixed Coupon Notes, Dual Currency Swaps, and Shark Fin notes.</p>
      <ul class="cv-points">
        <li>Architected and shipped the v1 EVM smart contracts.</li>
        <li>Owned the backend operational and settlement layers.</li>
        <li>Sole developer of all on-chain contract indexers.</li>
        <li>Built the PnL engine for the Pendle integration — staking yields on SY tokens held in our vaults, via a sliding-window model forked from SY's on-chain yield logic.</li>
      </ul>
      <div class="cv-topics">
        <span class="cv-topic">DeFi</span>
        <span class="cv-topic">structured products</span>
        <span class="cv-topic">EVM contracts</span>
        <span class="cv-topic">on-chain indexers</span>
      </div>
      <div class="cv-stats">
        <span class="cv-stat"><b>~$45M</b> peak TVL</span>
        <span class="cv-stat"><b>0 → 1</b> built from scratch</span>
      </div>
      <div class="cv-tags"><span class="cv-tag">Acquired by BONKbot</span></div>
      {%- assign cega_posts = site.posts | where: "company", "cega" -%}
      {%- if cega_posts.size > 0 -%}
      <details class="cv-writing" open>
        <summary>Writing from this chapter <span class="cv-writing-count">{{ cega_posts.size }}</span></summary>
        <ul class="cv-writing-list">
          {%- for post in cega_posts -%}
          <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
          {%- endfor -%}
        </ul>
      </details>
      {%- endif -%}
      <div class="cv-videos">
        <a class="cv-video-card" href="https://www.youtube.com/watch?v=-2zP8ibCkvo" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/-2zP8ibCkvo/hqdefault.jpg" alt="BUIDL Vietnam 2023 panel discussion video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">Panel — BUIDL Vietnam 2023</div>
        </a>
      </div>
      <div class="cv-stack">
        TypeScript · Node.js · Solidity · Supabase · React
        <span class="cv-stack-light"><span class="label">Some exposure:</span> Rust (Solana)</span>
      </div>
    </div>
  </div>

  <div class="cv-item">
    <span class="cv-dot"></span>
    <div class="cv-card">
      <div class="cv-head">
        <h3 class="cv-co">MyPropty</h3>
        <span class="cv-dates">Dec 2017 — May 2022</span>
      </div>
      <div class="cv-role">Founder &amp; CTO</div>
      <p class="cv-lead">Property-management software for small and mid-size landlords in Hong Kong.</p>
      <ul class="cv-points">
        <li>Founded the company and ran engineering as CTO, building the product end-to-end.</li>
        <li>Drove sales and business development too — not just the technical side.</li>
        <li>Still in production today, managing ~40 properties.</li>
      </ul>
      <div class="cv-topics">
        <span class="cv-topic">proptech</span>
        <span class="cv-topic">0→1</span>
        <span class="cv-topic">sales &amp; BD</span>
      </div>
      <div class="cv-stats">
        <span class="cv-stat"><b>~$200M</b> peak property value on platform</span>
        <span class="cv-stat"><b>~40</b> properties live today</span>
      </div>
      <div class="cv-tags">
        <span class="cv-tag"><a href="https://www.cyberport.hk/en/about_cyberport/entrepreneurship/cyberport_incubation_programme" target="_blank" rel="noopener">Cyberport Incubation Programme ↗</a></span>
        <span class="cv-tag">Still running</span>
      </div>
      {%- assign mypropty_posts = site.posts | where: "company", "mypropty" -%}
      {%- if mypropty_posts.size > 0 -%}
      <details class="cv-writing" open>
        <summary>Writing from this chapter <span class="cv-writing-count">{{ mypropty_posts.size }}</span></summary>
        <ul class="cv-writing-list">
          {%- for post in mypropty_posts -%}
          <li><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></li>
          {%- endfor -%}
        </ul>
      </details>
      {%- endif -%}
      <div class="cv-videos">
        <a class="cv-video-card" href="https://www.youtube.com/watch?v=WvxhmEismCQ" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/WvxhmEismCQ/hqdefault.jpg" alt="MyPropty platform preview video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">MyPropty — Platform Preview</div>
        </a>
        <a class="cv-video-card" href="https://www.youtube.com/shorts/5ekSMAAlxhg" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/5ekSMAAlxhg/hqdefault.jpg" alt="MyPropty WhatsApp assistant video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">MyPropty — WhatsApp Assistant</div>
        </a>
        <a class="cv-video-card" href="https://www.youtube.com/watch?v=7F8-GMygQk4" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/7F8-GMygQk4/hqdefault.jpg" alt="The Cloud technology consultant video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">The CLOUD — Technology Consultant<span class="cv-video-client">for Star Properties</span></div>
        </a>
        <a class="cv-video-card" href="https://www.youtube.com/watch?v=l5symo3xkcU" target="_blank" rel="noopener">
          <div class="cv-video-thumb"><img src="https://img.youtube.com/vi/l5symo3xkcU/hqdefault.jpg" alt="The Cloud TV commercial video thumbnail" loading="lazy"></div>
          <div class="cv-video-title">The CLOUD — TV Commercial<span class="cv-video-client">for Star Properties</span></div>
        </a>
      </div>
      <div class="cv-stack">Ruby on Rails · React · Postgres</div>
    </div>
  </div>

  <div class="cv-item">
    <span class="cv-dot"></span>
    <div class="cv-card">
      <div class="cv-head">
        <h3 class="cv-co">Cisco Meraki</h3>
        <span class="cv-dates">Aug 2014 — Jan 2017</span>
      </div>
      <div class="cv-role">Security Appliance Team</div>
      <p class="cv-lead">On the MX security-appliance team, part of Meraki's cloud-managed networking platform — devices that phone home to a central dashboard for configuration, monitoring, and firmware, instead of being managed box by box.</p>
      <ul class="cv-points">
        <li>Co-invented and built an uplink-monitoring system — a sliding-window monitor that let MX owners catch uplink latency and connectivity issues from the cloud dashboard.</li>
        <li>Spanned the stack: C++ components on the device, a Rails backend, a React frontend, and Scala collectors polling connection data from fleets of deployed appliances.</li>
        <li>Also worked across firewall configuration and other app-level features.</li>
      </ul>
      <div class="cv-topics">
        <span class="cv-topic">cloud-managed infra</span>
        <span class="cv-topic">firewall</span>
        <span class="cv-topic">uplink monitoring</span>
        <span class="cv-topic">networking</span>
      </div>
      <div class="cv-tags"><span class="cv-tag"><a href="https://patents.google.com/patent/WO2016182772A1/es" target="_blank" rel="noopener">Patent WO2016182772A1 ↗</a></span></div>
      <div class="cv-stack">
        Ruby on Rails · React · Postgres
        <span class="cv-stack-light"><span class="label">Some exposure:</span> C++ · Scala</span>
      </div>
    </div>
  </div>

</div>
