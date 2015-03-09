---
layout: post-header
excerpt: ""
title: "Margin vs. Padding"
tags: [CSS, HTML]
---

In the process of making my personal website, I discovered the difference between the two in CSS, which is that margins auto-collapse while padding doesn't. If we had two elements side by side and they had paddings of 1em each, then the total difference between the two elements would be 2em because padding is seen as part of an html object. However, if we were to replace it with margins of 1em, the distance between the two elements would only be 1em, because margins are seen as not part of the html object and can overlap.