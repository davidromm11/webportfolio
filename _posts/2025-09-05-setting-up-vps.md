---
layout: post
title: 'Setting Up My VPS and Hosting with Jekyll'
date: 2025-09-05
author: David Romm
description: 'How I set up a VPS, secured it, configured Nginx, and deployed my blog using Jekyll.'
---

In my first post, I walked through how I coded a website from scratch. By the end, I had a decent site but I wanted better. This time, I’ll cover how I got my site hosted online: choosing a VPS provider, securing the server, setting up Nginx, and finally moving my project into Jekyll so I could build something better.

## Choosing a VPS Provider

There are many VPS providers out there, which made the buying process a little overwhelming at first. Some providers had great introductory deals but required long-term contracts or had questionable cancellation policies. After doing some research and hearing positive recommendations, I decided to go with OVHcloud.

They offer a starter VPS with enough resources to run a static website. When setting up the server, you get to choose an operating system image. I went with Ubuntu 22.04 (distribution only) instead of “distribution + applications.” That way, I’d have full control over what was installed and could learn to configure everything myself. Ubuntu 22.04 is widely used and a common choice for web hosting.

OVH provides plenty of documentation and support guides, which were super helpful in making sure I didn’t miss important steps while setting up and securing the VPS.

## Initial Setup

Once the server was live, I connected through SSH using the root credentials provided. From there, I:

1. Created a new user for myself and gave it admin privileges (this is safer than being logged in as root all the time).
2. Set up SSH keys for authentication. At first, I tried adding a passphrase to my key for extra security, but the VPS didn’t have enough resources to handle it smoothly. After waiting 10+ minutes just to log in, I regenerated my key pair without a passphrase and copied the new public key over to the server.
3. Disabled password authentication and root login in the SSH config so only key-based logins were allowed.
4. Configured a firewall (UFW) and only left open the ports I needed.
5. Applied updates and patches to the operating system, then installed the basic apps I’d need later.

At this point, I had a server that was reasonably secure and ready to host my website.

## Web Hosting with Nginx

I started by purchasing my domain, dcromm.dev, and pointing it to my server’s IP address using DNS settings.

To handle web traffic, I installed Nginx, an open-source web server. Nginx uses _server blocks_ to define how domains map to files on the server. I created a new server block under `/etc/nginx/sites-available` and set:

- My domain name (`dcromm.dev`)
  &
- The root directory where my site files would live

I then created a symbolic link in `/etc/nginx/sites-enabled` so the config would go live, tested it, and reloaded Nginx.

After that, I set up Certbot to get a free HTTPS certificate. This wasn't all that necessary since there should be any super important traffic coming to and from my site, but better safe than sorry. I also made sure Nginx itself was allowed through the firewall.

With the basics in place, the last thing I needed was the actual site content.

## Using Jekyll

While my first site was a good learning exercise in raw HTML and CSS, I wanted something more flexible without having to manually repeat code across every page.

Jekyll takes Markdown and layouts, then builds them into static HTML files that a web server like Nginx can host. Here’s the basic workflow I used:

1. `jekyll new myblog` — generates a new site with all the necessary files and folders.
2. `bundle install` — installs the Ruby gems Jekyll depends on.
3. `bundle exec jekyll build` — builds the site into a `_site` folder, which Nginx points to.
4. `bundle exec jekyll serve` — runs a local preview server so I can see changes before pushing them live.

I created a Git repository and set up remote connections so that whenever I update the site locally, I can just push changes to my VPS and rebuild. This made deployment very smooth.

Finally, I chose a theme, edited the `_config.yml` file, and started adding content.

## Final Thoughts

Getting my VPS up and running was a big step. I learned about server security, firewalls, Nginx, SSL certificates, and how to use Jekyll to manage a blog.

My first website was something I hacked together to learn HTML and CSS. This new setup, though, feels much more functional. I now have a reliable hosting environment, a workflow for pushing updates, and a static site generator that makes writing posts like this one much easier.
