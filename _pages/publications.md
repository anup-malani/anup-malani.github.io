---
layout: page
permalink: /publications/
title: Publications
nav: true
nav_order: 2
---

{% include bib_search.liquid %}

<nav class="press-quicklinks">
  <a href="#published">Published</a> &middot; <a href="#working-papers">Working Papers</a>
</nav>

<div class="publications">

<h1 id="published">Published</h1>

{% bibliography -q @*[category!=working-papers] %}

<h1 id="working-papers">Working Papers</h1>

{% bibliography -q @*[category=working-papers] %}

</div>
