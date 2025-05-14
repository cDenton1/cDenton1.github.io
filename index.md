---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

**NOTICE:** Read this blog post for context about why the website looks and works the way it does currently - [link]

Recent Posts:

{% assign visible_posts = "" | split: "" %}
{% for post in site.posts %}
  {% unless post.tags contains "hidden-1" or post.tags contains "hidden-2" or post.tags contains "hidden-3" %}
    {% assign visible_posts = visible_posts | push: post %}
  {% endunless %}
{% endfor %}

<ul>
  {% for post in visible_posts limit:5 %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a><br>
      <small>{{ post.date | date: "%B %d, %Y" }}</small>
    </li>
  {% endfor %}
</ul>

