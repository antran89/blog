---
layout: article
title: "Making a Caffe Layer"
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
* /src/caffe/layers/new_layer.cu
* /src/caffe/test/test_new_layer.cpp

### caffe.proto

You have to give new index to your new layer. Look for `next available ID`. There are two lines containing the sentence. Increment the next available ID and define the new layer.

### Defining a layer

The layer has to be a child of the `Layer` virtual class. The virtual functions that you have to implement are the ones defined as  `= 0` which are

~~~
virtual void Reshape(const vector<Blob<Dtype>*>& bottom,
  vector<Blob<Dtype>*>* top) = 0;
virtual void Forward_cpu(const vector<Blob<Dtype>*>& bottom,
  vector<Blob<Dtype>*>* top) = 0;
virtual void Backward_cpu(const vector<Blob<Dtype>*>& top,
  const vector<bool>& propagate_down,
  vector<Blob<Dtype>*>* bottom) = 0;
~~~

Implement the functions and make sure to define in the header. Either common_layers.hpp or vision_layers.hpp is fine. Finally implement the test functions in `/src/caffe/test/test_new_layer.cpp`

~~~
make
make test
./build/test/test_new_layer.testbin
~~~

### Test Predefined Functions

There are several predifined test functions. Use those functions to unit-test your function.

    EXPECT_NEAR
    EXPECT_GE
    EXPECT_LE
    EXPECT_EQ

Check Backprop using `GradientChecker`

### Implementation Detail

* Blob offset

    When you compute the offset from the blob pointer, use the safe `offset(n,c)` function.

* `caffe_[mul|add|sub|div|sqr|powx|exp|abs|sin|cos|copy|scal|cpu_axpby]`

    Basic elementwise functions and matrix multiplication jjprovided in `/caffe/util/math_functions.hpp`. 

* CUDA Macros

    There are several CUDA macros which come very handy when making `Forward_gpu` and `Backward_gpu`

        // CUDA: grid stride looping
        #define CUDA_KERNEL_LOOP(i, n) \
          for (int i = blockIdx.x * blockDim.x + threadIdx.x; \
               i < (n); \
               i += blockDim.x * gridDim.x)


* CUDA brief summary

    I've been using CUDA for my research since CUDA 4.5 but the concept of grid, thread, blocks is sometimes confusing. Since caffe also requires cuda programming, I guess it's a great time to quickly summarize the concept of threads, blocks and grids and some basics.

    Each thread will be executed in parallel (in theory but stream processors can sometimes handle multiple threads)


    All CUDA kernels must starts with either `__device__` or `__global__`. `__device__` functions is only accessible from CUDA kernels. whereas `__global__` functions can be launched from CPU side.



## Angle To Trigonometric Layer

The layer takes $N \times C \times 1 \times 1$ `Blob` and produces $N \times 2C \times 1 \times 1$ `Blob`. The angle must be in radian (which is none of our concern since the NN weight will adjust automatically).

For each input it produces two values $sin(x)$ and $cos(x)$. Let’s concatenate $n$ sines with $n$ cosines. If we define $y_i = \sin(x_i)$ and $y_{i+C} = \cos(x_i)$, the gradient will be

$$
\begin{align}
\frac{\partial E(y_i, y_{i+C}, \dots)}{\partial x_i} & = \frac{\partial E(y_i, \dots)}{\partial y_i} \frac{\partial y_i}{\partial x_i} + \frac{\partial E(y_{i + C}, \dots)}{\partial y_{i+C}} \frac{\partial y_{i + C}}{\partial x_i}\\
& = \frac{\partial E(y_i, \dots)}{\partial y_i} \frac{\partial \sin(x_i)}{\partial x_i} + \frac{\partial E(y_{i + C}, \dots)}{\partial \cos(x_i)} \frac{\partial y_{i + C}}{\partial x_i}\\
& = \frac{\partial E(y_i, \dots)}{\partial y_i} \cos(x_i) - \frac{\partial E(y_{i + C}, \dots)}{\partial \cos(x_i)} \sin(x_i)
\end{align}
$$

