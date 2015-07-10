---
layout: article
title: "Doxygen, Doxypy and Doxypypy"
date: 2015-07-07T22:14:13-0700
modified:
categories: notes
comments: true
excerpt:
tags: [doxygen, doxypy]
ads: true
toc: true
image:
  feature:
  teaser:
---


Doxygen is one of the most popular code documentation generation tool. However, Doxygen can’t properly handle python docstring. Instead, by preprocessing the python files using doxygen, you can use Doxygen commands inside the python documentation string [^1][^2]. There is a similar package, doxypyp, which tires to replace all the Doxygen specific commands with human readable strings.

In this post, I’ll summarize and give a brief tutorial on how to use Doxypypy.

## Install Doxygen and Doxypypy

{% highlight bash %}
$ brew install doxygen
or
$ sudo apt-get install doxygen
$ pip install doxypypy
{% endhighlight %}

## Write documentation

Write documentations for python files like below. The following code snippet is excerpted from doxypypy sample code [^3].

{% highlight python %}
#!/usr/bin/env python
"""
Google Python Style Guide SampleClass

This is basically as close a copy of the Python examples in the Google
Python Style Guide as possible while still being valid code.

http://google-styleguide.googlecode.com/svn/trunk/pyguide.html?showone=Comments#Comments
"""


def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.  Silly things may happen if
    other_silly_variable is not None.

    Args:
        big_table: An open Bigtable Table instance.
        keys: A sequence of strings representing the key of each table row
            to fetch.
        other_silly_variable: Another optional variable, that has a much
            longer name than the other args, and which does nothing.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:
        {'Serak': ('Rigel VII', 'Preparer'),
         'Zim': ('Irk', 'Invader'),
         'Lrrr': ('Omicron Persei 8', 'Emperor')}
        If a key from the keys argument is missing from the dictionary,
        then that row was not found in the table.

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
    pass
{% endhighlight %}

For latex math equations, append `r` to make the docstring formatted in a raw string.

{% highlight python %}
class Polynomial(object):
    r"""Stores a polynomial.

    Here, a polynomial is defined as a finite series of the form
    @f[
      a_0 + a_1 x + a_2 x^2 + \cdots + a_N x^N,
    @f]
    where \f$ a_k \f$, for \f$k=0,\ldots,N\f$, are real coefficients, and
    \f$N\f$ is the degree of the polynomial.

    Attributes:
        coefficients:  A list of coefficients.
    """

    def __init__(self, coefficients):
        r"""Initialize a polynomial instance.
        Args:
            coefficients:  A list of coefficients. Beginning with \f$a_0\f$,
                           and ending with \f$a_N\f$.
        """
        self.coefficients = coefficients
{% endhighlight %}


## Create Doxyfile and edit

`Doxyfile` is required to convert your project into documentation.

Generate a default Doxyfile and modify so that it can use doxypypy as a preprocessing [^4][^5].

{% highlight bash %}
$ doxygen -g
$ $EDITOR Doxyfile

(add these lines to the end and comment them
out where they appear earlier)

# Customizations for doxypypy.py
FILTER_PATTERNS        = *.py=py_filter

$ $EDITOR py_filter
#!/bin/bash
python -m doxypypy.doxypypy -a -c $1

(save the above script)
{% endhighlight %}

## Run Doxygen

{% highlight bash %}
$ doxygen
{% endhighlight %}

## References

[^1]: [Stackoverflow doxypy](http://stackoverflow.com/questions/58622/can-i-document-python-code-with-doxygen-and-does-it-make-sense#answer-58701)
[^2]: [Doxygen commands](http://www.stack.nl/~dimitri/doxygen/manual/commands.html#cmdfdollar)
[^3]: [Doxypypy sample google](https://github.com/Feneric/doxypypy/blob/master/doxypypy/test/sample_google.py)
[^4]: [Doxypy Tutorial](http://notemagnet.blogspot.com/2009/10/using-doxypy-for-python-code.html)
[^5]: [Doxypypy Readme](https://github.com/Feneric/doxypypy)
