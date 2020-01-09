---
id: 5
title: Turbolinks native wrappers and Service Workers for offline first app
date: 2018-01-04T00:41:28+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=5
permalink: /2018/01/04/turbolinks-native-wrappers-and-service-workers/
dsq_thread_id:
  - "6389914126"
categories:
  - Uncategorized
---
We had a need for an &#8220;offline first&#8221; mobile app. Being part of a small team, if we can make this app utilising much of our existing codebase then all the better.

With the release of Turbolinks 5 came the release of the iOS and Android native wrappers for using your existing codebase with Turbolinks to release a hybrid app.

Unfortunately, at this point in time (Jan, 2018),  [ServiceWorkers currently aren&#8217;t supported on iOS or Android webviews](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker) , so it can&#8217;t be used in combination with Turbolinks for offline first apps.

We instead decided to use React Native with [Realm Database](https://realm.io/products/realm-database) and our existing Rails API.

References:

https://github.com/turbolinks/turbolinks/issues/196

https://gorails.com/episodes/how-to-use-turbolinks-clearCache#comment-3169959601