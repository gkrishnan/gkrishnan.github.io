---
layout: page
title: 
categories: ['helloworld']
tags: ['firstpost']
tagline: Supporting tagline
---
{% include JB/setup %}

Hello!

I'm Gopi Krishnan Nambiar, an engineer interested in Machine Learning and Distributed Systems.
 
I work at Salesforce's Big Data team and help build the applog ingestion and processing pipeline. Earlier, I worked for Intuit Bangalore on a text application platform - txtWeb.

I completed my Masters in CS (ML+Big Data) at Georgia Tech, during which I TA'ed the course Data and Visual Analytics under Prof. Polo Chau. I also dabbled with fraud detection models during my summer internship with the Security Analytics team at Salesforce. 

gk[at]gatech[dot]edu

## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

