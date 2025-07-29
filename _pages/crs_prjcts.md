---
layout: archive
title: "Course projects"
permalink: /crs_prjcts/
author_profile: true
---

{% include base_path %}

{% for post in site.crs_prjcts reversed %}
  {% include archive-single.html %}
{% endfor %}