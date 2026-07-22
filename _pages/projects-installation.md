---
layout: page
title: Installation
permalink: /projects/installation/
description: Installing developer tools.
nav: false
horizontal: false
---

<div class="projects">
  <p><a href="{{ site.baseurl }}/projects/">&larr; Back to Portfolio</a></p>
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_projects = site.projects | where: "category", "installation" | sort: "importance" %}
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
