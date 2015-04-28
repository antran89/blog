---
layout: article
title: "Neural Network Weight Initialization"
date: 2015-04-16T16:25:46-0700
modified:
categories: research
excerpt:
mathjax: true
comments: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

In Neural Network training, random initialization is not as important as
training procedures such as RMSProb, SGD, Momentum, Nesterov’s, etc. However,
setting the right scale for weights can impact the training time.

In [Lasagne](), the weight is appropriately scaled.

$$
S = \sqrt\frac{c}{N_i + N_o}
$$

For instance, for a convolution layer, $N_i = C_i \times H_W \times W_W$, $N_o
= C_o \times H_W \times W_W$. When we have a pooling layer this could be scaled
again $N_o = C_o \times H_W \times W_W / (H_p \times W_p)$ where $H_W$ and $W_W$
are the height and width of the convolution filter and $H_p$ and $W_p$ are the
width and height of the pooling region.

## Caffe Imagenet Reference model

It uses Caffe’s `caffe_rng_gaussian` function to fill out the blobs. Each layer
has a different standard deviation, and the way by which each layer is defined
consists the following

<pre>
conv1: 0.01
conv2: 0.01
conv3: 0.01
conv4: 0.01
conv5: 0.01
fc6: 0.005
fc7: 0.005
fc8: 0.01
</pre>

## Caffe googlenet

It uses `xavier` (I’m not sure what it refers to) to initialize all layers.

If we see the method,

{% highlight cpp %}
/**
 * @brief Fills a Blob with values @f$ x \sim U(-a, +a) @f$ where @f$ a @f$
 *        is set inversely proportional to the number of incoming nodes.
 *
 * A Filler based on the paper [Bengio and Glorot 2010]: Understanding
 * the difficulty of training deep feedforward neuralnetworks, but does not
 * use the fan_out value.
 *
 * It fills the incoming matrix by randomly sampling uniform data from
 * [-scale, scale] where scale = sqrt(3 / fan_in) where fan_in is the number
 * of input nodes. You should make sure the input blob has shape (num, a, b, c)
 * where a * b * c = fan_in.
 *
 * TODO(dox): make notation in above comment consistent with rest & use LaTeX.
 */
template <typename Dtype>
class XavierFiller : public Filler<Dtype> {
 public:
  explicit XavierFiller(const FillerParameter& param)
      : Filler<Dtype>(param) {}
  virtual void Fill(Blob<Dtype>* blob) {
    CHECK(blob->count());
    int fan_in = blob->count() / blob->num();
    Dtype scale = sqrt(Dtype(3) / fan_in);
    caffe_rng_uniform<Dtype>(blob->count(), -scale, scale,
        blob->mutable_cpu_data());
    CHECK_EQ(this->filler_param_.sparse(), -1)
         << "Sparsity not supported by this Filler.";
  }
};
{% endhighlight %}

The code basically returns an uniform-sampled array. The scale is defined as

$$
S = \sqrt{\frac{3}{|\{W \}|}}
$$

where $|\{W\}|$ is the cardinality of the weights (bad math notation).
Interestingly, from the `bvlc_googlenet/train_val.prototxt`, `std` (presumably standard deviation) is defined but it wasn’t used in the Initialization.

{% highlight bash %}
convolution_param {
  num_output: 64
  kernel_size: 1
  weight_filler {
    type: "xavier"
    std: 0.1
  }
  bias_filler {
    type: "constant"
    value: 0.2
  }
}
{% endhighlight %}


