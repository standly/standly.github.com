---
layout: default
title: Welcome
permalink: /itstuff/home/
---

<ul>
{% for post in site.categories.itstuff %}
   <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
{% endfor %}  
</ul>

###Hello error !