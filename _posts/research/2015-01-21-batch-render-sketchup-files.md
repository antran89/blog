---
layout: article
title: "Batch Render Sketchup Files"
date: 2015-01-05T22:33:34-08:00
modified:
categories: research
excerpt:
comments: true
tags: [SketchUp, render]
toc: true
ads: true
image:
  feature:
  teaser:
---


Rendering sketchup models.

## Image Thumbnail

There is a built in SketchUp command that renders a thumbnail.

{% highlight ruby %}
Sketchup.save_thumbnail(“/path/to/file.skp”, “/path/to/save/image.png”)
{% endhighlight %}

The function doesn’t even open the file and creates a small thumbnail usually of size 128x128

## Write Image function

If you want more control over the rendering. Checkout the `View.write_image` function.

{% highlight ruby %}
keys = {
    :filename => “/path/to/dir/image.png”,
    :width => 640,
    :height => 480,
    :antialias => false,
    :compression => 0.9,
    :transparent => true
}
model = Sketchup.active_model
view = model.active_view
view.write_image(keys)
{% endhighlight %}

[^1]: <http://www.sketchup.com/intl/en/developer/docs/ourdoc/view>
