---
layout: page
title: Time Travel
permalink: /archives/
icon-menu: icon-calendar
---

{% for post in site.posts %}

{% unless post.next %}
  <h1>Traveling to {{ post.date | date: '%Y' }}</h1>
{% else %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
  {% if year != nyear %}
  <h1>Traveling to {{ post.date | date: '%Y' }}</h1>
  {% endif %}
{% endunless %}

<div>{{ post.date | date:"%b %d" }} >> <a href="{{ post.url }}">{{ post.title }}</a></div>
{% endfor %}
