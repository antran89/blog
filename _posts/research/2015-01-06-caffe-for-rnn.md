---
layout: article
title: "Caffe for Recurrent Neural Network implementation"
date: 2015-01-06T10:32:45-08:00
modified:
categories: research
excerpt: "Caffe is one of the most famous CNN implementation. But with some modifications, it can be used for RNN"
comments: true
mathjax: true
tags: [RNN, Caffe]
ads: true
image:
  feature:
  teaser:
---

[Caffe](http://cafffe.berkeley.org) is one of the most famous Convolutional Neural Network implementations. It is fast, easy to use and read the codes. However, currently, there is no Recurrent Neural Network layers for Caffe. The networks must be Directed Acyclic Graph to use backpropagation on Caffe.

But since the RNN training, Back Propagation Through Time (BPTT), can be done in the same ways as training CNN using backpropagation, we can modify layers and implement RNN BPTT using Caffe.

First, we first roll out the network. And force the weights to be the same for the networks. Let $U,V,W$ be the weights of non-linear functions and $A,B,C$ be the output blob (Using Caffe terminology). 

<img src="{{ site.url }}/images/research/RNN0.png">

Then, we roll out for time and examine the network.

<img src="{{ site.url }}/images/research/RNN1.png">

After brief observation, it is clear that the nodes satisfy Markov property!. We can extract the common structure and can train feed-forward network.

<img src="{{ site.url }}/images/research/RNN2.png">
