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

[Caffe](http://github.com/BVLC/caffe) uses Google Protocol buffer and LMDB,
LevelDB to save data in a single unified database file. This allows faster data
loading which is crucial if you cannot cache all your data.

This type of database is a standard for Caffe and I’ll cover how to load such
database in Python.

## Saving Database in LMDB

I will not cover this step. If you are using ImageNet, CIFAR10, MNIST or some
common datasets, please refer to [Caffe examples](http://caffe.berkeleyvision.org/gathered/examples/cifar10.html#prepare-the-dataset)
to make LMDB or LevelDB databases.

## Loading the Dataset

All the data has to be first converted into a common protobuf Caffe `Datum`
format. The protobuf message for the `Dataum` is defined in `caffe.proto` file.

Copy the following excerpt from `caffe.proto` to `datum.proto` and follow the
[instruction](https://developers.google.com/protocol-buffers/docs/pythontutorial)
and compile it.

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

## Loading a binaryproto file

The mean of the data sometimes drastically affect the training. `Caffe`
provides an elegant way to compute mean of data and creates `mean.binaryproto`
file. To load, use the following excerpt from `Caffe` issue pages [^1].

Create `blob.proto` and put the following scripts into the file.
{% highlight cpp %}
// Specifies the shape (dimensions) of a Blob.
message BlobShape {
  repeated int64 dim = 1 [packed = true];
}

message BlobProto {
  optional BlobShape shape = 7;
  repeated float data = 5 [packed = true];
  repeated float diff = 6 [packed = true];

  // 4D dimensions -- deprecated.  Use "shape" instead.
  optional int32 num = 1 [default = 0];
  optional int32 channels = 2 [default = 0];
  optional int32 height = 3 [default = 0];
  optional int32 width = 4 [default = 0];
}

// The BlobProtoVector is simply a way to pass multiple blobproto instances
// around.
message BlobProtoVector {
  repeated BlobProto blobs = 1;
}
{% endhighlight % }

To load the file in python, compile the above `blob.proto` (or just put the above proto on datum).

{% highlight python %}
mean_blob = blob_pb2.BlobProto()
data = open(os.path.join(LMDB_PATH, "mean.binaryproto"), 'rb').read()
mean_blob.ParseFromString(data)
img_mean = np.array(mean_blob.data).reshape(mean_blob.num,
                                            mean_blob.channels,
                                            mean_blob.height,
                                            mean_blob.width)
{% endhighlight %}

[^1]: https://github.com/BVLC/caffe/issues/290
