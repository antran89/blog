---
layout: article
title: "Mathjax + SASS makes green equations!"
date: 2015-01-05T23:50:32-08:00
modified:
categories: notes
excerpt: ""
comments: true
tags: [mathjax, jekyll]
ads: true
image:
  feature:
  teaser:
---

# Problem

Using SASS (SCSS) pygments causes mathjax to render equations in green

# Cause

In the `_pygments.scss`, there are classes that MathJax also uses : `.mi` and `.mo` classes

{% highlight css %}
.mi { color: #009999 }                     // Literal.Number.Integer 
.mo { color: #009999 }                     // Literal.Number.Oct 
{% endhighlight %}
