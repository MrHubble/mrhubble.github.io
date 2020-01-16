---
id: 200
title: Webpacker and css
date: 2019-09-06T03:48:52+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=200
permalink: /webpacker-and-css/
categories:
  - Rails
---
Rails 6 ships with webpacker by default for **javascript only**. Sprockets will still be used for css.

### Why would we want to use webpacker for css?<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.02.38-pm.png" alt="" class="wp-image-201" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.02.38-pm.png 449w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.02.38-pm-300x114.png 300w" sizes="(max-width: 449px) 100vw, 449px" /> <figcaption><https://github.com/rails/rails/pull/33079#issuecomment-400273182></figcaption></figure> <figure class="wp-block-image"><img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.03.12-pm-700x195.png" alt="" class="wp-image-202" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.03.12-pm-700x195.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.03.12-pm-300x84.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.03.12-pm.png 763w" sizes="(max-width: 700px) 100vw, 700px" /><figcaption><https://github.com/rails/rails/pull/33079#issuecomment-400759113></figcaption></figure>

I think the main benefit may be Hot **Module** Reload (HMR), which is different to Hot **Live** Reload. Hot **Live** Reload means a full page refresh occurs on file change. **HMR** avoids the full page reload.

### Why wasn&#8217;t webpacker used for css by default?

<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.13.44-pm-700x142.png" alt="" class="wp-image-204" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.13.44-pm-700x142.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.13.44-pm-300x61.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.13.44-pm.png 755w" sizes="(max-width: 700px) 100vw, 700px" /> <figcaption><https://github.com/rails/rails/pull/33079#issuecomment-400771732></figcaption></figure>

<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.15.03-pm-700x237.png" alt="" class="wp-image-205" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.15.03-pm-700x237.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.15.03-pm-300x102.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.15.03-pm.png 752w" sizes="(max-width: 700px) 100vw, 700px" /> <figcaption><https://github.com/rails/rails/pull/33079#issuecomment-405560290></figcaption></figure>

### How do we use webpacker for css?

I believe there are two different ways to use webpacker for css, however, with both approaches you **must** set `extract_css: true` in `webpacker.yml`. I also believe you should use the `stylesheet_pack_tag` helper in your layout/view.

The two approaches:

#### 1. Add a .scss file to your /packs directory

For example:

`// app/javascript/packs/applications.scss`
`<br>@import '../src/stylesheets/our_test.scss';`

#### 2. Import you css in your javascript pack

For example:

`// app/javascript/packs/applications.js<br><br>import "../src/stylesheets/application"`

## The stylesheet\_pack\_tag

Both of the above solutions would use `<%= stylesheet_pack_tag 'application', 'data-turbolinks-track': 'reload' %>`

In my experience I needed to use that helper. However, I have also read that it&#8217;s **not** needed:<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-12.42.33-pm-700x590.png" alt="" class="wp-image-206" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-12.42.33-pm-700x590.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-12.42.33-pm-300x253.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-12.42.33-pm-768x647.png 768w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-12.42.33-pm.png 927w" sizes="(max-width: 700px) 100vw, 700px" /> <figcaption><https://medium.com/@adrian_teh/ruby-on-rails-6-with-webpacker-and-bootstrap-step-by-step-guide-41b52ef4081f></figcaption></figure>

## What was this talk of HMR?

After you&#8217;ve setup your css to go through webpacker, I believe to enable Hot Module Reloading, all you need to do is set `hmr: true` in `webpacker.yml`

And then apparently this also means something:<figure class="wp-block-image">

<img src="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.47.39-pm-700x76.png" alt="" class="wp-image-207" srcset="https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.47.39-pm-700x76.png 700w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.47.39-pm-300x33.png 300w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.47.39-pm-768x84.png 768w, https://www.mrcodebot.com/wp-content/uploads/2019/09/Screen-Shot-2019-09-06-at-1.47.39-pm.png 901w" sizes="(max-width: 700px) 100vw, 700px" /> <figcaption><https://github.com/rails/webpacker#paths></figcaption></figure>