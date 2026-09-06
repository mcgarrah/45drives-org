---
title: Technology
description: Every open-source component of the stack behind 45Drives storage, explained on its own terms.
permalink: /technology/
---

# The open-source stack, piece by piece

45Drives' storage servers are built entirely on established, independently-maintained
open-source projects. This section covers each piece on its own: what it is, why it's used,
and where to see the real code.

<div class="grid">
{% assign sorted_pages = site.technology | sort: "title" %}
{% for item in sorted_pages %}
  <div class="card">
    <h3><a href="{{ item.url | relative_url }}">{{ item.title }}</a></h3>
    <p>{{ item.description }}</p>
  </div>
{% endfor %}
</div>

New pieces of the stack get added here as they're documented — this list grows automatically.
