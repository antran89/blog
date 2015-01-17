---
layout: article
title: "Cygwin Notes"
date: 2015-01-12T05:27:32-08:00
modified:
categories: notes
comments: true
excerpt:
tags: [cygwin]
ads: true
image:
  feature:
  teaser:
---

Recently, our group purchased a Windows machine for graphics research. While I was searching for a good way to communicate with Linux machines, I found out that I can still use nice `bash` and almost all Linux files using `cygwin`

Also there is a command similar to `apt-get` in cygwin, which is `apt-cyg`. To install the `apt-cyg` follow the instructions.


First, Install `cygwin` and make sure to install `wget` when you select packages to install.

and follow the instructions on

<http://superuser.com/questions/41093/installing-cygwin-packages-from-the-command-line>

which is

{% highlight bash %}
wget http://apt-cyg.googlecode.com/svn/trunk/apt-cyg
chmod +x apt-cyg
mv apt-cyg /usr/local/bin/
apt-cyg install "package-to-install"
{% endhighlight %}

To simulate Linux environment, install the following packages

{% highlight bash %}
apt-cyg install bash-completion
apt-cyg install curl
apt-cyg install git
apt-cyg install python
apt-cyg install ncurses
{% endhighlight %}

The `ncurses` include `tput` which allows nicer text-based interfaces.


To open up a windows file explorer, type 

{% highlight bash %}
cygstart “dir”
{% endhighlight %}

where "dir" is the directory you want to open.

Check out some expert configuration on

<http://guysherman.com/2013/11/02/my-ultimate-cygwin-setup/>

I'm using `tmux` + `vim` on the `cygwin` terminal but it's a bit slow.
