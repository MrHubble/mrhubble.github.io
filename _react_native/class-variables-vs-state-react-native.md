---
id: 52
title: Class variables vs state with React Native
date: 2018-02-12T04:20:31+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=52
permalink: /2018/02/12/class-variables-vs-state-react-native/
dsq_thread_id:
  - "6474088489"
categories:
  - React Native
---
Global variables are assigned on app load. For example:

    import React, { Component } from 'react'
    import {
      StyleSheet,
      Text,
      View,
    } from 'react-native'
    const exampleGlobal = 'Helloooo, global variable here'
    

`exampleGlobal` will be executed when we run `react native run ios`.

Class variables will be assigned on loading of a component (ie when we navigate to the screen with the component). As the class variable declaration occurs in the `contructor` method, we can not assign a value to the class variable in another method after the contructor. For example, this will work:

    class ImaClass extends Component {
      constructor() {
        this.dog = 'woof'
       }
    
      componentDidMount() {
        if (this.dog == 'tweet')
          console.log('That's not a dog')
        }
    

whereas this will _not_ work as we like:

    class ImaClass extends Component {
      constructor() {
        this.cat = null
      }
    
      componentDidMount() {
        this.cat = 'meow'
        if (this.cat == 'meow')
          console.log('Ye ol cat features')
      }
    

If we want to set a component value after the constructor then we should use `this.setState({cat})` for example.