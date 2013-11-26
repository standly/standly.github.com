---
layout: default
title: Welcome
permalink: /itstuff/home/
---
{% assign items = site.categories.itstuff %}

<ul>
{% for item in items %}
  {% assign item_url = item | prepend:'/lifestuff/' | append:'/' %}

  {% if item_url == page.url %}
    {% assign c = 'current' %}
  {% else %}
    {% assign c = '' %}
  {% endif %}

  {% for p in site.pages %}
    {% if p.url == item_url %}
      <li class="{{ c }}"><a href="{{ site.url }}{{ p.url }}">{{ p.title }}</a></li>
    {% endif %}
  {% endfor %}

{% endfor %}  
</ul>
