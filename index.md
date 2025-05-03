---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

**NOTICE:** Read the first blog post listed in recent for context about why the website looks and works the way it does currently

Recent Posts:
<ul>
  {% for post in site.posts limit:5 %}
  <li class="post-preview">
    <div>
      <a href="{{ post.url }}">{{ post.title }}</a> <br>
      <small>
        {{ post.date | date: "%B %d, %Y" }}{% if post.read_time %} - {{ post.read_time}}{% endif %}
      </small> <br>
    </div>
  </li>
  {% endfor %}
</ul>
