---
layout: article
title: "LaTeX Beamer Notes"
date: 2015-04-28T21:12:57-0700
modified:
categories: notes
comments: true
excerpt:
tags: [beamer]
ads: true
image:
  feature:
  teaser:
---

## Numbered figure/table

By default, beamer does not number figures or tables.
Use the following command to make beamer to print numbers [^1].

{% highlight latex %}
\setbeamertemplate{caption}[numbered]
{% endhighlight %}

## Beamer item font size

Change the `\normalfont` to another font size.

{% highlight latex %}
\setbeamertemplate{itemize/enumerate body begin}{\normalfont}
\setbeamertemplate{itemize/enumerate subbody begin}{\normalfont}
\setbeamertemplate{itemize/enumerate subsubbody begin}{\normalfont}
{% endhighlight %}

[^1]: http://yihui.name/en/2010/08/numbered-figure-table-captions-in-beamer/
