---
layout: default
title: Hamburg VISTA - Publications
---

# Publications

## 2025
{% for paper in site.data.publications2025 %}
  <div class="quattrocento-sans-paper-titles">
  {{ paper.title }}</div>
  {{ paper.authors }}<br>{% if paper.journal %}<a href="{{ paper.journallink }}" target="_blank">{{ paper.journal }}</a>{% endif %}{% if paper.e-print and paper.journal == nil %}e-print:
  <a href="https://arxiv.org/abs/{{ paper.e-print }}" target="_blank">{{ paper.e-print }}</a>{% endif %}<br>
{% endfor %}

## 2024
{% for paper in site.data.publications2024 %}
  <div class="quattrocento-sans-paper-titles">
  {{ paper.title }}</div>
  {{ paper.authors }}<br>{% if paper.journal %}<a href="{{ paper.journallink }}" target="_blank">{{ paper.journal }}</a>{% endif %}{% if paper.e-print and paper.journal == nil %}e-print:
  <a href="https://arxiv.org/abs/{{ paper.e-print }}" target="_blank">{{ paper.e-print }}</a>{% endif %}<br>
{% endfor %}

## 2023
{% for paper in site.data.publications2023 %}
  <div class="quattrocento-sans-paper-titles">
  {{ paper.title }}</div>
  {{ paper.authors }}<br>{% if paper.journal %}<a href="{{ paper.journallink }}" target="_blank">{{ paper.journal }}</a>{% endif %}{% if paper.e-print and paper.journal == nil %}e-print:
  <a href="https://arxiv.org/abs/{{ paper.e-print }}" target="_blank">{{ paper.e-print }}</a>{% endif %}<br>
{% endfor %}

## 2022
{% for paper in site.data.publications2022 %}
  <div class="quattrocento-sans-paper-titles">
  {{ paper.title }}</div>
  {{ paper.authors }}<br>{% if paper.journal %}<a href="{{ paper.journallink }}" target="_blank">{{ paper.journal }}</a>{% endif %}{% if paper.e-print and paper.journal == nil %}e-print:
  <a href="https://arxiv.org/abs/{{ paper.e-print }}" target="_blank">{{ paper.e-print }}</a>{% endif %}<br>
{% endfor %}

