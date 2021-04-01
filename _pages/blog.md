---
layout: single
title: Blogs
permalink: /Blog/
author_profile: true
comments: true
---

{% for post in site.posts %}
  <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}
