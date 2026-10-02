---
title: '* Authors'
nav_order: 2
---

{% assign author_pages = site.pages | where_exp: "page", "page.author_ja" %}
{% assign authors = author_pages | group_by_exp: "page", "page.author_ja" | sort: "name" %}

{% for author in authors %}
  {% assign novels = author.items | sort: "title" %}
  {% assign romanized_name = "" %}
  {% for novel in novels %}
    {% if novel.author_romaji %}
      {% assign romanized_name = novel.author_romaji %}
      {% break %}
    {% endif %}
  {% endfor %}

## {% if romanized_name != "" %}{{ romanized_name }} ({{ author.name }}){% else %}{{ author.name }}{% endif %}

{% for novel in novels %}
- [{{ novel.title }}]({{ novel.url | relative_url }})
{% endfor %}

{% endfor %}
