---
layout: page
permalink: /tags/index.html
title: 標籤 
tagline: 
tags: 
image:
  feature: texture-feature-01.jpg
---
{% for tag in site.tags %}
<div class="btn-group" style="margin:5px">

    <button type="button" class="btn btn-default">
        {{ tag[0] }}
    </button>
    <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown">
    <span class="caret"></span>
    <span class="sr-only"></span>
    </button>

    <ul class="dropdown-menu" role="menu">
        {% for post in tag[1] %}
            <li><a href="{{ post.url }}"> {{ post.title }} </a></li>
        {% endfor %}
    </ul>
</div>
{% endfor %}
