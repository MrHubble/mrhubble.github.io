---
id: 61
title: CodePush defaults
date: 2018-02-15T23:49:26+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=61
permalink: /2018/02/15/codepush-defaults/
dsq_thread_id:
  - "6482713884"
categories:
  - React Native
---
By default CodePush will **check** for updates (`codePush.CheckFrequency`) whenever the app&#8217;s process is started([reference](https://github.com/Microsoft/react-native-code-push/blob/master/docs/api-js.md#checkfrequency)):

    codePush.CheckFrequency.ON_APP_START
    

By default CodePush will **install** updates (`codePush.InstallMode`) whenever the app is restarted (either by the user or the OS):

`codePush.InstallMode.ON_NEXT_RESTART`

This means that if a user constantly keeps your app in the background and never restarts their phone nor the app then it may be a long time before they receive the updates. Therefore, I think a better default is to check for updates every time your app resumes to the foreground from the background, and install any updates (if present) on resume also:

`checkFrequency: codePush.CheckFrequency.ON_APP_RESUME, installMode: codePush.InstallMode.ON_NEXT_RESUME`

MS docs on defaults: <https://docs.microsoft.com/en-us/appcenter/distribution/codepush/react-native>

Sample code: <https://github.com/Microsoft/react-native-code-push/blob/master/docs/api-js.md#codepush>