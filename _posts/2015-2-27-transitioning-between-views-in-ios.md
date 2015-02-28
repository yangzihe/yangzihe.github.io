---
title: "Transitiong between views in iOS"
tags: [iOS Developemnt, Xcode]
---

Watching tutorials on iOS development in Swift at the moment and just wanted to share what I learned. For apps that have more than one view, we add additional views to the Main.storyboard file by finding a ViewController object and dragging it onto the storyboard. After this, we need to dictate the logic such that we can arrive at the new view from the starting view. This is done via **segues**. To add a segue, ctrl + click from one view to the other. There are multiple types of segues, but the two main kinds are:

*   Modal: Used when the view you want to transition to is _self contained_. What this means is that basically, the new view has its own distinct logic or functionality in the context of the workflow. Two examples would be an app that transitions to a view that told you to either create a new account or use facebook to sign in and a calendar app when you click create new event. Both of these views have their own functionality (signing in and creating an event) that doesn't depend on anything from the previous view. The user also can't interact with the previous view until they've dismissed the modal view.

*   Push: A push segue pushes the view you're transitioning to on top of the view controller stack. It commonly used for hierarchial relationships, where the new view displays sub-items, for example. You can send data between the two views through the <code>prepareForSegue</code> function. A good example of this type of segue is the settings app. It has a bunch of different types of settings, and by selecting a certain type, you're brought to another view with more detailed settings options. When the push segue is used, a *back* button that points to the previous view is automatically generated.
-----
Cool shortcuts:
*   Cmd + Shft + O: Open a finder/alfred-like dialog that can take you anywhere.