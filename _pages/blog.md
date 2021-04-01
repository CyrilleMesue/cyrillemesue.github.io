---
layout: archive
title: Blogs
permalink: /Blog/
author_profile: true
comments: true
---
 This is my blog page

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
