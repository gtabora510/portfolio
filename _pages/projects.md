---
layout: page
title: portfolio
permalink: /projects/
description: A collection of technical documentation, API references, and style guidance.
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
