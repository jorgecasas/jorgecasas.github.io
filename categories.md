---
layout: page
title: Tags
permalink: /categories/
icon-menu: icon-bookmark
---

<ul>
{% for tag in site.tags %}		
	<li><a href="/tag/{{ tag[0] }}">{{ tag[0] }}</a></li>
{% endfor %}
</ul>