---
layout: archive
title: "Notes"
permalink: /notes/
author_profile: true
---

{% include base_path %}

For interesting tutorials and notes for myself.


{% assign dated_notes = site.notes | where_exp: "note", "note.date" | sort: "date" | reverse %}
{% assign undated_notes = site.notes | where_exp: "note", "note.date == nil" | sort: "title" %}

{% if site.notes and site.notes.size > 0 %}
{% for note in dated_notes %}
{% assign note_title = note.title | default: note.slug %}
{% assign note_url = note.url | default: '/notes/' %}
{% assign note_summary = note.summary | default: note.excerpt | default: note.content %}
- [{{ note_title }}]({{ note_url | relative_url }}){% if note_summary %}: {{ note_summary | strip_html | strip_newlines | truncate: 220 }}{% endif %}
{% endfor %}
{% for note in undated_notes %}
{% assign note_title = note.title | default: note.slug %}
{% assign note_url = note.url | default: '/notes/' %}
{% assign note_summary = note.summary | default: note.excerpt | default: note.content %}
- [{{ note_title }}]({{ note_url | relative_url }}){% if note_summary %}: {{ note_summary | strip_html | strip_newlines | truncate: 220 }}{% endif %}
{% endfor %}
{% else %}
  <p>No notes published yet. Add files in <code>_notes/</code> to populate this page.</p>
{% endif %}
