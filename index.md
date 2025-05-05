---
layout: default
title: "Home"
---

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% assign categories = "" | split: "" %}

{%- for post in sorted_posts -%}
  {% for category in post.categories %}
    {% unless categories contains category %}
      {% assign categories = categories | push: category %}
    {% endunless %}
  {% endfor %}
{%- endfor -%}

{% for category in categories %}
  <h3>{{ category | capitalize }}</h3>
  <ul>
    {% for post in sorted_posts %}
      {% if post.categories contains category %}
        <li>
          <a href="{{ post.url }}">{{ post.title }}</a>
          ({{ post.date | date: "%Y-%m-%d" }})
        </li>
      {% endif %}
    {% endfor %}
  </ul>
{% endfor %}
