---
layout: page
title: 
categories: ['helloworld']
tags: ['firstpost']
tagline: Supporting tagline
---
{% include JB/setup %}

Hello!

I'm Gopi Krishnan Nambiar and I work at Salesforce in their Big Data team. I completed my Masters in Computer Science (Machine Learning + Big Data focus) at Georgia Institute of Technology in 2016. Last summer, as an intern I worked on buildng a few Machine Learning models for fraud detection. Prior to joining Georgia Tech, I worked on the txtWeb Platform at Intuit India as a Senior Software Engineer. My interests lie Machine Learning and utilizing big data to create a huge impact. Feel free to drop me a note at gnambiar3[at]gatech[dot]edu. 

## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

