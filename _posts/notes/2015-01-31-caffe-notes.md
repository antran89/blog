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

## HDF5

Strangely, for HDF5 data format, one should provide a textfile that contains a path to the HDF5 file.[^2]

{% highlight bash %}
hdf5_data_param {
   source: “train.txt”
   batch_size: 10
}
{% endhighlight %}


[^1] http://caffe.berkeleyvision.org/gathered/examples/imagenet.html
[^2] http://stackoverflow.com/questions/28031841/hdf5-diag-error-detected-in-hdf5-1-8-11
