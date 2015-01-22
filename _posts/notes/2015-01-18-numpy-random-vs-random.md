---
layout: article
title: "Numpy.random vs Random"
date: 2015-01-18T19:29:34-0800
modified:
categories: notes
comments: true
excerpt:
tags: [python, numpy, random]
ads: true
image:
  feature:
  teaser:
---


There are two python random generators, `numpy.random` and `random` modules. From the reference [^1], It turns out that `numpy.random` object is not thread-safe unless you make a local copy of the `numpy.random.Random class`. `random` module on the other hand, is thread safe and is lightweight.

[^1]: Differences between numpy.random and random.random in Python <http://stackoverflow.com/questions/7029993/differences-between-numpy-random-and-random-random-in-python>
