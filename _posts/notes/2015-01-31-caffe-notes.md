---
layout: article
title: "CNN Framework Caffe Notes"
date: 2015-02-23T15:10:59-0800
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

## Caffe LSTM

Unrolled LSTM training available [^3]
<https://github.com/jeffdonahue/caffe/tree/recurrent>


## Pythonification of Caffe

Pycaffe supports loading and learning parameters in ‘Python’!

## Python Net Specification

Instead of using protocol buffer to define a network, a pull request produces a nice programming way to define a caffe network

<https://github.com/BVLC/caffe/pull/1733>


## New learning rate modification

New version of Caffe is available with the latest protobuf.

blob_lr is replaced by lr_mult
param { lr_mult: 1}

## FreezeLayer

New layer called FreezeLayer stops the layer modification. Good for fine-tuning a network.


[^1]: http://caffe.berkeleyvision.org/gathered/examples/imagenet.html
[^2]: http://stackoverflow.com/questions/28031841/hdf5-diag-error-detected-in-hdf5-1-8-11
[^3]: https://github.com/BVLC/caffe/pull/1873
