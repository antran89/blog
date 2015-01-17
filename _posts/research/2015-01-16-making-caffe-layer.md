---
layout: article
title: "Making Caffe Layer"
except: ""
date: 2015-01-16T19:49:25-0800
modified:
ads: true
category: research
tags: [Caffe, Layer]
image:
  feature: 
comments: true
mathjax : true
---

Caffe is one of the most popular open-source neural network implementation [^1]. It is modular, clean and fast. To modify the network to my liking, I defined a layer that produces cosine and sine of radian inputs.


### Files to modify or create

Relative from the `$(CAFFE_HOME)`

* /src/caffe/proto/caffe.proto
* /include/caffe/common_layers.hpp or vision_layers.hpp
* /src/caffe/layer_factory.cpp
* /src/caffe/layers/new_layer.cpp
* /src/caffe/layers/new_layer.cu (optional for gpu)
* /src/caffe/test/test_new_layer.cpp

### caffe.proto

You have to give new index to your new layer. Look for `next available ID`. There are two lines containing the sentence. Increment the next available ID and define the new layer.

### Defining a layer

The layer has to be a child of the `Layer` virtual class. The virtual functions that you have to implement are the ones defined as  `= 0` which are

    virtual void Reshape(const vector<Blob<Dtype>*>& bottom,
      vector<Blob<Dtype>*>* top) = 0;
    virtual void Forward_cpu(const vector<Blob<Dtype>*>& bottom,
      vector<Blob<Dtype>*>* top) = 0;
    virtual void Backward_cpu(const vector<Blob<Dtype>*>& top,
      const vector<bool>& propagate_down,
      vector<Blob<Dtype>*>* bottom) = 0;

Implement the functions and make sure to define in the header. Either common_layers.hpp or vision_layers.hpp is fine. Finally implement the test functions in `/src/caffe/test/test_new_layer.cpp`

~~~
make
make test
./build/test/test_new_layer.testbin
~~~


[^1] : http://caffe.berkeleyvision.org

