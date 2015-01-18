---
layout: article
title: "Add open qtconsole button on ipython menu"
date: 2015-01-17T16:15:37-0800
modified:
categories: notes
comments: true
excerpt:
tags: [ipython, notebook, qtconsoel]
ads: true
image:
  feature:
  teaser:
---

Found very cool ipython notebook customization that is very handy. Every time I make small change, I have to click run button using mouse which is very annoying. So I've been using `%qtconsole` command to create qtconsole connected to the notebook. But even typing was not convenient since I also have to add that line and run. The customization creates a button on the ipython menubar and opens qtconsole.[^1]

{% highlight bash %}
ipython profile create default
{% endhighlight %}

and add the following lines to  `~/.ipython/profile_default/static/custom/custom.js`

{% highlight javascript %}
$([IPython.events]).on('notebook_loaded.Notebook', function(){
    IPython.toolbar.add_buttons_group([
                {   
                'label'   : 'run qtconsole',
                'icon'    : 'icon-terimnal',
                'callback': function(){IPython.notebook.kernel.execute('%qtconsole')}
                } 
                // add more button here if needed.
            ]); 
});
{% endhighlight %}


[^1]: https://github.com/ipython/ipython/issues/2593/
