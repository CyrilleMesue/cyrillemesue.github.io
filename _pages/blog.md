---
layout: single
title: Blogs
permalink: /Blog/
author_profile: true
comments: true
---

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
