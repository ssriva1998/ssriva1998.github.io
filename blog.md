---
title: "Personal Blog"
---

### About

Some random thoughts I guess.

### Posts

{% for tag in site.tags %}
  {% if tag[0] == "personal" %}
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
  {% endif %}
{% endfor %}