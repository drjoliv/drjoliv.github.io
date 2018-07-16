---
layout: page
title: Jo Jo's First Blog
tagline: Supporting tagline
---
{% include JB/setup %}

# <img src="/image/me.jpeg" class="img-circle" height="80" width="80"> Jo Jo's First Blog

## About Me

My name is Desonte Jolivet I'm a veteran and  I've recently obtained my undergraduate degree in computer science. I've taken a large interest in functional programming and category theory. If I'm not programming in Haskell I'm trying to figure out how to bring the functional paradigm to Java. Recently I've been doing more of the latter by exploring monads in Java.

## Objective

This blog is mostly for me and anyone else who may stumble upon it in the interwebs so, for now, there is no specific audience. Posts will normally be a brain dump of any ideas that wander around in my head. I will also host any long-term documentation for any projects I work on here.

## Recent Posts
<ul class="posts">
  {% for post in site.posts %}
      {% if post.categories contains "short"%}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
  {% endfor %}
</ul>
