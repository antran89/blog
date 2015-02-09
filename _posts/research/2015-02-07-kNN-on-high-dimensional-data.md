---
layout: article
title: "K-NN on High Dimensional Data"
date: 2015-02-04T19:22:48-0800
modified:
categories: research
excerpt:
comments: true
tags: [kNN]
toc: true
ads: true
image:
  feature:
  teaser:
---

For high dimensional data, k-Nearest Neighbor (kNN) method becomes very expensive. One way to handle this is Approximate Nearest Neighbor (ANN) using Locality Sensitive Hashing (LSH) technique.

Distance metric also matters. In high-dimensional space, all the points are very far away from each others in Euclidean distance. Cosine similarity is a common way to compare high-dimension vectors
http://stackoverflow.com/questions/5751114/nearest-neighbors-in-high-dimensional-data
http://www.mobvis.org/publications/OmercevicDrbohlavLeonardis-iccv2007.pdf
https://www.cs.umd.edu/~hjs/mkbook/chapter4.pdf