---
layout: page
title: Administration
permalink: /projects/administration/
description: Administering developer tools.
nav: false
horizontal: false
---

<style>
  .projects .card {
    border-left: 4px solid var(--global-text-color, #222222);
  }
  .projects .card .card-title,
  .projects .card .card-text,
  .post-description,
  .navbar-nav a.nav-link {
    color: var(--global-text-color, #222222);
  }

  @media (prefers-color-scheme: dark) {
    .projects .card {
      border-left-color: #f8f9fa;
    }
    .projects .card .card-title,
    .projects .card .card-text,
    .post-description,
    .navbar-nav a.nav-link {
      color: #f8f9fa;
    }
  }
</style>
<div class="projects">
  <p><a href="{{ site.baseurl }}/projects/">&larr; Back to Portfolio</a></p>
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_projects = site.projects | where: "category", "administration" | sort: "importance" %}
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
