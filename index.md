---
layout: default
title: Denton's Blog
---

# Welcome to my blog!

**Exciting News:** In June I'll be flying out to Ottawa to compete at CyberSci Nationals! I attended CyberSci Regionals back in November, where my team and I placed third, and which led to me being invited to apply for a spot on the Junior Team. If you're interested, checkout my post about my experience at regionals, and I'll make sure to post an update about Nationals once I'm back! - [CyberSci Regionals 2024](https://cdenton1.github.io/2024/11/30/CyberSci-Regionals-24.html)

Recent Posts:

{% assign visible_posts = "" | split: "" %}
{% for post in site.posts %}
  {% unless post.tags contains "hidden-1" %}
    {% assign visible_posts = visible_posts | push: post %}
  {% endunless %}
{% endfor %}

<ul>
  {% for post in visible_posts limit:5 %}
    <li class="post-preview">
      <div>
        <a href="{{ post.url }}">{{ post.title }}</a><br>
        <small>
          {{ post.date | date: "%B %d, %Y" }}{% if post.read_time %} - {{ post.read_time}}{% endif %}
        </small> <br>
      </div>
    </li>
  {% endfor %}
</ul>

