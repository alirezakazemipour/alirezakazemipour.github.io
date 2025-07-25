---
layout: archive
title: "Unfinished ideas"
permalink: /unfin/
author_profile: true
---

{% include base_path %}

{% for post in site.unfin reversed %}
  {% include archive-single.html %}
{% endfor %}
