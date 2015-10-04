---
title: Marc Duiker
---

{% include header.html %}

_Warning: this page is work in progress, for now I'm just focussing on the content. Styling will eventually follow ;)._

## Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date_to_long_string }} - {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>