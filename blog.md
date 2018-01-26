---
layout: page
title: Blog
---

<!-- This blog serves two purposes:

1. As I continue to explore how the programming language to programming elegant software, it gives me a medium to share what I discover, communicate my progress, and teach others what I learn.
2. It provides a platform to share my experiences with a variety of topics. E.g. time and task management, navigating the educational landscape, useful software, etc.

For my posts, I upload and reference material I think is useful (articles, code snippets, etc). <a href="{{ site.baseurl }}/reference_material">This</a> is an aggregate of all of these items.
<br>

## Posts -->


<p><font color="purple"> category </font> | <font color="red"> tags </font></p>

<ol>
{% for post in site.posts %}
{% if post.hide != true %}
<li>
  <a href="{{ post.url }}">
    {{ post.title }}
  </a> | 
  {{ post.date | date: '%B %d, %Y' }}
  
  {% if post.categories.size > 0 %}
  <br>
  <em>
    <font color="purple"> <cite> {{ post.categories | join: ', ' }} </cite> </font>
  </em>
  
  {% endif %}
  
  {% if post.tags.size > 0 %}
  | <em>
    <font color="red"> {{ post.tags | join: ', ' }} </font>
  </em>
  {% endif %}
  
</li>
<br>
{% endif %}
{% endfor %}
</ol>
