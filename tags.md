---
layout: page
title: Tags
permalink: /tags/
---

{% for tag in site.tags %}{% assign tagname = tag[0] %}[#{{ tagname }}](#{{ tagname }}) {% endfor %}

{% for tag in site.tags %}
<details>
<summary>{{ tag[0] }}</summary>
{{ tag[1] | size }} posts
  {% for post in tag[1] %}
 - [{{ post.title }}]({% include relative %}{{ post.url }})
  {% endfor %}
{% endfor %}
</details>
