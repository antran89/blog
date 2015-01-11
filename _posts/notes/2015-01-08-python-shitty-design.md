---
layout: article
title: "Strange Numpy and Matrix in Python"
date: 2015-01-04T23:14:02-08:00
modified:
categories: notes
excerpt: "Some python functionalities are pointless"
comments: true
tags: [numpy]
ads: true
image:
  feature:
  teaser:
---

Some confusing functionalities in numpy/python that aren’t consistent

~~~python
>>> a = np.ndarray([1,10])
>>> a[1]
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
IndexError: index 1 is out of bounds for axis 0 with size 1
>>> a[0,1]
0.0
~~~

I don’t get the point of this redundant index.


~~~python
>>> np.zeros((1,5))
array([[0., 0., 0., 0., 0.]])
>>> np.zeros((1,5))[0]
array([0., 0., 0., 0., 0.])
~~~

