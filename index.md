---
layout: page
title: 
categories: ['helloworld']
tags: ['firstpost']
tagline: Supporting tagline
---
{% include JB/setup %}

Hello!

I'm Gopi Krishnan, a first year Masters in Computer Science student at Georgia Institute of Technology. Prior to joining Georgia Tech, I worked on the txtWeb Platform as a Senior Software Engineer. My interests are Machine Learning and programming. Feel free to drop me a note anytime at gnambiar3[at]gatech[dot]edu

## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

