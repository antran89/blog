---
layout: article
title: "IPython embed"
date: 2015-07-30T10:33:10-0700
modified:
categories: notes
comments: true
excerpt:
tags: [ipython, embed, debug]
ads: true
toc: true
image:
  feature:
  teaser:
---


If you want to start interactive python session in the middle of python code.
The following trick might be useful.

Put the following line in the section you want to start interactive python session.

{% highlight python %}
  from IPython import embed; embed()
{% endhighlight %}
