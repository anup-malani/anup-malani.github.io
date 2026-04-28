---
layout: page
permalink: /publications/
title: Publications
description: Publications in reverse chronological order.
nav: true
nav_order: 2
---

{% include bib_search.liquid %}

<div class="publications">

<h1 id="published">Published</h1>

{% bibliography -q @*[category!=working-papers] %}

<h1 id="working-papers">Working Papers</h1>

{% bibliography -q @*[category=working-papers] %}

</div>
