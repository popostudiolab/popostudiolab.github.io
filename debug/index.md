---
layout: page
title: POPO 1
---
# Work in the Public Progress

Here are the things I have tried working with AI to shape the project, hope they are useful to you too.

{% for post in site.debug | reverse %}
- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.excerpt | strip_html | truncatewords: 20 }} ({{ post.date | date: "%B %d, %Y" }})
{% endfor %}
