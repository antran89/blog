---
layout: article
title: "Theano Notes"
date: 2015-03-20T00:56:33-0700
modified: 2015-04-06T17:37:09-0700
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

[Theano tutorial](http://nbviewer.ipython.org/github/craffel/theano-tutorial/blob/master/Theano%20Tutorial.ipynb)

[Theano AlexNet](https://github.com/uoguelph-mlrg/theano_alexnet)

## Theano RNN

[link](http://stackoverflow.com/questions/24431621/does-theano-do-automatic-unfolding-for-bptt)

The package defines models for classification, autoencoding, regression, and prediction. Models can easily be created with any number of feedforward or recurrent layers and combined with different regularizers:
[link](https://github.com/lmjohns3/theanets)

### Theano RNN 

The repo contains basic Theano RNN implementation as well as references 
[Theano-RNN](https://github.com/gwtaylor/theano-rnn)

### Cursive Letter

[Generating Sequences With Recurrent Neural Networks](http://arxiv.org/pdf/1308.0850v5.pdf)

## Lasagne

[Neural Network Tools for Theano](https://github.com/benanne/Lasagne)

### Theano RNN

[RNN with LSTM](https://github.com/rakeshvar/rnn_ctc)


## Clean CNN implementation in Theano

[Theanet](https://github.com/rakeshvar/theanet/)

### Theano zero padding

Zero padding the input shape
{% highlight python %}
v = theano.shared(
    voxels.reshape(
        (1, n_vox, 1, n_vox, n_vox)
    ),
    name='v',
    borrow=True
)
x = tensor.alloc(0.0, 1, n_vox + 2*pad, 1, n_vox + 2*pad, n_vox + 2*pad)
x = tensor.set_subtensor(x[:, pad:n_vox+pad, :, pad:n_vox+pad, pad:n_vox+pad], v)
x.name = 'x'
{% endhighlight %}

### Setting up DNN Support

You have to setup `$CUDA_ROOT` and add the cuDNN library path to `LD_LIBRARY_PATH`, `LIBRARY_PATH` and `CPATH`[^1].
If you do not want to set up the environment variable, please visit [^1] and see alternative options.

Next, you have to use the latest cuDNN library[^2]. If not `Theano` will automatically fails when you use `DNN` package.

#### Convolution Backend Comparison

I used NVIDIA GTX660 for the following experiments. To test the performance, I used a LeNet implementation on Lasagne[^3]. The codes are publicly available at [^4].

1. theano.nnet.conv2d `mnist_conv.py`

<pre>
tarting training...
Epoch 1 of 500 took 3.922s
  training loss:                1.333408
  validation loss:              0.273623
  validation accuracy:          92.01 %%
Epoch 2 of 500 took 3.829s
  training loss:                0.314819
  validation loss:              0.158999
  validation accuracy:          95.27 %%
Epoch 3 of 500 took 3.813s
  training loss:                0.215939
  validation loss:              0.119314
  validation accuracy:          96.28 %%
Epoch 4 of 500 took 3.803s
  training loss:                0.173297
  validation loss:              0.099131
  validation accuracy:          97.04 %%
</pre>

2. cuda_convnet `mnist_conv_cc.py`

<pre>
tarting training...
Epoch 1 of 500 took 4.960s
  training loss:                1.436164
  validation loss:              0.294884
  validation accuracy:          91.20 %%
Epoch 2 of 500 took 4.843s
  training loss:                0.334583
  validation loss:              0.169420
  validation accuracy:          94.83 %%
Epoch 3 of 500 took 4.808s
  training loss:                0.229397
  validation loss:              0.127022
  validation accuracy:          96.27 %%
Epoch 4 of 500 took 4.791s
  training loss:                0.178838
  validation loss:              0.103492
  validation accuracy:          96.96 %%
</pre>

3. cuDNN `mnist_conv_dnn.py`

<pre>
tarting training...
Epoch 1 of 500 took 2.999s
  training loss:                1.235465
  validation loss:              0.260454
  validation accuracy:          92.68 %%
Epoch 2 of 500 took 2.982s
  training loss:                0.304258
  validation loss:              0.153187
  validation accuracy:          95.47 %%
Epoch 3 of 500 took 2.983s
  training loss:                0.207364
  validation loss:              0.113122
  validation accuracy:          96.56 %%
Epoch 4 of 500 took 2.983s
  training loss:                0.167065
  validation loss:              0.095353
  validation accuracy:          97.08 %%
</pre>

Compare to 4.9s of cuda_convnet and 3.9s of theano.nnet.conv2d, 2.98s of cuDNN is for mnist 1 epoch is pretty impressive.


[^1]: http://deeplearning.net/software/theano/tutorial/using_gpu.html
[^2]: https://developer.nvidia.com/cuDNN
[^3]: https://github.com/benanne/Lasagne
[^4]: https://github.com/benanne/Lasagne/tree/master/examples
