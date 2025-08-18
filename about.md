---
layout: default
title: About
description:
hero: true
hero_title: 'About Me'
hero_subtitle: ''
---

<div class="about-page">
  <div class="author-bio">
    <div class="author-avatar">
      <img src="/assets/images/headshot.jpg" alt="David Romm" onerror="this.style.display='none'">
    </div>
    
    <div class="author-info">
      <h2 class="author-name">David Romm</h2>
      <p class="author-title"></p>
      <p class="author-description">
      I'm a recent Data Science graduate from St. Lawrence University with a passion for tech, especially cybersecurity. I'm currently spending time researching the field with the goal of building a career around it.
      </p>
      <p class="author-description">
      Outside of tech, you'll often find me at the golf course, the gym, or spending time with my friends and family. I also enjoy cooking and fishing.
      </p>
      <p class="author-description"> 
      I've always been the type of person who can't fully grasp a topic until I understand <i>everything</i> about it. This pushes me to learn deeply and solve problems with a thorough, well-informed approach. In a field where a single issue could have hundreds of possible causes, it’s rewarding to find the solution because it shows you understand how to work through all those possibilities.
      </p>
      <p class="author-description"> 
      This blog serves a few purposes: it's my space to practice web development, vps configuration, web hosting, and writing. It's also a place where I can organize my thoughts and solidify my comprehension of topics, all while tracking my progress. Over time, I hope these posts will turn from personal learning notes to something that helps others.
      </p>
      <p class="author-description">
      I'm excited to share this journey with you and hope you find the content here helpful and interesting.
      </p>
    </div>
  </div>
</div>

<style>
  .about-page {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
  }

  .author-bio {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .author-avatar {
    margin-bottom: 2rem;
  }

  .author-avatar img {
    width: 200px;
    height: 200px;
    object-fit: cover;
    border-radius: 8px; /* Slightly rounded corners for a softer square */
    border: 3px solid var(--border-color);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }

  .author-avatar img:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  }

  .author-info {
    max-width: 700px;
  }

  .author-name {
    font-size: 2rem;
    margin-bottom: 0.5rem;
    color: var(--text-color);
  }

  .author-title {
    font-size: 1.1rem;
    color: var(--accent-color);
    margin-bottom: 1.5rem;
    font-weight: 500;
  }

  .author-description {
    font-size: 1rem;
    line-height: 1.7;
    color: var(--text-color-secondary);
    margin-bottom: 1.5rem;
    text-align: left;
  }

  .author-description:last-child {
    margin-bottom: 0;
  }

  @media (max-width: 768px) {
    .about-page {
      padding: 1rem;
    }

    .author-avatar img {
      width: 150px;
      height: 150px;
    }

    .author-name {
      font-size: 1.5rem;
    }

    .author-description {
      font-size: 0.95rem;
    }
  }
</style>
