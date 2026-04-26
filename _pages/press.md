---
layout: page
permalink: /press/
title: press
description: Op-eds, blog posts, podcasts, and online talks.
nav: true
nav_order: 3
---

{% assign press = site.data.press %}

{% if press.op_eds and press.op_eds.size > 0 %}
## Op-Eds

<ul>
{% for item in press.op_eds %}
  <li>
    <strong>{{ item.date }}</strong> &mdash;
    {% if item.url %}<a href="{{ item.url }}" target="_blank">{{ item.title }}</a>{% else %}{{ item.title }}{% endif %},
    <em>{{ item.venue }}</em>
    {% if item.coauthors %}(with {{ item.coauthors }}){% endif %}
    {% if item.pdf %} &middot; <a href="{{ item.pdf }}" target="_blank">[PDF]</a>{% endif %}
  </li>
{% endfor %}
</ul>
{% endif %}

{% if press.blogs and press.blogs.size > 0 %}
## Blogs and Online Articles

<ul>
{% for item in press.blogs %}
  <li>
    <strong>{{ item.date }}</strong> &mdash;
    {% if item.url %}<a href="{{ item.url }}" target="_blank">{{ item.title }}</a>{% else %}{{ item.title }}{% endif %},
    <em>{{ item.venue }}</em>
    {% if item.coauthors %}(with {{ item.coauthors }}){% endif %}
  </li>
{% endfor %}
</ul>
{% endif %}

{% if press.podcasts and press.podcasts.size > 0 %}
## Podcasts

<ul>
{% for item in press.podcasts %}
  <li>
    <strong>{{ item.date }}</strong> &mdash;
    <a href="{{ item.url }}" target="_blank">{{ item.title }}</a>,
    <em>{{ item.venue }}</em>
    {% if item.host %}(host: {{ item.host }}){% endif %}
  </li>
{% endfor %}
</ul>
{% endif %}

{% if press.talks and press.talks.size > 0 %}
## Online Talks

<ul>
{% for item in press.talks %}
  <li>
    <strong>{{ item.date }}</strong> &mdash;
    <a href="{{ item.url }}" target="_blank">{{ item.title }}</a>
    {% if item.venue %}, <em>{{ item.venue }}</em>{% endif %}
  </li>
{% endfor %}
</ul>
{% endif %}
