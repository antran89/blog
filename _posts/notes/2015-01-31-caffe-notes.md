---
layout: article
title: "CNN Framework Caffe Notes"
date: 2015-01-31T15:34:42-0800
modified:
categories: notes
comments: true
excerpt:
tags: [caffe, cnn, neural network]
ads: true
image:
  feature:
  teaser:
---

Notes for myself

## Dimension of Blob

Num training x Channel x Height x Width

## Caffe Usage

Caffe usage[^1]

Add caffe to `$PATH`.

{% highlight bash %}
caffe train -solver examples/mnist/lenet_solver.prototxt
{% endhighlight %}

## Solver, Train Val Prototxt

solver.prototxt
{% highlight bash %}

{% endhighlight %}

train_val.prototxt


[^1] http://caffe.berkeleyvision.org/gathered/examples/imagenet.html
