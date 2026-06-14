---
layout: home
---

<section class="home-hero">
  <div>
    <div class="eyebrow"><h4>Free and open source software</h4></div><br />
    <p class="home-hero__copy">
      I build practical developer tools, C++ libraries, command-line utilities, and the occasional RPG engine experiment. The mission is simple: useful software, open source, and documented enough for the next person to keep moving.
    </p>
    <br/>
    <div class="home-hero__actions">
      <a class="button-link" href="/projects/">Explore projects</a>
      <a class="button-link button-link--ghost" href="/blog/">Read the blog</a>
    </div>
  </div>

  <aside class="signal-panel">
    <div class="signal-panel__label">Latest update</div>
    <span class="signal-panel__value">{{ site.data.news.first.date | date: "%b %-d" }}</span>
    <p class="signal-panel__note">{{ site.data.news.first.title }}</p>
  </aside>
</section>

<section class="quick-grid" aria-label="Site highlights">
  <div class="quick-card">
    <h2>C++ libraries</h2>
    <p>Lightweight libraries for email, date/time work, and system-level learning.</p>
  </div>
  <div class="quick-card">
    <h2>Tools</h2>
    <p>Terminal apps and editor utilities built around practical workflows.</p>
  </div>
  <div class="quick-card">
    <h2>Writing</h2>
    <p>Notes from real implementation work with C++, Go, Linux, Azure, and debugging.</p>
  </div>
</section>

<section>
  <div class="section-header">
    <div>
      <div class="eyebrow">Activity</div>
      <h2>Latest news</h2>
      <p>Recent releases, package updates, project milestones, and new articles.</p>
    </div>
    <div class="section-header__actions">
      <a class="button-link button-link--ghost" href="/news/">All news</a>
    </div>
  </div>

  {% include news-list.html limit=5 compact=true %}
</section>

<section>
  <div class="section-header">
    <div>
      <div class="eyebrow">Notes</div>
      <h2>Latest blog posts</h2>
    </div>
    <div class="section-header__actions">
      <a class="button-link button-link--ghost" href="/blog/">All posts</a>
    </div>
  </div>

  <div class="post-preview-grid">
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    {%- for post in site.posts limit:4 -%}
      <article class="post-preview">
        <span class="post-preview__date">{{ post.date | date: date_format }}</span>
        <h3><a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a></h3>
      </article>
    {%- endfor -%}
  </div>
</section>
