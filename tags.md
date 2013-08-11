---
layout: page
permalink: /tags/index.html
title: 標籤 
tagline: 
tags: 
image:
  feature: texture-feature-01.jpg
---

<div id='tag_cloud'>
{% for tag in site.tags %}
<a href="#{{ tag[0] }}" title="{{ tag[0] }}" rel="{{ tag[1].size }}">{{ tag[0] }}</a>
{% endfor %}
</div>

<ul id='tag_list'>
{% for tag in site.tags %}
  <li class='tag_item' id='{{ tag[0] }}'>
    <span class='tag_name'>{{ tag[0] }}</span>
    <span>
      <ul>
      {% for post in tag[1] %}
        <li class='tag_post'><a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></li>
      {% endfor %}
      </ul>
    </span>
  </li>
{% endfor %}
</ul>