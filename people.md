---
layout: default
title: Hamburg VISTA - People
---

# People

### Board

{% assign sorted_people = site.people_board | sort: 'lastname' %}
<div class="profile-container">
  {% for person in sorted_people %}
    <div class="profile">
      <img src="{{ person.photo }}" class="circular-image" alt="Photo of {{ person.firstname }} {{ person.lastname }}">
      <p><a href="{{ person.link }}" target="_blank">{{ person.title }} {{ person.firstname }} {{ person.lastname }}</a><br>{{ person.affiliation }}</p>

    </div>
  {% endfor %}
</div>

### Members

{% assign sorted_people = site.people_PIs | sort: 'lastname' %}
<div class="profile-container">
  {% for person in sorted_people %}
    <div class="profile">
      <img src="{{ person.photo }}" class="circular-image" alt="Photo of {{ person.firstname }} {{ person.lastname }}">
      <p><a href="{{ person.link }}" target="_blank">{{ person.title }} {{ person.firstname }} {{ person.lastname }}</a><br>{{ person.affiliation }}</p>

    </div>
  {% endfor %}
</div>

