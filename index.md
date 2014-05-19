---
layout: page
title: Hello World!
categories: ['helloworld']
tags: ['firstpost']
tagline: Supporting tagline
---
{% include JB/setup %}

Welcome to Gopi Krishnan Nambiar's homepage!

This page is currently under construction, but feel free to browse through.. Most of it already works :=)
<a href:"mailto:gopikrishnan.nambiar@gmail.com">Drop me a note</a> if you find something interesting here. Contact details below..

## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

