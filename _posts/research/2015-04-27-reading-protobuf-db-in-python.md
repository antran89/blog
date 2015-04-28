---
layout: article
title: "Reading protobuf DB in Python"
date: 2015-04-27T20:05:48-0700
modified:
categories: research
excerpt:
comments: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

[Caffe](http://github.com/BVLC/caffe) uses Google Protocol buffer and LMDB, LevelDB to save data in a single unified files. This allows faster data loading which is crucial if you cannot cache all your data. Like the protobuf + lmdb combination, I will summarize steps to load such database in python.

## Saving Database in LMDB

I will not cover this step. If you are using ImageNet, CIFAR10, MNIST or some common datasets, please refer to [Caffe examples](http://caffe.berkeleyvision.org/gathered/examples/cifar10.html#prepare-the-dataset) to make LMDB or LevelDB databases.

## Loading the Dataset

All the data has been first converted into a common protobuf Caffe `Datum` format. The protobuf message for the `Dataum` is defined in `caffe.proto` file.

Copy the following excerpt from `caffe.proto` to `datum.proto` and follow the [instruction](https://developers.google.com/protocol-buffers/docs/pythontutorial) and compile it.

{% highlight cpp %}
message Datum {
  optional int32 channels = 1;
  optional int32 height = 2;
  optional int32 width = 3;
  // the actual image data, in bytes
  optional bytes data = 4;
  optional int32 label = 5;
  // Optionally, the datum could also hold float data.
  repeated float float_data = 6;
  // If true data contains an encoded image that need to be decoded
  optional bool encoded = 7 [default = false];
}
{% endhighlight %}

## Reading LMDB CIFAR10 in Python

{% highlight python %}
import os
import lmdb
import numpy
import matplotlib.pyplot as plt

# First compile the Datum, protobuf so that we can load using protobuf
# This will create datum_pb2.py
os.system('protoc -I={0} --python_out={1} {0}datum.proto'.format("./", "./"))

import datum_pb2

LMDB_PATH = "/path/to/caffe/examples/cifar10/cifar10_train_lmdb/"

env = lmdb.open(LMDB_PATH, readonly=True, lock=False)

visualize = True

datum = datum_pb2.Datum()
with env.begin() as txn:
    cur = txn.cursor()
    for i in xrange(10):
        if not cur.next():
            cur.first()
        # Read the current cursor
        key, value = cur.item()
        # convert to datum
        datum.ParseFromString(value)
        # Read the datum.data
        img_data = numpy.array(bytearray(datum.data))\
            .reshape(datum.channels, datum.height, datum.width)
        if visualize:
            plt.imshow(img_data.transpose([1,2,0]))
            plt.show()

        print key
{% endhighlight %}

