---
layout: default
title: ZmLiu's Blog
---
{% assign first_post = site.posts.first %}
<div id="post">
<h1> 
<a href = "{{ first_post.url }}">
{{ first_post.title }}
</a>
</h1>

<div class="authoring">
{{ first_post.date | date: "%m / %e, %Y" }}
</div>
<br>
{{ first_post.content }}
</div>

<br>
<h1> Newest 10 Posts </h1>
<ul class="posts">
  {% for post in site.posts limit:10 %}
  <li><span class="post_date">{{ post.date | date: "%m / %e, %Y" }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>