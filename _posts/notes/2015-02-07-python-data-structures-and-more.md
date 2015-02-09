---
layout: article
title: "Python Data Structure Internal Mechanism"
date: 2015-02-07T14:04:31-0800
modified:
categories: notes
comments: true
excerpt:
tags: [python, data structures]
ads: true
image:
  feature:
  teaser:
---

When you are working with fairly large amount of growing data, the internal mechanism of the data structures affect the performance of your program. Recently, I’m working on synthetic data generation for neural network training and the growing size of data can be a source of problem since simple `append` on a `list` could copy all the data everytime the size changes.

I found nice articles summarizing the internal mechanisms of the python `list` and `dictionary` data structures.

<http://www.laurentluce.com/posts/python-list-implementation/>
<http://www.laurentluce.com/posts/python-dictionary-implementation/>

The list can keep a pointer to a datum if it is immutable and works similarly to `vector` in C++. The dictionary uses *open addressing* hashing to create a hash table and saves pointers to the data.

Many more data-structures are also available and a nice article summarized in a list <http://kmike.ru/python-data-structures/>
