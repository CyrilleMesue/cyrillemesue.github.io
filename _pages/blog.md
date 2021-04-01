---
layout: archive
title: Blogs
permalink: /Blog/
author_profile: true
comments: true
---
 This is my blog page

{% for post in site.posts %}
  {% include archive-single.html %}
{% endfor %}
