---
layout: page
title: portfolio
permalink: /projects/
description: A sampling of the technical documentation, API references, and content style guidance I've produced over 20-plus years working across developer tools, enterprise platforms, and internal engineering teams.
nav: true
nav_order: 3
categories:
  - slug: administration
    title: Administration
    description: Administering developer tools.
  - slug: concepts
    title: Concepts
    description: Explaining developer concepts.
  - slug: configuration
    title: Configuration
    description: Configuring developer tools.
  - slug: developer
    title: Developer
    description: Referencing and integrating developer APIs.
  - slug: installation
    title: Installation
    description: Installing developer tools.
  - slug: style
    title: Style
    description: Content and writing style guidance.
---

<!-- pages/projects.md -->
<style>
  .projects .card {
    border-left: 4px solid #1f2937 !important;
  }
  .projects .card .card-title,
  .projects .card .card-text,
  .projects .card .card-body,
  .projects a,
  .post-title,
  .post-description,
  .navbar-nav a.nav-link {
    color: #1f2937 !important;
  }

  html[data-theme="dark"] .projects .card {
    border-left-color: #f8f9fa !important;
  }
  html[data-theme="dark"] .projects .card .card-title,
  html[data-theme="dark"] .projects .card .card-text,
  html[data-theme="dark"] .projects .card .card-body,
  html[data-theme="dark"] .projects a,
  html[data-theme="dark"] .post-title,
  html[data-theme="dark"] .post-description,
  html[data-theme="dark"] .navbar-nav a.nav-link {
    color: #f8f9fa !important;
  }
</style>
<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
    {% for cat in page.categories %}
    <div class="col mb-4">
      <a href="{{ site.baseurl }}/projects/{{ cat.slug }}/" class="text-decoration-none">
        <div class="card h-100">
          <div class="card-body">
            <h3 class="card-title">{{ cat.title }}</h3>
            <p class="card-text">{{ cat.description }}</p>
          </div>
        </div>
      </a>
    </div>
    {% endfor %}
  </div>
</div>
