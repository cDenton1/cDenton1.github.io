---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

Recent Posts:
<ul>
  {% for post in site.posts limit:5 %}
  <li style="margin-bottom: 1.5rem;">
    {% if post.image %}
      <img src="{{ post.image }}" alt="{{ post.title }}" Style="width: 100px; height: auto; float: left; margin-right: 1rem;" />
    {% endif %}
    <a href="{{ post.url }}" style="font-weight: bold;">{{ post.title }}</a><br>
    <small>
      {{ post.date | date: "%B %d, %Y" }}{% if post.read_time %} * {{ post.read_time}}{% endif %}
    </small> <br>
    <div style="clear: both;"></div>
  </li>
  {% endfor %}
</ul>
