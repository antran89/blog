---
layout: article
title: "Include an HTML file in another HTML using pure javascript"
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

If you want to embed an HTML file from another HTML file using pure
javascript[^1],

- Put `<script src="b.js"></script>` on the line that you want to import
- On `b.js`, put the following javascript command

{% highlight javascript %}
document.write('\
\
    <h1>Add your HTML code here</h1>\
\
     <p>Notice however, that you have to escape single quote \' </p>\
\
');
{% endhighlight %}

This will embed the above HTML codes into the original file. The above HTML
script has to be escaped (put a backslash in front of a next line and a single
quote).

- Adding such backslashes and escaping single quotes manually might be quite annoying.
- Please use the stream editor, `sed`, to add backslashes and escape single quotes.

{% highlight bash %}
sed 's/^.*$/&\\/g;' b.js > tmpB.js
sed s/\'/"\\\'"/g tmpB.js > escapedB.js
rm tmpB.js
{% endhighlight %}

- For SVG, just use `embed`. However, if you want to modify the svg, use the
above javascript so that it can modify the svg.

{% highlight html %}
<embed src="semgem/network.svg"
  width="600" height="1000"
  type="image/svg+xml"
  pluginspage="http://www.adobe.com/svg/viewer/install/" />
{% endhighlight %}


[^1]: http://stackoverflow.com/questions/8988855/include-another-html-file-in-a-html-file#answer-15250208
