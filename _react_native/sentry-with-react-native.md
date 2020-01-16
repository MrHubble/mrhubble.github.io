---
id: 128
title: Sentry with React Native
date: 2018-09-13T04:55:06+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=128
permalink: /2018/09/13/sentry-with-react-native/
categories:
  - Uncategorized
---
### Why Sentry?

<blockquote class="wp-block-quote">
  <p>
    Originally we were using Crashlytics to report errors but found that although this is one of the most popular reporting tools for mobile apps, it does not work as well with React Native. The problem is that most of the the business logic is happening inside of JavaScript. When something goes wrong in JavaScript, the JavaScript thread throws an exception, which causes the whole app to crash. Crashlytics had no access to the original JavaScript error so we had no information on why the crash happened.<br /> This lead us to find Sentry which happens to handle JavaScript and React Native well.
  </p>
  
  <cite>https://tech.kartenmacherei.de/tracking-react-native-errors-with-sentry-3719f9b24836<br /></cite>
</blockquote>

### Bitcode

<blockquote class="wp-block-quote">
  <p>
    Bitcode is an intermediate representation of a compiled program. Apps you upload to iTunes Connect that contain bitcode will be compiled and linked on the App Store.
  </p>
  
  <cite>https://stackoverflow.com/a/31088744/1299792<br /></cite>
</blockquote>

### huh?

<blockquote class="wp-block-quote">
  <p>
    Before producing actual machine code, compiler first creates an intermediate representation of the code called bitcode. It is not your code, bot not yet a machine code either. If you chose to enable bitcode in your project options and to upload bitcode to iTunes connect, Apple can actually recompile your application on their servers while applying the latest and greatest compiler optimizations. Even after you’ve stopped maintaining your app and not uploading new versions, they can still continue to optimize your binary as new compiler features are introduced. The problem with this approach is, of course, the dSYM files that were generated in your Xcode locally do not match the binaries that your end users are getting, meaning your users’ crashes can not be properly symbolicated neither locally nor by a third party crash report tool
  </p>
  
  <cite>https://www.bugsee.com/blog/ios-crash-symbolication-bitcode/</cite>
</blockquote>

### Do I need to use Bitcode?

No. 

<blockquote class="wp-block-quote">
  <p>
    For iOS apps, bitcode is the default, but optional. If you provide bitcode, all apps and frameworks in the app bundle need to include bitcode. For watchOS and tvOS apps, bitcode is required.
  </p>
  
  <cite>https://stackoverflow.com/a/34202574/1299792<br /></cite>
</blockquote>

### Should I use Bitcode? What&#8217;s the advantage?  


<blockquote class="wp-block-quote">
  <p>
    Including bitcode will allow Apple to re-optimize your app binary in the future without the need to submit a new version of your app to the store.
  </p>
  
  <cite>https://stackoverflow.com/a/34202574/1299792</cite>
</blockquote>

<blockquote class="wp-block-quote">
  <p>
    Bitcode is awesome. It lets our users download much smaller versions of our apps, containing only the bits they need to run it on their devices.
  </p>
  
  <cite>https://littlebitesofcocoa.com/225-downloading-dsyms-with-fastlane</cite>
</blockquote>

### What does this have to do with Sentry?

<blockquote class="wp-block-quote">
  <p>
    A dSYM upload is required for Sentry to symoblicate your crash logs for viewing. The symoblication process unscrambles Apple’s crash logs to reveal the function, variables, file names, and line numbers of the crash. The dSYM file can be uploaded through the sentry-cli tool or through a Fastlane action.
  </p>
  
  <p>
    If Bitcode is enabled in your project, you will have to upload the dSYM to Sentry after it has finished processing in the iTunesConnect. We also recommend using the latest Xcode 8.1 version for submitting your build. The dSYM can be downloaded in three ways.
  </p>
  
  <cite>https://docs.sentry.io/clients/cocoa/dsym/</cite>
</blockquote>

### Install Sentry

I ran into many issues with installing Sentry and I believe the below steps as shown [here](https://github.com/getsentry/react-native-sentry/issues/344#issuecomment-362354851) are what solved the problems for me:

  1. Do NOT use CocoaPods for `react-native-sentry`
  2. Follow the standard install steps:  
    a) `yarn add react-native-sentry`  
    b) `react-native link react-native-sentry`
  3. If your Podfile was modified by the setup script, remove any reference of ReactNativeSentry
  4. Open Xcode and drag `node_modules/react-native-sentry/ios/RNSentry.xcodeproj` into your project&#8217;s Libraries directory
  5. Open your project&#8217;s &#8220;Linked Frameworks and Libraries&#8221; and add `libRNSentry.a`

#### Remove debug upload from Sentry build

Seeing as we&#8217;re using Bitcode we need to prevent Sentry from automatically uploading the symbols in its build process:

<blockquote class="wp-block-quote">
  <p>
    Additionally we add a build script called “Upload Debug Symbols to Sentry” which uploads debug symbols to Sentry.<br />However this will not work for bitcode enabled builds. If you are using bitcode you need to remove that line (sentry-cli upload-dsym) and consult the documentation on dsym handling instead (see With Bitcode).
  </p>
  
  <cite>https://docs.sentry.io/clients/react-native/manual-setup/#build-steps</cite>
</blockquote>

In an attempt to remove this I removed the references to &#8220;Upload Debug Symbols to Sentry&#8221; in my \`project.pbxproj\` file:  


<figure class="wp-block-image"><img src="https://www.mrcodebot.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-13-at-2.54.08-pm.png" alt="" class="wp-image-131" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-13-at-2.54.08-pm.png 521w, https://www.mrcodebot.com/wp-content/uploads/2018/09/Screen-Shot-2018-09-13-at-2.54.08-pm-300x107.png 300w" sizes="(max-width: 521px) 100vw, 521px" /></figure> 

### Setting up Sentry

Install the cli with:

<pre class="wp-block-preformatted">curl -sL https://sentry.io/get-cli/ | bash<br /></pre>

and then setup with:

<pre class="wp-block-preformatted">sentry-cli login</pre>

### Using Fastlane to upload dsym to Sentry  


<pre class="wp-block-preformatted">fastlane add_plugin sentry</pre>

Create a lane:

<pre class="wp-block-preformatted">fastlane add_plugin sentry</pre>

<pre class="wp-block-preformatted">lane :refresh_dsyms do
    download_dsyms
    sentry_upload_dsym(
        auth_token: ENV['SENTRY_AUTH_TOKEN'],
        org_slug: ENV['SENTRY_ORG_SLUG'],
        project_slug: ENV['SENTRY_PROJECT_SLUG']
    )
    clean_build_artifacts
end</pre>