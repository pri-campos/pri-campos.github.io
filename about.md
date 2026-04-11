---
layout: page
title: About
permalink: /about/
---

{% assign start_year = 2009 %}
{% assign start_month = 5 %}

{% assign current_year = "now" | date: "%Y" | plus: 0 %}
{% assign current_month = "now" | date: "%m" | plus: 0 %}

{% assign years = current_year | minus: start_year %}

{% if current_month < start_month %}
  {% assign years = years | minus: 1 %}
{% endif %}

<section class="about-minimal">

  <div class="about-header">
    <img
      src="{{ '/assets/images/about-profile.png' | relative_url }}"
      alt="Portrait of Priscila Campos"
      class="about-avatar"
    >

    <div>
      <p class="about-eyebrow">about.</p>
      <h1>Priscila Campos</h1>
    </div>
  </div>

  <div class="about-content">

    <p class="about-lead">
      I’m a Quality Engineer, born in Minas Gerais, Brazil — where things move fast, yet there’s always time for coffee.
    </p>

    <p>
      I approach quality as a living system. 
      One that requires understanding how efficiency, speed, and differentiation interact — and how to design strategies that balance these forces without compromising long-term sustainability.
    </p>

    <p>
      My work focuses on building adaptable quality strategies, grounded in data and shaped by context. 
      I enjoy mapping complex systems, identifying leverage points, and defining how quality emerges — not just how it is tested.
    </p>

    <p>
      I see quality engineering as a dynamic puzzle: 
      pieces are constantly moving, rules evolve, and outcomes depend on how well we understand connections and transformations within the system.
    </p>

    <p>
      What drives me is complexity. 
      I’m motivated by challenges that require new ways of thinking — whether that means introducing new approaches or recombining existing ones to create something better.
    </p>

    <hr class="about-divider"/>
  </div>

</section>