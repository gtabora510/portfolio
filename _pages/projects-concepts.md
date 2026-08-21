---
layout: page
title: Concepts
permalink: /projects/concepts/
description: Explaining developer concepts.
nav: false
horizontal: false
---

<style>
  .projects .card {
    border-left: 4px solid #1f2937 !important;
  }
  .projects .card .card-title,
  .projects .card .card-text,
  .projects .card .card-body,
  .projects a,
  .post-description,
  .navbar-nav a.nav-link {
    color: #1f2937 !important;
  }

  @media (prefers-color-scheme: dark) {
    .projects .card {
      border-left-color: #f8f9fa !important;
    }
    .projects .card .card-title,
    .projects .card .card-text,
    .projects .card .card-body,
    .projects a,
    .post-description,
    .navbar-nav a.nav-link {
      color: #f8f9fa !important;
    }
  }
</style>
<div class="projects">
  <p><a href="{{ site.baseurl }}/projects/">&larr; Back to Portfolio</a></p>
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_projects = site.projects | where: "category", "concepts" | sort: "importance" %}
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
