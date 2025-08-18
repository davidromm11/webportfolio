---
layout: default
title: Home
hero: true
hero_title: "David's Blog"
hero_subtitle: 'Where I document my exploration of IT.'
hero_cta:
  text: 'View all Posts'
  url: '/blog/'
---

## Latest Posts

<div class="blog-list">
  {% for post in site.posts limit:3 %}
    <article class="post-preview">
      <div class="post-preview-header">
        <h3 class="post-preview-title">
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </h3>
        <div class="post-preview-meta">
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
          {% if post.author %}<span>by {{ post.author }}</span>{% endif %}
          {% if post.reading_time %}<span>{{ post.reading_time }} min read</span>{% endif %}
        </div>
      </div>
      
      {% if post.excerpt %}
        <div class="post-preview-excerpt">
          {{ post.excerpt | strip_html | truncatewords: 30 }}
        </div>
      {% endif %}
      
      {% if post.tags and post.tags.size > 0 %}
        <div class="post-preview-tags">
          {% for tag in post.tags limit:3 %}
            <a href="{{ '/tags/' | append: tag | slugify | relative_url }}" class="tag">{{ tag }}</a>
          {% endfor %}
          {% if post.tags.size > 3 %}
            <span class="tag">+{{ post.tags.size | minus: 3 }} more</span>
          {% endif %}
        </div>
      {% endif %}
      
      <a href="{{ post.url | relative_url }}" class="read-more">Read more</a>
    </article>
  {% endfor %}
</div>

{% if site.posts.size > 3 %}

  <div class="text-center mt-xl">
    <a href="{{ '/blog/' | relative_url }}" class="btn btn-primary">View All Posts</a>
  </div>
{% endif %}
---

_Feel free to [reach out](/contact/) if you have any questions, suggestions, or just want to connect._
