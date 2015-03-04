---
layout: article
title: "Markdown and Jekyll Cheatsheet"
date: 2015-02-19T00:29:18-0800
modified: 2015-03-03T23:46:56-0800
categories: notes
comments: true
excerpt:
tags: [markdown, jekyll]
ads: true
image:
  feature:
  teaser:
---

## Markdown

- Supported Syntax Highlights

[http://pygments.org/languages/]

- Block Quote

{% raw %}
> quotes
> next line quotes
{% endraw %}

## Jekyll

- double curly braces vs percent sign

    Output markup is delimited by double curly-braces, while tag markup is delimited by a the curly brace / percent sign duo.[^3]

- Refering to a post in Jekyll[^1]

{% highlight html %}
{% raw %}
{% post_url /subdir/2010-07-21-name-of-post %}
{% endraw %}
{% endhighlight %}

[^1]: <http://jekyllrb.com/docs/templates/>
[^2]: <http://sourceforge.net/p/jekyllc/bugs/markdown_syntax>
[^3]: <http://code.tutsplus.com/articles/building-static-sites-with-jekyll--net-22211>
