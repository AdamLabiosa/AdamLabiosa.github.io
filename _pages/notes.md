---
layout: archive
title: "Notes"
permalink: /notes/
author_profile: true
---

{% include base_path %}

For interesting tutorials or notes for myself that might be useful to someone else!


{% assign dated_notes = site.notes | where_exp: "note", "note.date" | sort: "date" | reverse %}
{% assign undated_notes = site.notes | where_exp: "note", "note.date == nil" | sort: "title" %}

{% if site.notes and site.notes.size > 0 %}
{% for note in dated_notes %}
{% assign note_summary = note.summary | default: note.excerpt %}
- [{{ note.title }}]({{ note.url | relative_url }}){% if note_summary %}: {{ note_summary | strip_html | strip_newlines | truncate: 220 }}{% endif %}
{% endfor %}
{% for note in undated_notes %}
{% assign note_summary = note.summary | default: note.excerpt %}
- [{{ note.title }}]({{ note.url | relative_url }}){% if note_summary %}: {{ note_summary | strip_html | strip_newlines | truncate: 220 }}{% endif %}
{% endfor %}
{% else %}
  <p>No notes published yet. Add files in <code>_notes/</code> to populate this page.</p>
{% endif %}
