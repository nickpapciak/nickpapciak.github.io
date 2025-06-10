---
layout: page
title: Projects
permalink: /projects/
---

<div class="projects-list">
  {% assign all_projects = site.projects | sort: "feature-order" %}
  {% for project in all_projects %}
    <div class="project-block">
      <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>

      {% if project.technologies %}
        <p><strong>Technologies:</strong> {{ project.technologies | join: ", " }}</p>
      {% endif %}

      {% if project.description %}
        <p>{{ project.description }}</p>
      {% endif %}

      {% if project.image %}
        <img src="{{ project.image }}" alt="Preview of {{ project.title }}" style="max-width: 400px;" />
      {% endif %}

      <a href="{{ project.url }}">
        <i class="fas fa-link" aria-hidden="true"></i> View Project Page
      </a>

      <hr />
    </div>
  {% endfor %}
</div>
