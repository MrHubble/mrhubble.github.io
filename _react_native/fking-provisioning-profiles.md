---
id: 36
title: 'F$%king provisioning profiles'
date: 2018-01-22T23:01:53+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=36
permalink: /2018/01/22/fking-provisioning-profiles/
dsq_thread_id:
  - "6431496453"
categories:
  - React Native
---
When I first started out with React Native and Xcode I struggled greatly with provisioning profiles. Luckily [fastlane](https://fastlane.tools) was there to save the day.

However, today I went to run my React Native app on my iOS device through Xcode and got the old `A valid provisioning profile for this executable was not found` error. I was able to find some helpful advice:

> You can&#8217;t install a build that was signed with the app store distribution provisioning profile and certificate. It will fail to install on the device if you try. You need to use either a development profile, or an enterprise distribution profile to install on test devices. The iOS Distribution certificate can only be used to build an app that will be installed via the App Store.

https://stackoverflow.com/a/40305294/1299792

So I make sure I&#8217;m using my development profiles and try to run it again but still no dice. Another suggested solution was to delete myProjectTests&#8230;.nope, didn&#8217;t work.

What worked for me was to delete the `DerivedData` by going to `Xcode > Preferences > Locations` to get the folder location of the derived data, then manually deleting all the contents of that folder ([stackoverflow answer](https://stackoverflow.com/a/44843912/1299792)).