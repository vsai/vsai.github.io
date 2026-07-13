---
layout: page
title: Consultancy
permalink: /consultancy/
---

I work with a small number of founders and teams who want senior help turning
strategy into shipped product — especially in crypto, trading systems, and
applied AI. Below are a few ways to work together.
{% for section in site.data.consulting.sections %}
## {{ section.title }}
{% if section.blurb %}
{{ section.blurb }}
{% endif %}
{% for pkg in section.packages %}
### {{ pkg.name }}
{% if pkg.price_display and pkg.price_display != "" %}
**{{ pkg.price_display }}**
{% endif %}
{{ pkg.blurb }}

{% if pkg.payment_link and pkg.payment_link != "" -%}
[{{ pkg.cta_label }}]({{ pkg.payment_link }})
{%- else -%}
[{{ pkg.cta_label | default: "Enquire →" }}](mailto:hello@vishalsaidaswani.com?subject={{ pkg.cta_subject | default: pkg.name | replace: " ", "%20" }})
{%- endif %}
{% endfor %}
{% endfor %}

---

Not sure which fits? [Email me](mailto:hello@vishalsaidaswani.com) and we'll
figure it out.
