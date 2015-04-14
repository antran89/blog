---
layout: article
title: "Andrew Maas Deep Neural Networks in Speech Recognition"
date: 2015-04-13T17:26:02-0700
modified:
categories: research
excerpt:
comments: true
mathjax: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

### HMM-GMM Speech Recognition

Naive inference on HMM depends only on the previous stats and the input feature which does not work well. DNN works on par or better than HMM after the network was scaled as the size of the data.


### Impact of Depth

Depth for small dataset may work as a regularization.

### HMM-Free Recognition

(Graves & Jaitly. 2014) first beat the HMM-GMM model by replacing the top HMM layer with the character classification.

HMM-GMM usually outputs a word that is in the dictionary so if it somehow infer a character differently, it generate a very different word. But DNN outputs a pronunciation that so the output makes more sense that the HMM-GMM.

He used a Bi-directional RNN and it learns to align a phonem correctly.

### Recurrence

Compare to the feed forward network, having a recurrent layer improve performance significantly. Adding backward recurrent connection and making it bi-directional additionally helps the performance.


### No Language Model

Can predict a word that was not on the training data.