The $\frac{\partial E(y_i, \dots)}{\partial y_i}$ is defined in `top[n]->[c|g]pu_diff`

#### angle_to_trig.cu

~~~cuda
#include <algorithm>
#include <vector>

#include "caffe/layer.hpp"
#include "caffe/util/math_functions.hpp"
#include "caffe/vision_layers.hpp"

namespace caffe {


/* Define automatic type switching cuda sin,cos functions
 * [sin|cos]f is single precision trigonometric functions
 * [sin|cos] is double precision trigonometric functions
 */
template <typename Dtype>
Dtype auto_type_cos(Dtype x);

template <typename Dtype>
Dtype auto_type_sin(Dtype x);

template<>
__device__ float auto_type_cos<float>(const float x){  return cosf(x);}
template<>
__device__ double auto_type_cos<double>(const double x){ return cos(x);}
template<>
__device__ float auto_type_sin<float>(const float x){  return sinf(x);}
template<>
__device__ double auto_type_sin<double>(const double x){  return sin(x);}


template <typename Dtype>
__global__ void AngleToTrigLayerForward(const int n, const int n_channel,
    const Dtype* in, Dtype* out) {
  CUDA_KERNEL_LOOP(index, n) {
    unsigned int data_index = index / n_channel;
    unsigned int channel_index = index % n_channel;
    unsigned int top_index = 2 * n_channel * data_index + channel_index;

    out[top_index] =
        auto_type_sin<Dtype>(in[index]);
    out[top_index + n_channel] =
        auto_type_cos<Dtype>(in[index]);
  }
}

template <typename Dtype>
void AngleToTrigLayer<Dtype>::Forward_gpu(const vector<Blob<Dtype>*>& bottom,
    vector<Blob<Dtype>*>* top) {
  const Dtype* bottom_data = bottom[0]->gpu_data();
  Dtype* top_data = (*top)[0]->mutable_gpu_data();
  const int count = bottom[0]->count();
  const int n_channel = bottom[0]->channels();
  // NOLINT_NEXT_LINE(whitespace/operators)
  AngleToTrigLayerForward<Dtype><<<CAFFE_GET_BLOCKS(count), CAFFE_CUDA_NUM_THREADS>>>(
      count, n_channel, bottom_data, top_data);
  CUDA_POST_KERNEL_CHECK;
}

template <typename Dtype>
__global__ void AngleToTrigLayerBackward(const int n, const int n_channel, 
    const Dtype* top_diff, const Dtype* top_data, Dtype* bottom_diff) {
  CUDA_KERNEL_LOOP(index, n) {
    unsigned int data_index = index / n_channel;
    unsigned int channel_index = index % n_channel;
    unsigned int top_index = 2 * n_channel * data_index + channel_index;
    bottom_diff[index] =
        top_diff[top_index] * top_data[top_index + n_channel]
        - top_diff[top_index + n_channel] * top_data[top_index];
  }
}

template <typename Dtype>
void AngleToTrigLayer<Dtype>::Backward_gpu(const vector<Blob<Dtype>*>& top,
    const vector<bool>& propagate_down,
    vector<Blob<Dtype>*>* bottom) {
  if (propagate_down[0]) {
    const Dtype* top_diff = top[0]->gpu_diff();
    const Dtype* top_data = top[0]->gpu_data();
    Dtype* bottom_diff = (*bottom)[0]->mutable_gpu_diff();
    const int count = (*bottom)[0]->count();
    const int n_channel = (*bottom)[0]->channels();
    // NOLINT_NEXT_LINE(whitespace/operators)
    AngleToTrigLayerBackward<Dtype><<<CAFFE_GET_BLOCKS(count), CAFFE_CUDA_NUM_THREADS>>>(
        count, n_channel, top_diff, top_data, bottom_diff);
    CUDA_POST_KERNEL_CHECK;
  }
}


INSTANTIATE_CLASS(AngleToTrigLayer);


}  // namespace caffe
~~~

[^1]: <http://caffe.berkeleyvision.org>

