---
id: 148
title: Binding in React Native
date: 2018-11-22T02:47:15+00:00
author: finally
layout: post
guid: https://www.mrcodebot.com/?p=148
permalink: /2018/11/22/binding-in-react-native/
categories:
  - Uncategorized
---
In his course _React for Beginners_, Wes Bos recommends we using class fields/properties rather than normal functions that are bound in the constructor. I believe the official react docs also used to recommend this approach but now they [warn that this is experimental](https://reactjs.org/docs/handling-events.html)

Example of class fields/properties:

    class MyClass extends Component {
      myMethod = () => {}
    }
    

`myMethod` is a class property which you can call in React with `onPress={this.myMethod}`

Note that you don’t use `()` with `onPress={this.myMethod()}` because that would call the function as soon as the screen is mounted.

I think this is much better than the deprecated `this.myMethod.bind(this)`

I don’t think you can send parameter with class fields/properties so if the method receives a parameter I like to use arrow in render:

    onPress={() => this.myMethod(myVariable)}
    

Another thing I found, in my testing I couldn’t use the class property or arrow functions for the deprecated `ListView`. Why I am still using `ListView`? I am using it in conjunction with Realm and it is highly recommended that you use the `ListView` and `ListView.DataSource` provided by the realm/react-native module

Maybe that was just me messing things up. Not sure.

I continued to use class properties when I was writing normal js classes (not just jsx for React) because apparently it has advantages. One disadvantage is it gets messed up if you try to use `super.myMethod`. I don’t completely understand this but using `this` rather than `super` seemed to work for me.

Direction the community is taking:

<img src="https://www.mrcodebot.com/wp-content/uploads/2018/11/Screen-Shot-2018-11-22-at-1.44.05-pm.png" alt="" width="647" height="610" class="alignnone size-full wp-image-152" srcset="https://www.mrcodebot.com/wp-content/uploads/2018/11/Screen-Shot-2018-11-22-at-1.44.05-pm.png 647w, https://www.mrcodebot.com/wp-content/uploads/2018/11/Screen-Shot-2018-11-22-at-1.44.05-pm-300x283.png 300w" sizes="(max-width: 647px) 100vw, 647px" /> 

<https://twitter.com/housecor/status/921841848641097728>