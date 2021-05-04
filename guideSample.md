---
layout: default
title: Testing How-To
---
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <p>{{ post.description }}</p>
      <p>By: {{ post.author }}</p>
    </li>
  {% endfor %}
</ul>
