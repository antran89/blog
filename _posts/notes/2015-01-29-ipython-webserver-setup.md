---
layout: article
title: "Ipython Webserver Setup"
date: 2015-01-29T00:28:52-0800
modified:
categories: notes
comments: true
excerpt:
tags: [ipython, notebook, webserver]
ads: true
image:
  feature:
  teaser:
---

IPython secure webserver setup

## Password

{% highlight bash %}
$ ipython
In [1]: from IPython.lib import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: ‘sha1:67c9e60bb8........’
{% endhighlight %}

## Create cert file

Go to the directory you want to save the cert file

{% highlight bash %}
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
{% endhighlight %}

If it gives you some stupid error that involves `/opt/anaconda1anaconda2anaconda3/ssl/`[^2], update openssl

{% highlight %}
conda update openssl
{% endhighlight %}

## Modify default nbserver configuration

If you don’t have default nbserver profile, create one using

{% highlight bash %}
ipython profile create nbserver
{% endhighlight %}

Then, modify your default profile on `~/.ipython/profile_default/ipython_notebook_config.py`

{% highlight python %}
c = get_config()

# Kernel config
c.IPKernelApp.pylab = ‘inline’  # if you want plotting support always

# Notebook config
c.NotebookApp.certfile = u’/absolute/path/to/your/certificate/mycert.pem’
c.NotebookApp.ip = ‘*’
c.NotebookApp.open_browser = False
c.NotebookApp.password = u’sha1:bcd259ccf...[your hashed password here]’
# It is a good idea to put it on a known, fixed port
c.NotebookApp.port = 9999
{% endhighlight %}

## Run

{% highlight bash %}
ipython notebook
{% endhighlight %}


[^1]: http://ipython.org/ipython-doc/1/interactive/public_server.html
[^2]: https://github.com/ContinuumIO/anaconda-issues/issues/137
