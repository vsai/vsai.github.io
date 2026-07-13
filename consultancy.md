---
layout: page
title: Consultancy
permalink: /consultancy/
---

I work with a small number of founders and teams who want senior help turning
strategy into shipped product — especially in crypto, trading systems, and
applied AI. Below are a few ways to work together.

{% for section in site.data.consulting.sections %}
<section class="consulting-section">
  <h2>{{ section.title }}</h2>
  {% if section.blurb %}<p class="consulting-section-blurb">{{ section.blurb }}</p>{% endif %}
  <div class="consulting-grid">
    {% for pkg in section.packages %}
    <div class="consulting-card">
      <h3>{{ pkg.name }}</h3>
      {% if pkg.price_display and pkg.price_display != "" %}<p class="consulting-price">{{ pkg.price_display }}</p>{% endif %}
      <p class="consulting-blurb">{{ pkg.blurb }}</p>
      {% if pkg.payment_link and pkg.payment_link != "" -%}
      <a class="consulting-cta" href="{{ pkg.payment_link }}">{{ pkg.cta_label }}</a>
      {%- else -%}
      <a class="consulting-cta" href="mailto:hello@vishalsaidaswani.com?subject={{ pkg.cta_subject | default: pkg.name | replace: " ", "%20" }}">{{ pkg.cta_label | default: "Enquire →" }}</a>
      {%- endif %}
    </div>
    {% endfor %}
  </div>
</section>
{% endfor %}

<p class="consulting-footer-note">Not sure which fits? <a href="mailto:hello@vishalsaidaswani.com">Email me</a> and we'll figure it out.</p>
