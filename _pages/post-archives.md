---
title: "Posts"
layout: archive
author_profile: true
permalink: /posts.html
---

{% for post in paginator.posts %}
  {% include archive-single.html %}
{% endfor %}
