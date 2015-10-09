---
layout: article
title: "Caffe Python Layer"
date: 2015-07-21T17:36:42-0700
modified:
categories: research
excerpt:
comments: true
tags: []
toc: true
ads: true
image:
  feature:
  teaser:
---

Python layer in Caffe can speed up development process [Issue1703](https://github.com/BVLC/caffe/pull/1703)

## Compile WITH_PYTHON_LAYER option

First, you have to build Caffe with `WITH_PYTHON_LAYER` option [^1]. Run `make
clean` to delete all the compiled binaries. Then,

{% highlight bash %}
WITH_PYTHON_LAYER=1 make && make pycaffe
{% endhighlight %}

If you skip this, caffe will complain that layer factory function can't find Python layer.

{% highlight bash %}
layer_factory.hpp:77] Check failed: registry.count(type) == 1 (0 vs. 1) Unknown layer type: Python
{% endhighlight %}

## Python Layer

A [gist](https://gist.github.com//shelhamer/8d9a94cf75e6fb2df221) from Evan
Shelhamer summarizes the basics of the python layer.

{% highlight python %}
...
layer {
  type: 'Python'
  name: 'loss'
  top: 'loss'
  bottom: 'ipx'
  bottom: 'ipy'
  python_param {
    # the module name -- usually the filename -- that needs to be in $PYTHONPATH
    module: 'pyloss'
    # the layer name -- the class name in the module
    layer: 'EuclideanLossLayer'
  }
  # set loss weight so Caffe knows this is a loss layer
  loss_weight: 1
}
{% endhighlight %}

So you have to define a python layer that is defined in your $PYTHONPATH.

So the module name should be `pyloss.py`.

{% highlight python %}
import caffe
import numpy as np

class EuclideanLossLayer(caffe.Layer):

    def setup(self, bottom, top):
        # check input pair
        if len(bottom) != 2:
            raise Exception("Need two inputs to compute distance.")

    def reshape(self, bottom, top):
        # check input dimensions match
        if bottom[0].count != bottom[1].count:
            raise Exception("Inputs must have the same dimension.")
        # difference is shape of inputs
        self.diff = np.zeros_like(bottom[0].data, dtype=np.float32)
        # loss output is scalar
        top[0].reshape(1)

    def forward(self, bottom, top):
        self.diff[...] = bottom[0].data - bottom[1].data
        top[0].data[...] = np.sum(self.diff**2) / bottom[0].num / 2.

    def backward(self, top, propagate_down, bottom):
        for i in range(2):
            if not propagate_down[i]:
                continue
            if i == 0:
                sign = 1
            else:
                sign = -1
            bottom[i].diff[...] = sign * self.diff / bottom[i].num
{% endhighlight %}

[^1]: https://github.com/BVLC/caffe/issues/2093
