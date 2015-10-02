---
layout: page
title: Introduction
tagline: Welcome note
---
{% include JB/setup %}

<div class="blog-index">  
  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  {% include last_post_detail.html %}
</div>

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

