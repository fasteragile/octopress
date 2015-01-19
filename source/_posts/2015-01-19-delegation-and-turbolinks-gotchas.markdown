---
layout: post
title: "Delegation and turbolinks gotchas"
date: 2015-01-19 17:31:54 -0600
comments: true
categories: [JavaScript]
---

When you bind a click through delegation on document, you can't do so inside
a page:load turbolinks event or the observer will bind to the element for each
turbolinks click event as you click through to your target element. 

Discuss why this happens

Show an example with code and a video (gif maybe) showing the multiple post requests we were seeing on garnet hill

Offer the solution
