---
layout: default
title: Tags
permalink: /tags/
---

<h1>Browse by Tag</h1>

<ul>
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <li>
      <a href="/tags/{{ tag[0] | slugify }}/">{{ tag[0] }} ({{ tag[1].size }})</a>
    </li>
  {% endfor %}
</ul>
