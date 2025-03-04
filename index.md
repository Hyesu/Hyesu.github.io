---
layout: default
title: "Home"
---

<h2>Latest Posts</h2>
<ul>
  {% for post in site.posts limit:10 %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date: "%Y-%m-%d" }})
    </li>
  {% endfor %}
</ul>
