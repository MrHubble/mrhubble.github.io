---
id: 39
title: Remote debugging on iPhone / iOS device with React Native
date: 2018-01-23T01:38:35+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=39
permalink: /2018/01/23/remote-debugging-iphone-ios-device-react-native/
dsq_thread_id:
  - "6431698089"
categories:
  - React Native
---
Having a problem where shaking your iPhone won&#8217;t activate [the in-app developer menu](https://facebook.github.io/react-native/docs/debugging.html)? Yep, me too.

First, check your scheme to make sure your product is in debug mode:

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/01/Screen-Shot-2018-01-23-at-11.49.40-am.png" alt="" width="1766" height="932" class="alignnone size-full wp-image-40" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/01/Screen-Shot-2018-01-23-at-11.49.40-am.png 1766w, https://www.mrcodebot.com/wp-content/uploads/2018/01/Screen-Shot-2018-01-23-at-11.49.40-am-300x158.png 300w, https://www.mrcodebot.com/wp-content/uploads/2018/01/Screen-Shot-2018-01-23-at-11.49.40-am-768x405.png 768w, https://www.mrcodebot.com/wp-content/uploads/2018/01/Screen-Shot-2018-01-23-at-11.49.40-am-700x369.png 700w" sizes="(max-width: 1766px) 100vw, 1766px" /> 

_Product → Scheme → Edit Scheme_

Also, make sure you have your iPhone and your computer **on the same network.**

To get your iPhone to access a local development API on your computer, use [ngrok](https://ngrok.com), ie:

Start ngrok with something similar to:

    ngrok http -region=au -host-header=rewrite api.lvh.me:3000
    

and then in your React Native code:

    if (__DEV__)
      return 'https://myRandomAddress.au.ngrok.io'