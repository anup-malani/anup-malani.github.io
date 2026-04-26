---
layout: page
permalink: /publications/
title: publications
description: Publications grouped by topic; chronological order within each section. Working papers and unpublished work appear at the bottom.
nav: true
nav_order: 2
---

{% include bib_search.liquid %}

<div class="publications">

<h2 class="bibliography">Peer-Reviewed Publications</h2>

<h3 class="year">Health</h3>
{% bibliography --query "@*[category=peer-reviewed-health]" %}

<h3 class="year">Statistics</h3>
{% bibliography --query "@*[category=peer-reviewed-statistics]" %}

<h3 class="year">Economic Growth and Development</h3>
{% bibliography --query "@*[category=peer-reviewed-econ-growth-dev]" %}

<h3 class="year">Blockchain</h3>
{% bibliography --query "@*[category=peer-reviewed-blockchain]" %}

<h3 class="year">Law and Regulation</h3>
{% bibliography --query "@*[category=peer-reviewed-law-reg]" %}

<h2 class="bibliography">Other Publications</h2>

<h3 class="year">Journal Articles</h3>
{% bibliography --query "@*[category=other-articles]" %}

<h3 class="year">Books and Book Chapters</h3>
{% bibliography --query "@*[category=books]" %}

<h3 class="year">Conference Proceedings</h3>
{% bibliography --query "@*[category=conf-proceedings]" %}

<h3 class="year">Reports and Other Publications</h3>
{% bibliography --query "@*[category=reports]" %}

<h2 class="bibliography">Legal Briefs</h2>
{% bibliography --query "@*[category=legal-briefs]" %}

<h2 class="bibliography">Working Papers</h2>
{% bibliography --query "@*[category=working-papers]" %}

</div>
