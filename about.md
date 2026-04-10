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
      <p class="about-eyebrow">About</p>
      <h1>Priscila Campos</h1>
    </div>
  </div>

  <div class="about-content">

    <p class="about-lead">
      I am a Quality Engineer professional with {{ years }}+ years of experience in technology,
      working primarily in critical domains such as fintech, banking, e-commerce, and ERP systems.
    </p>

    <p>
      My work focuses on quality as an engineering capability. I design validation strategies,
      test architectures, and automation approaches that integrate directly into delivery workflows,
      supporting reliability in complex distributed systems.
    </p>

    <p>
      I have experience with microservices in containerized environments, as well as distributed
      systems in AWS. This includes validating asynchronous flows built on queues and events,
      using messaging services such as SQS and SNS.
    </p>

    <p>
      Throughout my career, I have specialized in translating complex business rules into executable
      specifications, turning end-to-end automation into a form of living documentation that scales
      across teams.
    </p>

    <p>
      My approach is grounded in systems thinking, observability, and architectural analysis.
      I am interested in making systems more understandable, more reliable, and easier to evolve —
      not only verifying behavior, but improving how systems are built.
    </p>

    <hr class="about-divider"/>

    <div class="about-columns">

      <div>
        <h2>Focus</h2>
        <ul>
          <li>Quality strategy for complex systems</li>
          <li>Test architecture for web and backend services</li>
          <li>Distributed systems validation in AWS</li>
          <li>Asynchronous and event-driven systems</li>
        </ul>
      </div>

      <div>
        <h2>Selected Work</h2>
        <ul>
          <li>Test harnesses for edge cases in complex integrations</li>
          <li>Data-driven quality visibility systems</li>
          <li>Custom frameworks for asynchronous validation</li>
        </ul>
      </div>

      <div>
        <h2>Additional</h2>
        <p class="muted">
          Experience with mobile automation (Android, iOS, React Native) as a complementary area.
        </p>
      </div>

    </div>

  </div>

</section>