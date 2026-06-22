---
layout: home
---

<section class="home-hero">
  <div class="hero-index" aria-hidden="true">J# / 001</div>
  <div class="home-hero__content">
    <p class="eyebrow">Free and open source software</p>
    <h1>Let’s write<br><span>some code!</span></h1>
    <p class="home-hero__copy">I create practical developer tools, lightweight C++ libraries, command-line utilities, and an occasional RPG engine—built openly and made to last.</p>
    <div class="home-hero__actions">
      <a class="button-link" href="{{ '/projects/' | relative_url }}">View open-source work <span>↗</span></a>
      <a class="text-link" href="{{ '/about/' | relative_url }}">Meet the developer →</a>
    </div>
  </div>
  <div class="hero-orbit" aria-hidden="true">
    <div class="hero-orbit__ring"><span>C++</span><span>Rust</span><span>Go</span></div>
    <div class="hero-orbit__core">OPEN<br>SOURCE</div>
    <small>BUILD / SHARE / LEARN</small>
  </div>
  <div class="hero-scroll">SCROLL TO EXPLORE <i></i></div>
</section>

<section class="statement-band">
  <span>BUILD USEFUL THINGS</span><i>✦</i><span>SHARE THE SOURCE</span><i>✦</i><span>KEEP LEARNING</span>
</section>

<section class="discipline-section">
  <header class="section-heading"><p>01 / THE WORK</p><h2>Software with<br>a practical pulse.</h2></header>
  <div class="discipline-grid">
    <article><span>01</span><div><h3>Libraries</h3><p>Focused C++ building blocks for email, date/time, and systems work.</p></div><b>C++</b></article>
    <article><span>02</span><div><h3>Tools</h3><p>Terminal and editor utilities designed around real developer workflows.</p></div><b>RUST</b></article>
    <article><span>03</span><div><h3>Experiments</h3><p>Game engines, computer-science exercises, and curiosity with a compiler.</p></div><b>GO / SDL2</b></article>
  </div>
  <a class="button-link button-link--outline" href="{{ '/projects/' | relative_url }}">Explore every project <span>→</span></a>
</section>

<section class="signal-section">
  <header class="section-heading section-heading--row"><div><p>02 / CHANGELOG</p><h2>Latest signals.</h2></div><a class="text-link" href="{{ '/news/' | relative_url }}">Full project log →</a></header>
  {% include news-list.html limit=4 compact=true %}
</section>

<section class="writing-section">
  <header class="section-heading section-heading--row"><div><p>03 / FIELD NOTES</p><h2>What I learned<br>along the way.</h2></div><a class="text-link" href="{{ '/blog/' | relative_url }}">All writing →</a></header>
  <div class="post-preview-grid">
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    {%- for post in site.posts limit:4 -%}
    <a class="post-preview" href="{{ post.url | relative_url }}"><span class="post-preview__date">{{ post.date | date: date_format }}</span><h3>{{ post.title | escape }}</h3><i>↗</i></a>
    {%- endfor -%}
  </div>
</section>

<section class="closing-cta">
  <img src="{{ '/assets/jedlogo.png' | relative_url }}" alt="">
  <div><h2>Let’s write<br><em>some code!</em></h2></div>
  <a href="https://github.com/{{ site.github_username }}">START ON GITHUB <b>↗</b></a>
</section>
