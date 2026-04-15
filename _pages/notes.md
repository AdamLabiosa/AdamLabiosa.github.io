---
layout: archive
title: "Notes"
permalink: /notes/
author_profile: true
---

{% include base_path %}

{% assign sorted_notes = site.notes | sort: "title" %}

{% if sorted_notes and sorted_notes.size > 0 %}
  {% for note in sorted_notes %}
    <div class="list__item">
      <article class="archive__item" itemscope itemtype="http://schema.org/CreativeWork">
        <h2 class="archive__item-title" itemprop="headline">
          <a href="{{ note.url | relative_url }}">{{ note.title }}</a>
        </h2>
        {% if note.excerpt %}
          <p>{{ note.excerpt | strip_html | truncate: 220 }}</p>
        {% endif %}
      </article>
    </div>
  {% endfor %}
{% else %}
  <p>No notes published yet. Add files in <code>_notes/</code> to populate this page.</p>
{% endif %}
