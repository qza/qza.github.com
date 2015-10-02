---
layout: page
title: All posts
tagline: Welcome note
---
{% include JB/setup %}

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

