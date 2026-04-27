---
layout: page
permalink: /blog/
title: Blog
nav: true
nav_order: 4
description: Posts from <a href='https://anupmalani.substack.com' target='_blank'>anupmalani.substack.com</a> — Research Notes on epidemics and economic development.
---

{% assign posts = site.data.substack.posts %}

<div class="publications">

{% assign prev_year = "" %}
{% for item in posts %}
  {% assign yr = item.date | slice: -4, 4 %}
  {% if yr != prev_year %}
    <h2 class="bibliography">{{ yr }}</h2>
  {% endif %}
  <div class="press-item">
    <div class="title">
      <a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>
    </div>
    <div class="periodical">
      <em>Substack</em> &middot; {{ item.date }}
    </div>
  </div>
  {% assign prev_year = yr %}
{% endfor %}

</div>

<style>
  .press-item { margin-bottom: 1rem; }
  .press-item .title { font-weight: bold; }
  .press-item .periodical { font-size: 0.95em; }
</style>
