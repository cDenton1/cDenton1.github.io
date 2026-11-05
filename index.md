---
layout: default
title: Denton's Blog
---

**Updates:** Making some small changes to various parts of the site and removing older posts if I don't feel they fit my purpose of this site anymore. Added a new game in the meantime: [Card Matching](https://cdenton1.github.io/card-matching).

### Highlighted Posts

[Building a Honeypot](https://cdenton1.github.io/2026/02/18/Building-a-Honeypot.html) - Built a modified evil twin honeypot on a Raspberry Pi Pico W for my own learning and to further test my Python abilities.

[CyberSci Nationals 2025](https://cdenton1.github.io/2025/06/19/CyberSci-Nationals-25.html) - Had the opportunity to compete amongst the top 45 cybersecurity students from across the country in Ottawa.

[Bring Back Old Tech](https://cdenton1.github.io/2026/10/17/Bring-Back-Old-Tech.html) - Passion project of mine, breaking out and reusing/repurposing older technology to avoid it from going to waste and limit my phone usage.

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
