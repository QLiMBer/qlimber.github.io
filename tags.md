---
layout: default
title: Tags
permalink: /tags/
---

# Tags

{% assign tags = site.tags | sort %}
<ul>
  {% for tag in tags %}
    <li><a href="#{{ tag[0] | slugify }}">{{ tag[0] }} ({{ tag[1].size }})</a></li>
  {% endfor %}
</ul>

{% for tag in tags %}
  <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
  <ul>
    {% for post in tag[1] %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}


