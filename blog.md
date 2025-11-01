---
layout: default
title: Blog
---

<div class="blog-header">
  <h1>All Posts</h1>
</div>

<div class="posts-list">
  {% for post in site.posts %}
    <article class="post-item">
      <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
      {% endif %}
      {% if post.tags and post.tags.size > 0 %}
        <div class="post-tags-inline">
          {% for tag in post.tags limit:3 %}
            <a href="{{ '/tags/' | append: tag | slugify | relative_url }}">#{{ tag }}</a>
          {% endfor %}
        </div>
      {% endif %}
    </article>
  {% endfor %}
</div>
