+++
title = 'Goodbye, Jekyll! Hello, Hugo!'
description = 'Migrating from Jekyll to Hugo'
date = 2024-11-06T21:15:17-03:00
tags = ['Blogging', 'Static Site', 'Jekyll', 'Hugo']
draft = false
+++

After running into some dependency issues and long build and deployment times on Cloudflare, I decided it was finally time to move away from Jekyll and switch to a different Static Site Generator.

I’ve tried other ways of publishing on my blog before, like Ghost and 11ty, but this time I went with Hugo.

I spent a focused (and somewhat frustrating) weekend getting the hang of Hugo’s core features and basic structure, as well as migrating my pages, posts, and layouts, which to be honest, were pretty confusing at first glance.

Here are my first impressions and what I’m liking so far:

### 1. **Performance**
   - Hugo provides incredible speed in building sites.

### 2. **Dependencies** 
   - Hugo comes in a single binary file with no dependencies, while Jekyll requires Ruby and several Ruby gems, which can sometimes lead to compatibility or dependency issues.

### 3. **Plugins**
   - Hugo does not rely on plugins the way Jekyll does, which need to be managed, updated, and may cause compatibility issues, might break with updates or cause security concerns. Everything is built-in.

### 4. **Native Taxonomy Support**
   - Hugo supports taxonomies like tags, categories, or custom classifications out of the box. Jekyll can support these too, but it often requires plugins, which adds complexity like I mentioned above.

### 5. **Deployment and CI/CD Integration**
   - Hugo like Jekyll integrates seamlessly with CI/CD systems like GitHub Actions, Netlify, Vercel, and Cloudflare (which I'm currently using), but require no additional setup to manage Ruby dependencies, and it's damn fast.

### 6. **Shortcodes**
   - Hugo’s shortcodes allow to embed rich content within Markdown, such as custom HTML, code snippets.

What I haven’t been loving about Hugo:

### 1. **Templating**
   - Although there is a powerfull templating system, I haven't been able to make a couple of things work yet, like styling my rss and sitemap files. Jekyll templating system is way easier to work with.

I'm still not totally happy with the blog's result, but I'm getting there. I still have a lot to learn about the template system, so I can feel as comfortable as I did with Jekyll.

On the bright side, I don’t have to worry about Ruby and its dependencies anymore, and now I'm able to deploy my site in just a few seconds, which is pretty exciting.