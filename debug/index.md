---
layout: page
title: Debug
---

Here are the things I have tried, hope they are useful to you too.

{% for post in site.categories.debug %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.excerpt | strip_html | truncatewords: 20 }} ({{ post.date | date: "%B %d, %Y" }})
{% endfor %}
