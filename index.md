---
layout: page
title: Hello World!
categories: [Firstpost]
tagline: Supporting tagline
---
{% include JB/setup %}

Welcome 
to Gopi Krishnan Nambiar's homepage!
This page is currently under construction, but feel free to browse through.. Most of it already works :=)

## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

