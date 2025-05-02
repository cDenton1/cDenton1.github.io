---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

Recent Posts:
<ul>
  {% for post in site.posts limit:5 %}
  <li class="post-preview">
    {% if post.image %}
      <img src="{{ post.image }}" alt="{{ post.title }}" />
    {% endif %}
    <div>
      <a href="{{ post.url }}">{{ post.title }}</a> <br>
      <small>
        {{ post.date | date: "%B %d, %Y" }}{% if post.read_time %} - {{ post.read_time}}{% endif %}
      </small> <br>
    </div>
  </li>
  {% endfor %}
</ul>
