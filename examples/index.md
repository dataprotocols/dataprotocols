---
layout: default
title: Examples
---

{% assign sorted_pages = site.pages | sort:"url" %}
{% for page in sorted_pages %}
  {% if page.category == 'example' %}
  * [{{ page.title }}]({{ page.url | remove: 'index.html' }}) 
  {% endif %}
{% endfor %}

