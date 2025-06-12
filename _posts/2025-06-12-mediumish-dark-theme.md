---
title: "Automated Dark Mode for my Jekyll Theme: My first Github fork"
date: 2025-06-12
layout: post
author: Nicco
categories: [ Coding, Github ]
image: assets/uploads/post-dark-theme.webp
---


I like dark mode. Not because it's trendy, but because it feels right. It's easier on the eyes, especially when you're reading or writing late. It saves battery. It's good for OLED screens. It even saves a bit of energy, which, at scale, isn't nothing.
My whole setup is dark. My terminal. My editor. My calendar. My email. Even my documents. Everything—until I open a blog post or a web app that ignores the user's preference and burns my retinas with a bright white UI.
So I made a decision: **every website I build will support dark mode**. That includes my personal homepage ([niccocirone.com](https://niccocirone.com)) and now my blog at ([niccocirone.com/blog](https://niccocirone.com/blog))
The blog is built on ([Mediumish](https://github.com/wowthemesnet/mediumish-theme-jekyll)), a nice minimal Jekyll theme. It didn’t support dark mode out of the box. So I fixed that.

---

### Step 1: Forking the original repository

I forked the repo from [wowthemesnet/mediumish-theme-jekyll](https://github.com/wowthemesnet/mediumish-theme-jekyll) and created a new branch called `dark-theme`.
I decided that I wanted to do all my edits in the browser and use Github Pages for my testings and showcases. This is because installing Jekyll on my mac needs many Gb of space that I don't want to use in this way, and it just feels the most lightweight setup for this simple project.

Then I went on scoping my project. The idea was simple: if a browser prefers dark mode, I want to serve a dark stylesheet.
Jekyll doesn't support anything dynamic on its own, but the browser does.

So I added this bit at the end of the `head` of the `default.html` layout:

```html
<link rel="stylesheet" href="{{ '/assets/css/dark.css' | relative_url }}" media="(prefers-color-scheme: dark)">
```

This tells the page to let dark.css rules override the rules of the previously listed css files. Hence, it has to come last.

---

### Step 2: Writing the CSS

With help from ChatGPT in identifying all the bits to change, I built the dark theme in `dark.css`.

I styled:

* the background (`#111`)
* text (`#f0f0f0`)
* links (`#66d9ef` and lighter on hover)
* the navbar, jumbotron, footers, cards, forms
* a star rating component that needed a border-radius fix
* the newsletter bar at the bottom

The design keeps the tone of the original theme but swaps the palette.

I also added a rule to swap the jumbotron image to another, more dark-mode-friendly image, which I also generated and added to the assets.

---

### Fixing the Deploy

The deploy itself stumbled on a couple issues along the way.
First of all, I had to change the Gemfile of the original repository to match what Github pages uses, or Github Actions wouldn't build my page.

```ruby
gem "jekyll", "~> 4.3"
```

Second, after renaming the repo, GitHub Pages broke. :(
The dark theme didn’t load, and the layout looked wrong.
The problem? I had forgotten to update `baseurl` in `_config.yml`. It was still pointing at the old repo name. 
Once I fixed that, the paths to assets like `dark.css` started working again.

---

### Getting my Repo ready for the pull request and showcase

At this point, all my changes were in `dark-theme`, and `master` still matched the original upstream.

I:

* set `dark-theme` as the default branch
* deleted `master`
* renamed the repo to something more descriptive
* updated the README
* left GitHub Pages active as a live demo

You can view it here:
👉 [niccocirone.com/dark-theme-for-mediumish-theme-jekyll](https://niccocirone.com/dark-theme-for-mediumish-theme-jekyll/)

---

### Sending the PR

Once I had everything working and cleaned up, I sent a pull request to the original repo:

> [wowthemesnet/mediumish-theme-jekyll #72](https://github.com/wowthemesnet/mediumish-theme-jekyll/pull/72)

If it gets merged, great. If not, I still have a great learning experience in my bag, a working fork with a live demo and a blog post to show for it.
And, if you're like me, you can get inspiration to finally stop your browser from flashing white every time you want to read your own blog.

![first-pull]({{ site.baseurl }}/assets/uploads/first-pull.webp)
[Click to check out the pull request](https://github.com/wowthemesnet/mediumish-theme-jekyll/pull/266)
---
