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

I work for Meta as a Machine Learning Engineer in the Stories Ranking team, with a focus on Notifications, Stories and Messaging experiences. Earlier I used to work for Salesforce’s Einstein.ai group building Deep Learning systems and have also dabbled with high volume data ingestion pipelines. I completed my Masters in Computer Science (Machine Learning) at Georgia Tech, where I was the head TA for Data and Visual Analytics (CSE 6242) under Prof. Polo Chau, Graduate Research Assistant at the Tenenbaum Institute working on an ed-tech UAV simulator application advised by Prof. Doug Bodner and worked on 3D modeling simulations advised by Prof. Ling Liu.


## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

