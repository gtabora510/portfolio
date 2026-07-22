---
layout: page
title: Installation
permalink: /projects/installation/
description: Installing developer tools.
nav: false
horizontal: false
---

<style>
  .projects .card {
    border-left: 4px solid #222222;
  }
  .projects .card .card-title {
    color: #222222;
  }
  .projects .card .card-text {
    color: #222222;
  }
  .post-description {
    color: #222222;
  }
  .navbar-nav a.nav-link {
    color: #222222;
  }
</style>
<div class="projects">
  <p><a href="{{ site.baseurl }}/projects/">&larr; Back to Portfolio</a></p>
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_projects = site.projects | where: "category", "installation" | sort: "importance" %}
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
