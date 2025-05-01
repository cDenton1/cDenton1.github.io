---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

Recent Posts:
<ul>
  {% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
  </li>
  {% endfor %}
</ul>
