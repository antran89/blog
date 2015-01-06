---
layout: archive
title: "Research"
date: 2014-05-30T11:40:45-08:00
modified:
excerpt: "An archive of my research thoughts, progresses and codes"
tags: []
image:
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.research %}
  {% include post-list.html %}
{% endfor %}
</div><!-- /.tiles -->
