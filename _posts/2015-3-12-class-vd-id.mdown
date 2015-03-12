---
layout: post-header
title: Class vs. ID
tags: [HTML, CSS]
---

In HTML/CSS a class is denoted by the `class` keyword and id is denoted by the `id` keyword. So what's the difference? I see people using both a lot, and for some reason, my CSS linter tells me not to use an id as a selector.

* A class is not unique. This means that multiple elements can have the same class name. It also means that a particular element can have more than one class.

* An id (like the name suggests) is unique. A HTML element can only have one id, and furthermore, it's the only element that's allowed to have that id on the page. This is because of a useful feature in HTML5, which allows a user to instruct a page to scroll to an HTML element with a particular id. Thus, if two different elements had the same id, then the page wouldn't know which one to scroll to.