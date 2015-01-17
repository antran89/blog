---
layout: article
title: "Mathjax Tips"
date: 2015-01-05T20:40:20-08:00
modified: 2015-01-14T01:41:41-0800
categories: notes
excerpt:
comments: true
tags: []
image:
  feature:
  teaser:
  thumb:
---

### Curly Bracket

To render set in inline mathjax, I had to used “\\\\\{“ instead of “\\\{”. Probably some app is causing the inline math backslash to be escaped.

The following line was rendered correctly

{% highlight tex %}
$ \lVert A \rVert_1 = \max_j \sum_i^n \lvert a_{ij} \rvert $
{% endhighlight %}

But when I added curly bracket on summation subscript, it wasn’t rendered.

{% highlight tex %}
$ \lVert A \rVert_1 = \max_j \sum_{i}^n \lvert a_{ij} \rvert $
{% endhighlight %}

Turns out that you have to escape the underscore `_` too...


### Declare Operators

{% highlight html %}
{% raw %}
<div tyle="display:none">
  $
    \DeclareMathOperator*{\argmin}{arg\,min}
    \newcommand{\csch}{\mathop{\rm csch}\nolimits}
  $
</div>
{% endraw %}
{% endhighlight %}
