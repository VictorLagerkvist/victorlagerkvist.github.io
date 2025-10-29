---
layout: page
permalink: /publications/
title: Publications
description: Journal articles and conference papers.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->
{% include bib_search.liquid %}

<div class="publications">

<h2> Preprints </h2>
{% bibliography --query @*[eprinttype=arXiv] %}

<hr style="margin: 3em 0;">

<h2> Journal and Conference Papers </h2>
{% bibliography --query @*[eprinttype!=arXiv] %}

</div>

