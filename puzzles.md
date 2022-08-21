---
title: "Puzzle Blog"
---

### About

I've always wanted to have a blog where I put solved puzzles. I guess I'll try.

### Puzzles

<ul>
  {% for puzzle in site.categories.puzzles %}
    <li><a href="{{ puzzle.url }}">{{ puzzle.title }}</a></li>
  {% endfor %}
</ul>

<!-- {% for tag in site.tags %}
  {% if tag[0] == "puzzle" %}
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
  {% endif %}
{% endfor %} -->
