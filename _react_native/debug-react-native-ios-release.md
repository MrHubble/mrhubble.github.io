---
id: 137
title: Debug React Native iOS release
date: 2018-11-20T01:20:47+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=137
permalink: /2018/11/20/debug-react-native-ios-release/
categories:
  - Uncategorized
---
If you&#8217;re having problems with your iOS release then it may mean there was a problem with the build and `main.jsbundle` was not created. To begin with, delete `appName/ios/main.jsbundle` then attempt to manually create a new production release build:

<pre class="wp-block-preformatted"><code>react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ios/main.jsbundle --assets-dest ios</code></pre>

You may find there&#8217;s a problem with your build such as `maximum call stack size exceeded`. For me, this was fixed with the magical: `rm -rf node_modules`

When there are no problems, `main.jsbundle` should be created automatically when building a production release.

Also during the normal production build process, `--dev=false` will be used to set `__DEV__` to `false`. <https://stackoverflow.com/a/34317341/1299792>  
[https://docs.expo.io/versions/latest/workflow/how-expo-works#publishingdeploying-an-expo-app-in-production](http://how expo works)

**Background on building a production release**

<blockquote class="wp-block-quote">
  <p>
    During the development process, React Native has loaded your JavaScript code dynamically at runtime. For a production build, you want to pre-package the JavaScript bundle and distribute it inside your application. Doing this requires a code change in your code so that it knows to load the static bundle.<br /> This will now reference the main.jsbundle resource file that is created during the Bundle React Native code and images Build Phase in Xcode.
  </p>
</blockquote>

[https://facebook.github.io/react-native/docs/running-on-device](http://running on a device)