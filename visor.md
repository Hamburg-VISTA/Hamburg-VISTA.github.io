---
layout: default
title: Hamburg VISTA - VISOR
---

<img src="/assets/images/visor-logo.png" alt="VISOR logo" width="400">
# VISOR - the VISTA School of Graduate Research
VISOR provides co-funding for projects on the topic of development or application of AI techniques in the natural sciences and engineering, initiated by pairs of supervisors associated with Hamburg VISTA from different institutions. The first call for projects went out in February 2025, and to date X projects have received funding. Here you can learn more about the projects, and discover their ideas, their goals, and the innovations they are driving. 

<strong>VISOR spokesperson:</strong> Prof. Gregor Kasieczka (UHH)

<strong>VISOR co-spokesperson:</strong> Prof. Oliver Niggemann (HSU)

{% assign sorted_people = site.people_visor | sort: 'lastname' %}
<div class="profile-container">
	{% for person in sorted_people %}
	{% assign words = person.intro | split: " " %}
    {% assign preview = words | slice: 0, 10 | join: " " %}
	<div class="visor-profile-block">
	    <div class="top-block">
	        <div class="title-wrapper">
	            <p class="title">{{ person.project_title }}</p>
	        </div>
	    <img src="{{ person.photo }}" class="photo">
	    <p class="student">{{ person.firstname }} {{ person.lastname }} ({{ person.affiliation }})</p>
	    Supervisors: {{ person.supervisor1 }}, {{ person.supervisor2 }}
	    
	    </div>

        <div class="bottom-block">
            <div class="subtitle-block">
            <span class="subtitle-preview">{{ preview }}...</span>

            <span class="subtitle-full" style="display:none;">
              {{ person.intro }}
            </span>

            <a href="#" class="read-more">[Read more]</a>
            <a href="#" class="show-less" style="display:none;">[Show less]</a>
            </div>
        </div>
	</div>
  {% endfor %}
</div>

<script>
document.addEventListener("DOMContentLoaded", function () {
  // ---- read-more toggle ----
  document.querySelectorAll(".subtitle-block").forEach(function (block) {
    const preview = block.querySelector(".subtitle-preview");
    const full = block.querySelector(".subtitle-full");
    const more = block.querySelector(".read-more");
    const less = block.querySelector(".show-less");

    more.addEventListener("click", function (e) {
      e.preventDefault();
      preview.style.display = "none";
      full.style.display = "inline";
      more.style.display = "none";
      less.style.display = "inline";

      // titles/heights might need recalculation after the card grows
      requestAnimationFrame(adjustTitleHeights);
      setTimeout(adjustTitleHeights, 50);
    });

    less.addEventListener("click", function (e) {
      e.preventDefault();
      preview.style.display = "inline";
      full.style.display = "none";
      more.style.display = "inline";
      less.style.display = "none";

      requestAnimationFrame(adjustTitleHeights);
      setTimeout(adjustTitleHeights, 50);
    });
  });

  // ---- helper: debounce ----
  function debounce(fn, wait) {
    let t;
    return function () {
      clearTimeout(t);
      t = setTimeout(fn, wait);
    };
  }

  // ---- main function: equalize title-wrapper heights per visual row ----
  function adjustTitleHeights() {
    const cards = Array.from(document.querySelectorAll(".visor-profile-block"));
    if (!cards.length) return;

    // Reset any previously applied min-heights so measurement is accurate
    cards.forEach(c => {
      const tw = c.querySelector(".title-wrapper");
      if (tw) tw.style.minHeight = '';
    });

    // Group cards by their top position (visual row).
    // Use Math.round on getBoundingClientRect().top to avoid subpixel differences.
    const groups = new Map();
    cards.forEach(card => {
      const top = Math.round(card.getBoundingClientRect().top);
      if (!groups.has(top)) groups.set(top, []);
      groups.get(top).push(card);
    });

    // For each row group, compute max title-wrapper height and apply it
    groups.forEach(groupCards => {
      let maxH = 0;
      groupCards.forEach(card => {
        const tw = card.querySelector(".title-wrapper");
        if (!tw) return;
        const h = Math.ceil(tw.getBoundingClientRect().height);
        if (h > maxH) maxH = h;
      });

      // Apply the max as a min-height (px) to each title-wrapper in the row
      if (maxH > 0) {
        groupCards.forEach(card => {
          const tw = card.querySelector(".title-wrapper");
          if (tw) tw.style.minHeight = maxH + "px";
        });
      }
    });
  }

  // Run on load, after small delay (fonts/images may change layout)
  adjustTitleHeights();
  // Also run a few times shortly after load to catch late layout shifts
  setTimeout(adjustTitleHeights, 100);
  setTimeout(adjustTitleHeights, 500);

  // Recompute on resize (debounced)
  window.addEventListener("resize", debounce(adjustTitleHeights, 100));
});
</script>
