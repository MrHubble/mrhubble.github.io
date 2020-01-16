---
id: 73
title: Navigating back to an existing route with React Navigation
date: 2018-02-27T09:08:20+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=73
permalink: /2018/02/27/navigating-back-existing-route-react-navigation/
dsq_thread_id:
  - "6508217310"
categories:
  - React Native
---
By default React Navigation will add a route to the stack every time you navigate to it. This means if we press to move back and forth between our **Home** and **Profile** screens then our stack will contain multiple entries for **Home** and **Profile**. For example, we could imagine this stack looking like `[Home, Profile, Home, Profile, Home]`.

For me this caused a bug on one of my screens with an event for an external library being called for each time the route had been added to the stack. For reference, the external library was [react-native-signature-capture](https://github.com/RepairShopr/react-native-signature-capture) and it was my function called for the `onSaveEvent`.

With react-navigation >= 1.0.3 (1.0.3 has a fix for a bug [where the screen would freeze when using key](https://github.com/react-navigation/react-navigation/issues/3477)) we can supply a `key` attribute of the route we would like to navigate back to:

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/02/Screen-Shot-2018-02-27-at-6.58.07-pm.png" alt="" width="1498" height="852" class="alignnone size-full wp-image-74" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/02/Screen-Shot-2018-02-27-at-6.58.07-pm.png 1498w, https://www.mrcodebot.com/wp-content/uploads/2018/02/Screen-Shot-2018-02-27-at-6.58.07-pm-300x171.png 300w, https://www.mrcodebot.com/wp-content/uploads/2018/02/Screen-Shot-2018-02-27-at-6.58.07-pm-768x437.png 768w, https://www.mrcodebot.com/wp-content/uploads/2018/02/Screen-Shot-2018-02-27-at-6.58.07-pm-700x398.png 700w" sizes="(max-width: 1498px) 100vw, 1498px" /> [taken from the React Navigation docs](https://reactnavigation.org/docs/navigation-prop.html#navigate-link-to-other-screens)

In practice this works by getting your routes key and then passing as a param to the next route.

## 1&#46; Pass your key for your route:

    this.props.navigation.navigate({
      routeName: 'Profile',
      params: {
        homeKey: this.props.navigation.state.key
      }
    })
    

## 2&#46; Use that key to navigate back to the existing route in your stack:

    this.props.navigation.navigate({
      routeName: 'Home',
      params: { myParams },
      key: this.props.navigation.state.params.homeKey
    })
    

A good example can be found in [in the navigation playground](https://github.com/react-navigation/react-navigation/blob/95565487ec1cd209b8bcee6c962a67f85e497bb8/examples/NavigationPlayground/js/StacksWithKeys.js) where they manually pass the key value, ie `this.props.navigation.navigate({routeName: 'Profile', key: 'A'})`

Currently I prefer to use the key that is auto generated for me for example `"id-1519705322567-5"`.