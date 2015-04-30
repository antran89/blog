---
layout: article
title: "Include HTML in HTML with sed"
date: 2015-04-30T01:27:03-0700
modified:
categories: notes
comments: true
excerpt:
tags: [javascript]
ads: true
image:
  feature:
  teaser:
---

Including an HTML file from another HTML file using pure javascript [^1].

- Use `<script src="b.js"></script>`
- In `b.js`

{% highlight javascript %}
document.write('\
\
    <h1>Add your HTML code here</h1>\
\
     <p>Notice however, that you have to escape single quote \'
    </p>\
\
');
{% endhighlight %}


- Adding such backslashes and escaping single quotes might be quite annoying.

{% highlight bash %}
sed 's/^.*$/&\\/g;' b.js > tmpB.js
sed s/\'/"\\\'"/g tmpB.js > escapedB.js
rm tmpB.js
{% endhighlight %}

- For SVG, just use `embed`. However, if you want to modify the svg, use the above javascript so that it can modify the svg.

{% highlight html %}
<embed src="semgem/network.svg"
  width="600" height="1000"
  type="image/svg+xml"
  pluginspage="http://www.adobe.com/svg/viewer/install/" />
{% endhighlight %}


[^1]: http://stackoverflow.com/questions/8988855/include-another-html-file-in-a-html-file#answer-15250208
