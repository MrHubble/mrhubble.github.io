---
id: 93
title: Submit new build version to App store
date: 2018-03-28T21:56:24+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=93
permalink: /2018/03/28/submit-new-build-version-to-app-store/
categories:
  - React Native
---
Use either [fastlane deliver](https://docs.fastlane.tools/actions/deliver/) or [follow this guide](https://customersupport.doubledutch.me/hc/en-us/articles/229159407-iOS-How-to-Update-an-App-in-iTunes-Connect) to use iTunes Connect

## Fastlane deliver

If you&#8217;ve already used fastlane to upload your build to testflight, grab all of your existing metadata with `fastlane deliver download_metadata`. Then submit the existing build `fastlane deliver submit_build --build_number 830`