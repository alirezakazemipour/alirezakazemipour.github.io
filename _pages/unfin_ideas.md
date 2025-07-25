---
layout: archive
title: "Unfinished ideas"
permalink: /unfin_ideas/
author_profile: true
---

{% include base_path %}

{% for post in site.unfin_ideas reversed %}
  {% include archive-single.html %}
{% endfor %}
