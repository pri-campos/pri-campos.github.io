---
layout: home
title: "Pri Campos"
---

Artigos técnicos e notas.

## Artigos

{% for post in site.posts %}
- **{{ post.date | date: "%Y-%m-%d" }}** — [{{ post.title }}]({{ post.url }})
{% endfor %}
