---
layout: default
title: Hamburg VISTA - All publications
---

# Publications

<div id="filters-container">
<label for="pi-filter">Filter by researchers:</label>
<select id="pi-filter"><option value="">All</option></select>

<label for="year-filter">Filter by year:</label>
<select id="year-filter"><option value="">All</option></select>

<label for="affiliation-filter">Filter by affiliation:</label>
<select id="affiliation-filter"><option value="">All</option></select>
</div>

<ul id="pub-list"></ul>

<script>
  // Build all_people using collection items (works if people are collections)
  const all_people = [];
  {% comment %}
    For collection items use `for person in site.collection_name` and reference
    the front-matter fields as person.first_name etc.
  {% endcomment %}

  {% for person in site.people_PIs %}
    all_people.push({
      firstname: "{{ person.firstname | escape }}",
      lastname: "{{ person.lastname | escape }}",
      affiliation: {{ person.affiliation | jsonify }},
      publications: {% if person.publications %}{{ person.publications | jsonify }}{% else %}[] {% endif %}
    });
  {% endfor %}

  {% for person in site.people_board %}
    all_people.push({
      firstname: "{{ person.firstname | escape }}",
      lastname: "{{ person.lastname | escape }}",
      affiliation: {{ person.affiliation | jsonify }},
      publications: {% if person.publications %}{{ person.publications | jsonify }}{% else %}[] {% endif %}
    });
  {% endfor %}

  // Flatten publications, keep pi metadata
  const publications = [];
  all_people.forEach(person => {
    if (!person.publications) return;
    person.publications.forEach(pub => {
      publications.push({
        title: pub.title || "",
        authors: pub.authors || [],
        year: pub.year || null,
        venue: pub.venue || "",
        doi: pub.doi || null,
        url: pub.url || null,
        firstname: person.firstname,
        lastname: person.lastname,
        affiliation: person.affiliation
      });
    });
  });

  // Sort publications: first by year descending, then by PI last name ascending
  publications.sort((a, b) => {
    // Sort by year descending
    if (b.year !== a.year) return b.year - a.year;
  
    // If years are the same, sort by last name ascending
    return a.lastname.localeCompare(b.lastname);
  });


  // Populate dropdowns
  const piFilter = document.getElementById("pi-filter");
  const yearFilter = document.getElementById("year-filter");
  const affFilter = document.getElementById("affiliation-filter");
  
  // Only include people who have at least one publication
  const peopleWithPubs = all_people.filter(p => Array.isArray(p.publications) && p.publications.length > 0);


  const piNames = [...new Set(peopleWithPubs.map(p => `${p.firstname} ${p.lastname}`))].sort();
  piNames.forEach(name => piFilter.insertAdjacentHTML('beforeend', `<option value="${name}">${name}</option>`));

  const years = [...new Set(publications.map(p => p.year).filter(y => y !== null))].sort((a,b)=>b-a);
  years.forEach(y => yearFilter.insertAdjacentHTML('beforeend', `<option value="${y}">${y}</option>`));

  const affiliations = [...new Set(peopleWithPubs.flatMap(p => p.affiliation))].sort();
  affiliations.forEach(a => affFilter.insertAdjacentHTML('beforeend', `<option value="${a}">${a}</option>`));

  // Render function
function renderPubs(filters = {}) {
  const container = document.getElementById("pub-list");
  container.innerHTML = "";

  publications.forEach(pub => {
    const piFull = `${pub.firstname} ${pub.lastname}`;
    if (filters.pi && filters.pi.length > 0 && !filters.pi.includes(piFull)) return;
    if (filters.year && filters.year !== "" && pub.year !== Number(filters.year)) return;
    if (filters.affiliation &&filters.affiliation !== "" && !pub.affiliation.includes(filters.affiliation)) return;

    const authors = Array.isArray(pub.authors) ? pub.authors.join(", ") : pub.authors;

    // Venue becomes the clickable link
    const venueLink = pub.doi 
      ? `<a href="https://doi.org/${pub.doi}" target="_blank" rel="noopener">${pub.venue}</a>` 
      : pub.url 
        ? `<a href="${pub.url}" target="_blank" rel="noopener">${pub.venue}</a>` 
        : pub.venue;

    // Create a div for each publication
    const pubDiv = document.createElement("div");
    pubDiv.style.marginBottom = "1em";  // spacing between publications

    // Fill div content
    pubDiv.innerHTML = `
      <div class="quattrocento-sans-paper-titles"><strong>${pub.title}</strong> (${pub.year})</div><br>
      <div>${authors}</div>
      <div>${venueLink}</div>
    `;

    container.appendChild(pubDiv);
  });
}

  // Initial render
  renderPubs();

  // Event handlers
  [piFilter, yearFilter, affFilter].forEach(el => {
    el.addEventListener("change", () => {
      renderPubs({
        pi: piFilter.value,
        year: yearFilter.value,
        affiliation: affFilter.value
      });
    });
  });
</script>

