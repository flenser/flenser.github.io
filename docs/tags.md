---
title: Tag Cloud
nav_order: 1
---

{% assign all_tags = "" | split: "" %}
{% for p in site.pages %}
  {% if p.tags %}
    {% assign all_tags = all_tags | concat: p.tags %}
  {% endif %}
{% endfor %}
{% assign unique_tags = all_tags | uniq %}

{% assign sortable = "" | split: "" %}
{% assign max_count = 0 %}
{% for tag in unique_tags %}
  {% assign count = 0 %}
  {% for p in site.pages %}
    {% if p.tags contains tag %}
      {% assign count = count | plus: 1 %}
    {% endif %}
  {% endfor %}
  {% if count > max_count %}{% assign max_count = count %}{% endif %}
  {% assign padded = "000" | append: count | slice: -3, 3 %}
  {% assign key = padded | append: "###" | append: tag %}
  {% assign key_arr = key | split: "@@@NEVER@@@" %}
  {% assign sortable = sortable | concat: key_arr %}
{% endfor %}

{% assign sorted = sortable | sort | reverse %}

<div class="tag-cloud" style="margin: 1.5rem 0; line-height: 2.4;">
{% for item in sorted %}
  {% assign parts = item | split: "###" %}
  {% assign count = parts[0] | plus: 0 %}
  {% assign tag = parts[1] %}
  {% assign count_f = count | times: 1.0 %}
  {% assign ratio = count_f | divided_by: max_count %}
  {% assign font_size = ratio | times: 1.4 | plus: 0.9 %}
  {% assign hue_offset = ratio | times: 220 %}
  {% assign hue = 220 | minus: hue_offset %}
  <a href="#{{ tag | slugify }}" style="display:inline-block; margin: 0 0.6rem; font-size: {{ font_size }}rem; color: hsl({{ hue }}, 75%, 45%); font-weight: 600; text-decoration: none;">{{ tag }}</a>
{% endfor %}
</div>

{% for item in sorted %}
  {% assign parts = item | split: "###" %}
  {% assign count = parts[0] | plus: 0 %}
  {% assign tag = parts[1] %}

## {{ tag }} <span style="font-size: 0.7em; color: #888;">({{ count }})</span>
{: #{{ tag | slugify }} }

{% for p in site.pages %}
  {% if p.tags contains tag %}
- [{{ p.title }}]({{ p.url }})
  {% endif %}
{% endfor %}

{% endfor %}
