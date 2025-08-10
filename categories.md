---
layout: default
title: Categories
permalink: /categories/
---

# Categories

{% assign cats = site.categories | sort %}
<ul>
  {% for cat in cats %}
    <li><a href="#{{ cat[0] | slugify }}">{{ cat[0] }} ({{ cat[1].size }})</a></li>
  {% endfor %}
</ul>

{% for cat in cats %}
  <h2 id="{{ cat[0] | slugify }}">{{ cat[0] }}</h2>
  <ul>
    {% for post in cat[1] %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}


