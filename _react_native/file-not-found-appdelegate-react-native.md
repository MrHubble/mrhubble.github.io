---
id: 85
title: File not found in AppDelegate with React Native
date: 2018-03-24T08:16:58+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=85
permalink: /2018/03/24/file-not-found-appdelegate-react-native/
categories:
  - React Native
---
If you&#8217;re seeing the error `x.h file not found` in AppDelegate.m in Xcode then try this solution:

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.10.50-pm.png" alt="" width="1174" height="596" class="alignnone size-full wp-image-86" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.10.50-pm.png 1174w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.10.50-pm-300x152.png 300w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.10.50-pm-768x390.png 768w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.10.50-pm-700x355.png 700w" sizes="(max-width: 1174px) 100vw, 1174px" /> 

<https://github.com/facebook/react-native/issues/12077#issuecomment-302318906>

If that doesn&#8217;t fix it then try:

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.17.47-pm.png" alt="" width="1442" height="394" class="alignnone size-full wp-image-91" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.17.47-pm.png 1442w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.17.47-pm-300x82.png 300w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.17.47-pm-768x210.png 768w, https://www.mrcodebot.com/wp-content/uploads/2018/03/Screen-Shot-2018-03-24-at-7.17.47-pm-700x191.png 700w" sizes="(max-width: 1442px) 100vw, 1442px" /> 

<https://github.com/facebook/react-native/issues/12077#issuecomment-318364850>

This exact solution worked for me for `RCTBundleURLProvider.h file not found`. I also used a similar solution to fix `AppCenterReactNativeCrashes.h file not found`.