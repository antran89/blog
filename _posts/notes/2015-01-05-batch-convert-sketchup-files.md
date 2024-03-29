---
layout: article
title: "Batch Convert Sketchup Files on OSX"
date: 2015-01-05T22:33:34-08:00
modified:
categories: notes
excerpt: "Some problems while converting skp models on OSX"
comments: true
tags: [SketchUp, export, OSX]
ads: true
image:
  feature:
  teaser:
---

When converting skp files into different format using Sketchup API on OSX, I found erratic behaviors. For instance,

`Sketchup.active_model`

will return fail if your focus is not on Sketchup!. Check the [API](http://www.sketchup.com/intl/en/developer/docs/ourdoc/sketchup#active_model).

The reason is that for MacOS, you can open multiple documents which forces the Sketchup to use the focused window. (whereas on Windows, you can only open one document)

Very erratic design choice, Sketchup could have returned the most recently focused model instead.

Also even after closing the model using `model.close` or `Sketchup.send_action(‘closeDocument:’)` does not cleanup the memory.
