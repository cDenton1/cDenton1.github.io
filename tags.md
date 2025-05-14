---
layout: default
title: Tags
permalink: /tags/
---

<h1>Browse by Tag</h1>

<ul>
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    {% assign visible_posts = "" | split: "" %}

    {% for post in tag[1] %}
      {% unless post.tags contains "hidden-3" %}
        {% assign visible_posts = visible_posts | push: post %}
      {% endunless %}
    {% endfor %}

    {% if visible_posts.size > 0 %}
      <li>
        <a href="/tags/{{ tag[0] | slugify }}/">{{ tag[0] }} ({{ visible_posts.size }})</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>
