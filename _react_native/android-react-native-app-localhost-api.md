---
id: 34
title: Android React Native app with localhost API
date: 2018-01-22T00:03:18+00:00
author: finally
layout: react_native
guid: https://www.mrcodebot.com/?p=34
permalink: /android-react-native-app-localhost-api/
dsq_thread_id:
  - "6429201377"
categories:
  - React Native
---
You may have your own API of which you need to connect your React Native app to. Naturally you&#8217;d like to test this locally in development rather than in production on the server. With Xcode this is easier as it uses a simulator, whereas Android Studio uses an emulator which makes it much more challenging to connect to localhost. Developing/testing on a physical device is a more desirable solution for me also as it means Android Studio doesn&#8217;t chew up all of my RAM.

I tried [multiple](https://stackoverflow.com/a/4779992/1299792) [Stackoverflow](https://stackoverflow.com/questions/4779963/how-can-i-access-my-localhost-from-my-android-device/9515366#9515366) solutions but found that when your development API uses a subdomain (ie `api.localhost:5000`) the api won&#8217;t get picked up as a subdomain. We can&#8217;t use an alias either (ie `myapi.com`) because the Android device doesn&#8217;t have the alias to point it to the loopback address. This is only possible on rooted phones after you modify the android `/etc/hosts` file as described [here](https://blog.dandyer.co.uk/2012/01/12/accessing-local-name-based-virtual-hosts-from-the-android-emulator/).

A good way around this is to use [ngrok](https://ngrok.com) which you can install with [homebrew](https://brew.sh). Then in the terminal run:

`ngrok http -region=au -host-header=rewrite api.lvh.me:3000`

I use my region (`au`) for speedier results, and ngrok needs to point to the subdomain for my api. This has been set up by adding `127.0.0.1    api.lvh.me` to my [hosts file](https://www.imore.com/how-edit-your-macs-hosts-file-and-why-you-would-want)

In your React Native code you can now use ngrok to access your localhost API, ie:

    if (__DEV__)
      return 'https://your_random_address.au.ngrok.io'