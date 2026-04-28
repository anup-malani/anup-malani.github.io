---
layout: page
permalink: /publications/
title: Publications
description: Publications in reverse chronological order.
nav: true
nav_order: 2
---

{% include bib_search.liquid %}

<div class="publications" markdown="1">

## Published

{% bibliography -q @*[category!=working-papers] %}

## Working Papers

{% bibliography -q @*[category=working-papers] %}

</div>
