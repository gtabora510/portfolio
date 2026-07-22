---
layout: page
title: Style
permalink: /projects/style/
description: Content and writing style guidance.
nav: false
horizontal: false
---

<div class="projects">
  <p><a href="{{ site.baseurl }}/projects/">&larr; Back to Portfolio</a></p>
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_projects = site.projects | where: "category", "style" | sort: "importance" %}
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
