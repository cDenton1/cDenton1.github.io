---
layout: default
title: Denton's Blog
---

### Highlights:

Very busy couple months ahead, but I placed first (along with every other team) at the KPMG Cyber Hackathon! You can read more about it [here](https://cdenton1.github.io/2025/10/03/KPMG-Cyber-Hackathon.html).

### Recent Posts:

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

**[See More Posts](https://cdenton1.github.io/all-posts)**
