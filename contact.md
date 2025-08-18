---
layout: default
title: Contact
description: Get in touch with me for collaborations, questions, or just to say hello.
hero: true
hero_title: "Let's Connect"
hero_subtitle: "Whether you have a question about a blog post, want to collaborate on a project, or just want to chat about technology, I'm always happy to connect."
---

<div class="contact-page">
  <div class="contact-methods">

    <div class="contact-method">
      <div class="contact-icon">
        <svg width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
        <path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/>
        </svg>
      </div>
      <div class="contact-info">
        <h4>Email</h4>
        <p>Best for discussions</p>
        <a href="mailto:davidromm11@gmail.com">davidromm11@gmail.com</a>
      </div>
    </div>

    <div class="contact-method">
      <div class="contact-icon">
        <svg width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
          <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
        </svg>
      </div>
      <div class="contact-info">
        <h4>GitHub</h4>
        <p>Check out my code and projects</p>
        <a href="https://github.com/davidromm11" target="_blank" rel="noopener">@davidromm11</a>
      </div>
    </div>

    <div class="contact-method">
      <div class="contact-icon">
        <svg width="24" height="24" fill="currentColor" viewBox="0 0 24 24">
          <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
        </svg>
      </div>
      <div class="contact-info">
        <h4>LinkedIn</h4>
        <p>Networking</p>
        <a href="https://linkedin.com/in/david-romm" target="_blank" rel="noopener">David Romm</a>
      </div>
    </div>

  </div>

Looking forward to hearing from you.

</div>

<style>
.contact-methods {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin: 2rem 0;
}

.contact-method {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1.5rem;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 0.5rem;
  transition: all 0.3s ease;
}

.contact-method:hover {
  transform: translateY(-2px);
  border-color: var(--accent-color);
}

.contact-icon {
  background: var(--accent-color);
  color: white;
  padding: 0.75rem;
  border-radius: 0.375rem;
  flex-shrink: 0;
}

.contact-info h4 {
  margin-bottom: 0.5rem;
  color: var(--text-color-heading);
}

.contact-info p {
  margin-bottom: 0.5rem;
  color: var(--text-color-secondary);
  font-size: 0.875rem;
}

.contact-info a {
  color: var(--accent-color);
  font-weight: 500;
}

.contact-form {
  max-width: 600px;
  margin: 2rem 0;
  padding: 2rem;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 0.5rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: var(--text-color-heading);
  font-weight: 500;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem 1rem;
  background: var(--bg-tertiary);
  border: 1px solid var(--border-color);
  border-radius: 0.375rem;
  color: var(--text-color);
  font-size: 1rem;
  font-family: inherit;
  transition: all 0.3s ease;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--accent-color);
  box-shadow: 0 0 0 3px rgba(var(--accent-color), 0.1);
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
}

.form-group input::placeholder,
.form-group textarea::placeholder {
  color: var(--text-color-muted);
}

.faq-section {
  margin-top: 3rem;
}

.faq-item {
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid var(--border-color);
}

.faq-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
}

.faq-item h4 {
  color: var(--accent-color);
  margin-bottom: 0.5rem;
  font-size: 1.125rem;
}

.faq-item p {
  color: var(--text-color-secondary);
  line-height: 1.6;
}

@media (max-width: 767px) {
  .contact-methods {
    grid-template-columns: 1fr;
  }
  
  .contact-form {
    padding: 1.5rem;
  }
  
  .contact-method {
    padding: 1rem;
  }
}
</style>
