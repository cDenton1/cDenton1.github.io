---
layout: default
title: Denton's Blog
---

### Highlights:

Competed solo at the **AL1C3 1N PWN3RLAND** CTF hosted by SAIT on July 26th, and I placed **2nd**!

Released v2.0 of my modular command-line decryption tool, **Decrypter**. Read more about it [here](https://cdenton1.github.io/2025/07/10/Decrypter.html) on my blog, or over on [GitHub](https://github.com/cDenton1/Decrypter).

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
