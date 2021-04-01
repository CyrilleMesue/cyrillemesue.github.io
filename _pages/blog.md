---
layout: single
title: Blogs
permalink: /Blog/
author_profile: true
comments: true
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
