---
layout: page
permalink: /teaching/
title: Teaching
nav: true
nav_order: 4
---

Courses taught, listed by most recent term. Primarily for law students at the University of Chicago unless otherwise noted.

{% assign teaching = site.data.teaching %}

<ul class="course-list">
{% for course in teaching.courses %}
  <li>
    <span class="course-name">{{ course.name }}</span>{% if course.audience %} <span class="course-meta">({{ course.audience }})</span>{% endif %}{% if course.school %} <span class="course-meta">— {{ course.school }}</span>{% endif %} — {{ course.terms }}.
  </li>
{% endfor %}
</ul>

## Resources

- [**AI training**]({{ '/teaching/ai/' | relative_url }}) — a hands-on curriculum for getting productive with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), the terminal-based coding agent. Ten weekly sessions plus a bonus website build, free to use under a Creative Commons license.

<style>
  .course-list li {
    margin-bottom: 0.5rem;
  }
  .course-name {
    font-weight: 600;
  }
  .course-meta {
    font-size: 0.9em;
    color: var(--global-text-color-light, #828282);
  }
</style>
