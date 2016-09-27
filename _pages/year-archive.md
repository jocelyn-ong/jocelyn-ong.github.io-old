---
layout: archive
title: "All posts"
permalink: /page-archive/
author_profile: false
---

{% include base_path %}
{% for post in site.pages %}
  {% include archive-single.html %}
{% endfor %}