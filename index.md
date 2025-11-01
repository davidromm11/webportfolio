---
layout: default
title: Home
---

<div class="home-intro">
  <h1>David Romm</h1>
  <p>Projects, thoughts, and experiences.</p>
</div>

<div class="posts-list">
  {% for post in site.posts limit:5 %}
    <article class="post-item">
      <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
      {% endif %}
    </article>
  {% endfor %}
</div>

{% if site.posts.size > 5 %}

<div style="text-align: center; margin-top: 3rem;">
  <a href="{{ '/blog/' | relative_url }}" class="view-all-btn">View All Posts</a>
</div>
{% endif %}

---

_Feel free to [reach out](/contact/) if you have any questions, suggestions, or just want to connect._
