---
layout: post-header
title: "On UILabels"
tag: [Swift, iOS Development, Xcode]
permalink: /blog/:title
---

When transitioning views in an app, if you want to send data from the current view controller to the next one, you can't directly change the properties of UI elements of the new view, because they haven't been loaded yet. Trying to do so will crash your app. To work around this, create a new variable that isn't a UI element in the second View Controller class, and set that to the value that you want. This won't crash because a normal variable isn't a UI element, and so we don't have to consider whether or not it's already been loaded. Then in the <code>viewDidLoad()</code> function, set the UI element equal to the variable. This way, the UI element will be set to the value you want before the user interacts with the new view, but after the view has been completely loaded.