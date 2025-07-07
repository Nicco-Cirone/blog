---
title: "Building a Blog from Zero with Jekyll and Github Pages"
date: 2025-06-10
layout: post
author: Nicco
categories: [ Coding, Web Development ]
image: assets/uploads/blog_home.webp
---

In June 2025, I set out to migrate my old blog from WordPress.com to a fast, clean, static site built with Jekyll and hosted on GitHub Pages. My goal: have full control over my content, simplify hosting, and use a custom domain like blog.niccocirone.com.
At the time, I was starting from literal zero. No local Jekyll setup, no theme picked out, and no idea how WordPress exports would behave. Here’s how the journey unfolded — and what I learned the hard way.

The first challenge was just installing Ruby and Jekyll on macOS. I used Homebrew to install Ruby, but I quickly hit roadblocks:

* Gems failed to build due to native extension issues
* eventmachine couldn’t compile because macOS lacked Xcode headers
* jekyll wouldn’t install without dragging in a 12GB+ Xcode installation

I realized that the local setup was more effort than it was worth — so I pivoted.

**Discovering GitHub Codespaces**
I switched to GitHub Codespaces, a remote development environment with Jekyll pre-installed and no local config headaches. Within minutes, I had a working dev environment.

I created a repo (/blog) with an empty `README.md`, installed Jekyll via bundle, and served the site. Success: I saw the default Jekyll homepage live at https://niccocirone.com/blog.

**Migrating WordPress Content**
WordPress lets you export your entire blog as an XML file. I downloaded mine and used jekyll-import to convert it into Markdown files in the _posts/ folder.
This mostly worked, but images didn’t display — they referenced WordPress-hosted paths. So the next step was to write a script to download every image from the export and place them in assets/uploads/, then updated the Markdown files to use:

`![Alt text]({{ site.baseurl }}/assets/uploads/image.webp)`

I also decided to convert every image to WebP for performance and consistency. Using a shell script and the cwebp tool, I batch-converted ~140 images and removed the originals.

**Testing Themes (and Breaking Things)**
I wanted a nicer layout than the default. After evaluating a few, I chose Mediumish — clean, readable, and popular among technical writers.
I copied the theme into my repo and adjusted _config.yml to include:

`remote_theme: "wowthemesnet/mediumish-theme-jekyll"
baseurl: "/blog"`

Then everything broke.

**The Case of the Missing Posts**
Despite all the posts being in _posts/ with correct front matter (layout: post, date, etc.), the homepage showed nothing.

I tried everything:

* Switching from `paginator.posts` to site.posts
* Rewriting `index.html` with just a bullet list of post titles
* Adding future: true in `_config.yml`
* Changing layouts from default to home and back
* Confirming that `{% raw %}{{ content }}{% endraw %}` existed in default.html

Nothing worked — until I realized that category pages (like `/categories.html`) actually did show the posts. That meant the posts were fine, but the homepage loop wasn’t working.

Eventually, I suspected that either:

* `site.posts` wasn’t being populated on the homepage
* GitHub Pages wasn’t running a proper Jekyll build
* Or the layout chain (`index.html` → `home.html` → `default.html`) had a disconnect

I dug deeper into the differences between my local build and GitHub Pages. Locally, using GitHub Codespaces with `bundle exec jekyll serve`, everything looked perfect — posts appeared, the “Featured” section worked, pagination was fine.
But the live site at `niccocirone.com/blog` remained broken. No featured posts. No main content. Just a skeleton.

Then I looked closely at the build logs from GitHub Pages.

Turns out, GitHub Pages doesn’t use the latest version of Jekyll. It uses **Jekyll 3.10.0**, while I was developing locally on **Jekyll 4.4.1** with Ruby 3.3.4. That mismatch explained everything — the theme I chose (Mediumish) relied on newer Liquid tags and pagination methods that don’t work in Jekyll 3.x.
So the site built fine locally but failed silently when deployed through GitHub Pages’ default workflow.

**Fixing It with GitHub Actions**

The solution? Skip GitHub’s built-in Pages build process and instead **build the site manually** using [GitHub Actions](https://github.com/features/actions), then deploy the output.

GitHub Actions lets you define workflows that run in response to events (like `push`). I used the official "Deploy Jekyll site to Pages" workflow template and slightly customized it:

* Set the Ruby version to match my local dev (3.1, because GitHub’s hosted runner couldn’t use 3.3 yet)
* Ran `bundle install` with my Gemfile
* Used `bundle exec jekyll build` to generate the site
* Uploaded the generated `_site/` folder as an artifact
* Deployed that artifact to GitHub Pages

With that, the deployment **matched** my local preview exactly — because it was the same Jekyll version, same theme, same configuration.

Today, the blog:

* Loads fast using WebP images and GitHub Pages CDN
* Has all legacy WordPress posts, cleaned and converted
* Uses a Jekyll theme that I fully control
* Is editable entirely from GitHub
  
---

Site: [https://niccocirone.com/blog](https://niccocirone.com/blog)
Source: [https://github.com/Nicco-Cirone/blog](https://github.com/Nicco-Cirone/blog)
