---
id: 13
title: Convert vanilla React Native app to Expo
date: 2018-01-07T08:28:05+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=13
permalink: /2018/01/07/convert-vanilla-react-native-app-to-expo/
dsq_thread_id:
  - "6397116779"
categories:
  - React Native
---
Being new to React Native I was looking for a way to easily test my app on a physical device and I came across Expo. I had heard that setting up Android Studio was a painful experience ([according to a video in this course](https://www.udemy.com/react-native-advanced/)) and also wasn&#8217;t enjoying running xcode on my basic 2012 Macbook Air. To use Expo I would need to convert my existing React Native app to an Expo app.

<div>
  A basic example is discussed here: <a href="https://github.com/expo/xde#converting-an-existing-project-to-work-with-expo" target="_blank" rel="noopener" data-saferedirecturl="https://www.google.com/url?hl=en-GB&q=https://github.com/expo/xde%23converting-an-existing-project-to-work-with-expo&source=gmail&ust=1515286848635000&usg=AFQjCNF0wU8AJUdJGeOuNV3ue9LdT1ig5w">https://github.com/expo/<wbr />xde#converting-an-existing-<wbr />project-to-work-with-expo</a>
</div>

<div>
</div>

<div>
  However, my app makes use of libraries that use native code (with <i>react-native link</i>) and therefore I would need to detach the app and use ExpoKit: <a href="https://docs.expo.io/versions/v15.0.0/guides/detach.html" target="_blank" rel="noopener" data-saferedirecturl="https://www.google.com/url?hl=en-GB&q=https://docs.expo.io/versions/v15.0.0/guides/detach.html&source=gmail&ust=1515286848636000&usg=AFQjCNFiu7gpufJtpjQZ3GvDV1uLd36kdA">https://docs.expo.io/<wbr />versions/v15.0.0/guides/<wbr />detach.html</a>
</div>

&nbsp;

I decided that converting to expo, detaching and using ExpoKit all for the purpose of testing on my actual device probably wasn&#8217;t the smartest thing to do. I played around with Visual Studio App Center and TestFlight but I&#8217;ll discuss that in another post.

The best solution for me was to get a more powerful development machine and [use xcode and Android Studio to test on my phone](https://facebook.github.io/react-native/docs/running-on-device.html)

Here is a very related post on the problems faced when converting an existing app to Expo. The author faced issues with using TypeScript, Relay, ExNavigation (rather than React Navigation), XDE and native dependencies. <a href="https://medium.com/2-minute-revolution-developer-blog/our-review-of-expo-and-why-we-dont-think-it-s-ready-for-prime-time-bb0897657295" target="_blank" rel="noopener" data-saferedirecturl="https://www.google.com/url?hl=en-GB&q=https://medium.com/2-minute-revolution-developer-blog/our-review-of-expo-and-why-we-dont-think-it-s-ready-for-prime-time-bb0897657295&source=gmail&ust=1515357844011000&usg=AFQjCNE06-A-H1kfwlMbIl-rITKqmC1JaQ">https://medium.<wbr />com/2-minute-revolution-<wbr />developer-blog/our-review-of-<wbr />expo-and-why-we-dont-think-it-<wbr />s-ready-for-prime-time-<wbr />bb0897657295</a>