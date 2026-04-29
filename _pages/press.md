---
layout: page
permalink: /press/
title: Press
nav: true
nav_order: 3
---

{% comment %}
Press items are pulled from _data/press.yml and grouped by year within each section.
Item ordering within each section comes from press.yml (manual, most-recent-first).
The year is extracted from the last 4 chars of `date` — works for any date string ending in YYYY.
{% endcomment %}

{% assign press = site.data.press %}

<nav class="press-quicklinks">
  {% if press.op_eds and press.op_eds.size > 0 %}<a href="#op-eds">Op-Eds</a>{% endif %}
  {% if press.blogs and press.blogs.size > 0 %} &middot; <a href="#blogs">Blogs and Online Articles</a>{% endif %}
  {% if press.podcasts and press.podcasts.size > 0 %} &middot; <a href="#podcasts">Podcasts</a>{% endif %}
  {% if press.interviews and press.interviews.size > 0 %} &middot; <a href="#interviews">Interviews/Profiles</a>{% endif %}
  {% if press.talks and press.talks.size > 0 %} &middot; <a href="#talks">Online Talks</a>{% endif %}
</nav>

<div class="publications">

{% if press.op_eds and press.op_eds.size > 0 %}
  <h1 id="op-eds">Op-Eds</h1>
  {% assign prev_year = "" %}
  {% for item in press.op_eds %}
    {% assign yr = item.date | slice: -4, 4 %}
    {% if yr != prev_year %}
      <h2 class="bibliography">{{ yr }}</h2>
    {% endif %}
    <div class="press-item">
      <div class="title">
        {% if item.url %}<a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>{% else %}{{ item.title }}{% endif %}
      </div>
      <div class="periodical">
        <em>{{ item.venue }}</em>{% if item.coauthors %}, with {{ item.coauthors }}{% endif %} &middot; {{ item.date }}
        {% if item.pdf %}&middot; <a href="{{ item.pdf }}" rel="external nofollow noopener" target="_blank">[PDF]</a>{% endif %}
      </div>
    </div>
    {% assign prev_year = yr %}
  {% endfor %}
{% endif %}

{% if press.blogs and press.blogs.size > 0 %}
  <h1 id="blogs">Blogs and Online Articles</h1>
  {% assign prev_year = "" %}
  {% for item in press.blogs %}
    {% assign yr = item.date | slice: -4, 4 %}
    {% if yr != prev_year %}
      <h2 class="bibliography">{{ yr }}</h2>
    {% endif %}
    <div class="press-item">
      <div class="title">
        {% if item.url %}<a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>{% else %}{{ item.title }}{% endif %}
      </div>
      <div class="periodical">
        <em>{{ item.venue }}</em>{% if item.coauthors %}, with {{ item.coauthors }}{% endif %} &middot; {{ item.date }}
      </div>
    </div>
    {% assign prev_year = yr %}
  {% endfor %}
{% endif %}

{% if press.podcasts and press.podcasts.size > 0 %}
  <h1 id="podcasts">Podcasts</h1>
  {% assign prev_year = "" %}
  {% for item in press.podcasts %}
    {% assign yr = item.date | slice: -4, 4 %}
    {% if yr != prev_year %}
      <h2 class="bibliography">{{ yr }}</h2>
    {% endif %}
    <div class="press-item">
      <div class="title">
        <a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>
      </div>
      <div class="periodical">
        <em>{{ item.venue }}</em>{% if item.host %}, hosted by {{ item.host }}{% endif %}{% if item.coauthors %} (with {{ item.coauthors }}){% endif %} &middot; {{ item.date }}
      </div>
    </div>
    {% assign prev_year = yr %}
  {% endfor %}
{% endif %}

{% if press.interviews and press.interviews.size > 0 %}
  <h1 id="interviews">Interviews/Profiles</h1>
  {% assign prev_year = "" %}
  {% for item in press.interviews %}
    {% assign yr = item.date | slice: -4, 4 %}
    {% if yr != prev_year %}
      <h2 class="bibliography">{{ yr }}</h2>
    {% endif %}
    <div class="press-item">
      <div class="title">
        {% if item.url %}<a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>{% else %}{{ item.title }}{% endif %}
      </div>
      <div class="periodical">
        <em>{{ item.venue }}</em>{% if item.interviewer %}, by {{ item.interviewer }}{% endif %} &middot; {{ item.date }}
        {% if item.pdf %}&middot; <a href="{{ item.pdf }}" rel="external nofollow noopener" target="_blank">[PDF]</a>{% endif %}
      </div>
    </div>
    {% assign prev_year = yr %}
  {% endfor %}
{% endif %}

{% if press.talks and press.talks.size > 0 %}
  <h1 id="talks">Online Talks</h1>
  {% assign prev_year = "" %}
  {% for item in press.talks %}
    {% assign yr = item.date | slice: -4, 4 %}
    {% if yr != prev_year %}
      <h2 class="bibliography">{{ yr }}</h2>
    {% endif %}
    <div class="press-item">
      <div class="title">
        <a href="{{ item.url }}" rel="external nofollow noopener" target="_blank">{{ item.title }}</a>
      </div>
      <div class="periodical">
        <em>{{ item.venue }}</em>{% if item.coauthors %} (with {{ item.coauthors }}){% endif %} &middot; {{ item.date }}
      </div>
    </div>
    {% assign prev_year = yr %}
  {% endfor %}
{% endif %}

</div>

<style>
  .press-quicklinks {
    margin-bottom: 1.5rem;
    font-size: 0.95em;
  }
  .press-item {
    margin-bottom: 1rem;
  }
  .press-item .title {
    font-weight: bold;
  }
  .press-item .periodical {
    font-size: 0.95em;
  }
</style>
