---
layout: page
permalink: /teaching/
title: Teaching
nav: true
nav_order: 4
---

A list of courses taught, by school. Within each school, courses are listed by most recent term.

{% assign teaching = site.data.teaching %}

<div class="teaching">

{% for school in teaching.schools %}
<h2 id="{{ school.name | slugify }}">{{ school.name }}</h2>

{% if school.note %}<p class="school-note"><em>{{ school.note }}</em></p>{% endif %}

<ul class="course-list">
{% for course in school.courses %}
  <li>
    <span class="course-name">{{ course.name }}</span>{% if course.audience %} <span class="course-audience">({{ course.audience }})</span>{% endif %} — {{ course.terms }}.
  </li>
{% endfor %}
</ul>

{% endfor %}

</div>

## Resources

- [**AI training**]({{ '/teaching/ai/' | relative_url }}) — a hands-on curriculum for getting productive with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), the terminal-based coding agent. Ten weekly sessions plus a bonus website build, free to use under a Creative Commons license.

<style>
  .teaching .course-list {
    margin-bottom: 1.5rem;
  }
  .teaching .course-list li {
    margin-bottom: 0.5rem;
  }
  .teaching .course-name {
    font-weight: 600;
  }
  .teaching .course-audience {
    font-size: 0.9em;
    color: var(--global-text-color-light, #828282);
  }
  .teaching .school-note {
    font-size: 0.95em;
    margin-top: -0.5rem;
    margin-bottom: 1rem;
  }
</style>
