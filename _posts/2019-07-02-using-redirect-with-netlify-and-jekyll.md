---
layout: post
title:  "Using redirects with Netlify and Jekyll"
date:   2019-07-01 15:33:00 -0500
permalink: /redirects-netlify-and-jekyll/
---
This site is made using Jekyll and hosted on Netlify. I was surprised to learn that Netlify has support for redirects. Here's how you can configure it for Jekyll(or any static generator for that matter)

### Step 1: Create a _redirects file in project root

Create a _redirects file in the project root. Populate it redirect url and the urls you want them to point to. For example I wanted to point /resume to /about 

```
/resume  /about
```

You can also add comments using #

### Step 2: Include _redirects in _config.yml

This is a Jekyll specific step. You will need to include _redirects in config.yml. In config.yml add an includes entry like so

```
include: [_redirects]
```

### Step 3: Deploy to Netlify

Deploy your site to Netlify and you are done. If you enter <your_base_url>/resume you are redirected to <your_base_url>/about 
