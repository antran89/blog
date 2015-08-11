---
layout: article
title: "CNN Framework Caffe Notes"
date: 2015-02-25T14:26:05-0800
modified: 2015-07-31T11:33:00-0700
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

Unrolled LSTM training available[^3]
<https://github.com/jeffdonahue/caffe/tree/recurrent>


## Pythonification of Caffe

Pycaffe supports loading and learning parameters in ‘Python’![^4]

{% highlight python %}
pretrained_net = caffe.Net(
   "net.prototxt", "net.caffemodel")
solver = caffe.SGDSolver("solver.prototxt")
solver.net.copy_from(pretrained_net)
solver.solve()
{% endhighlight %}

## Python Net Specification

Instead of using protocol buffer to define a network, a pull request produces a nice programming way to define a caffe network

<https://github.com/BVLC/caffe/pull/1733>


## New learning rate modification

New version of Caffe is available with the latest protobuf.

`blob_lr` is replaced by `lr_mult`

{% highlight bash %}
param { lr_mult: 1}
{% endhighlight %}

## FreezeLayer

New layer called `FreezeLayer` stops the layer modification. Good for fine-tuning a network.

## Caffe Validation

When you are using the `test_iter`, make sure that the number of iterations and the batch size matches the number of test images.


## Annoying Installation bugs

This is a system specific bugs that I got while compiling caffe master branch (technically http://github.com/chrischoy/caffe-cudnn2)

- /usr/local/lib/libgflags.a: could not read symbols: Bad value

    What is this? Just download the recent gflags from https://github.com/gflags/gflags and install.

- Cannot find library

    Caffe makefile does not by default use $LIBRARY_PATH, $LD_LIBRARY_PATH environment variables. Just hard set the LIBRARY_PATH in Makefile.config to include the path you want to include

    /usr/bin/ld: cannot find -lcblas


## Compiling Caffe when you don't have sudo access

When you are working on servers where you don't have a sudo access, things can be more difficult than it should be.

1. Use anaconda

    At least this will install lapack and atlas which will 
    conda install atlas

2. Add anaconda/lib to Makefile.config and system env variable $LD_LIBRARY_PATH

    This will save installing lapack and ATLAS

3. Download cuDNN and copy the header to include path and static and dynamic libraries to lib path

    /path/to/your/custom/include
    /path/to/your/custom/lib

4. Install necessary packages

    For my case, I had to compile new gflags, boost, 

## Theano Caffe Interoperability

  <https://github.com/tnarihi/tnarihi-caffe-helper/blob/master/python/caffe_helper/theano_util.py>

[^1]: http://caffe.berkeleyvision.org/gathered/examples/imagenet.html
[^2]: http://stackoverflow.com/questions/28031841/hdf5-diag-error-detected-in-hdf5-1-8-11
[^3]: https://github.com/BVLC/caffe/pull/1873
[^4]: http://vision.stanford.edu/teaching/cs231n/slides/evan.pdf
