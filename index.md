---
layout: default
title: Denton's Blog
---

### Highlight:

I've started on a new project recently, a **modular command line decrypting tool**. To get a sneak peek, check out my GitHub repository, [Decrypter](https://github.com/cDenton1/Decrypter) and keep an eye out for my related post.

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
