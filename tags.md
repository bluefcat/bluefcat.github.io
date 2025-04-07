---
layout: page
title: Tags
permalink: /tags/
---

{% for tag in site.tags %}{% assign tagname = tag[0] %}[#{{ tagname }}](#{{ tagname }}) {% endfor %}

{% for tag in site.tags %}
<details>
<summary>{{ tag[0] }}</summary>
<p>{{ tag[1] | size }} posts</p>
  {% for post in tag[1] %}
    <a href={% include relative %}{{ post.url }}>{{ post.title }}</a>
  {% endfor %}
</details>
{% endfor %}
