---
id: 168
title: Turbolinks calls Stimulus connect twice
date: 2019-02-01T02:28:31+00:00
author: finally
layout: rails
guid: https://www.mrcodebot.com/?p=168
permalink: /turbolinks-calls-stimulus-connect-twice/
categories:
  - Rails
---
With Turbolinks, when you _click_ to return to a page (not using the browser back button), Turbolinks will display a cached preview of the page while it attempts to grab the content from the server.

If you have a Stimulus object on the page (`data-controller=`), then the controller will be initialised _twice_. It will be called once for the cached page, and then again for the fresh page content.

It took me a while to figure out what was going on here and I’m really surprised that the Stimulus Turbolinks “One Two Punch” doesn’t handle this differently. It’s expected behaviour according to one of the Basecamp devs:

<https://discourse.stimulusjs.org/t/controller-initialized-twice-when-visiting-from-a-turbolinks-page/17/2>