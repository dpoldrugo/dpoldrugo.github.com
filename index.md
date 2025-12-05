---
layout: page
title: Home
header: Latest Posts
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <li>
      <span>{{ post.date | date_to_string }}</span> &raquo;
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

<p>
  <a href="/posts.html">View all posts...</a>
</p>