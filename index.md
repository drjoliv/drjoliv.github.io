---
layout: page
title: Desonte Jolivet | Notes on code and ideas
description: Notes on functional programming, category theory, and the ideas that connect them.
---
{% include JB/setup %}

<main class="home">
  <section class="hero">
    <div class="hero-copy">
      <p class="eyebrow"><span class="status-dot"></span> A personal corner of the internet</p>
      <h1>Curious about<br><span class="text-gradient">how things fit together.</span></h1>
      <p class="hero-intro">I'm Desonte — a computer science grad, veteran, and lifelong learner exploring the ideas behind better software.</p>
      <a class="hero-link" href="#about">A little about me <span aria-hidden="true">&darr;</span></a>
    </div>
    <div class="hero-portrait">
      <div class="portrait-glow"></div>
      <img src="/image/me.jpeg" alt="Desonte Jolivet" class="portrait">
      <span class="portrait-caption">DESONTE JOLIVET <span>·</span> DEVELOPER &amp; THINKER</span>
    </div>
    <span class="hero-index" aria-hidden="true">01 / HELLO</span>
  </section>

  <section class="intro-grid" id="about">
    <article class="info-card">
      <p class="eyebrow">01 — About me</p>
      <h2>Following the thread.</h2>
      <p>I'm a veteran with an undergraduate degree in computer science and a particular interest in functional programming and category theory. When I'm not programming in Haskell, I'm exploring how to bring functional ideas to Java — lately, through monads.</p>
    </article>
    <article class="info-card">
      <p class="eyebrow">02 — Around here</p>
      <h2>A place to think out loud.</h2>
      <p>This blog is a home for ideas as they wander through my head: notes, experiments, and long-term documentation for projects I work on. If something here catches your curiosity, you're in the right place.</p>
    </article>
  </section>

  <section class="recent-section">
    <div class="section-heading">
      <div>
        <p class="eyebrow">From the notebook</p>
        <h2>Recent posts<span class="accent-period">.</span></h2>
      </div>
      <a class="all-posts-link" href="/shorts.html">All posts <span aria-hidden="true">&rarr;</span></a>
    </div>
    <ul class="posts recent-posts">
  {% for post in site.posts %}
      {% if post.categories contains "short" %}
      <li class="post-card">
        <a href="{{ BASE_PATH }}{{ post.url }}">
          <span class="post-date">{{ post.date | date: "%b %d, %Y" }}</span>
          <span class="post-title">{{ post.title }}</span>
          <span class="post-arrow" aria-hidden="true">&nearr;</span>
        </a>
      </li>
      {% endif %}
  {% endfor %}
    </ul>
  </section>
</main>
