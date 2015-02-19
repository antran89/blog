---
layout: article
title: "Ctags Notes"
date: 2015-02-15T16:47:52-0800
modified:
categories: notes
comments: true
excerpt:
tags: [tag, ctags]
ads: true
image:
  feature:
  teaser:
---

Ctag command notes

## Cuda tags[^1]

{% highlight bash %}
ctags -R --langmap=c++:+.cu *
ctags -R --langmap=c++:+.cuh *
{% endhighlight %}


## Tagbar CUDA


[^1]: http://stackoverflow.com/questions/10277883/ctagstaglist-for-cu-cuda-files
