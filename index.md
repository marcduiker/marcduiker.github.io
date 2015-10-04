---
title: Marc Duiker
---

{% include header.html %}

_Warning: this page and it's subpages are heavily under construction. For now I'm just focussing on content (blog posts). Some sort of styling will eventually follow ;)._

## Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date_to_long_string }} - {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>