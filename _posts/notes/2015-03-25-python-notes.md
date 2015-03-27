---
layout: article
title: "Python notes"
date: 2015-03-25T05:58:53-0700
modified:
categories: notes
comments: true
excerpt:
tags: [python]
ads: true
image:
  feature:
  teaser:
---

An article from PayPal explains about common enterprise python applications. It
also goes into python concurrency packages especially futures and GIL which I
did not know that python has such functionalities.
[10 Myths of Enterprise Python](https://www.paypal-engineering.com/2014/12/10/10-myths-of-enterprise-python/)

## Maintaining row/column orientation

If you want to extract a column from a numpy matrix `M`, you will get a vector
that has a trailing comma in the `M.shape`. Coming from a background of Matlab,
this is very annoying feature but which allows lazy computation. To avoid such
unisance, use `M[:,1:2]` if you want to extract the second column.

[link](http://stackoverflow.com/questions/15165170/how-do-i-maintain-row-column-orientation-of-vectors-in-numpy)

## Google style doc

A stackoverflow answer summarized various python documentation styles. Particularly,

{% highlight python %}
"""
This is a groups style docs.

Parameters:
    param1 - this is the first param
    param2 - this is a second param

Returns:
    This is a description of what is returned

Raises:
    KeyError - raises an exception
"""
{% endhighlight %}

[link](http://stackoverflow.com/questions/3898572/what-is-the-standard-python-docstring-format)

## PyPy vs CPython

Must read answers before you want to use one of these python compile languages.
[link](http://stackoverflow.com/questions/18946662/why-shouldnt-i-use-pypy-over-cpython-if-pypy-is-6-3-times-faster)
