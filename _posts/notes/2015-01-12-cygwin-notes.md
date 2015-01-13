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

On windows, you can still use nice `bash` and almost all Linux files.

Install `cygwin` and make sure to install `wget` when the selecting packages to install.

and follow the instructions on

http://superuser.com/questions/41093/installing-cygwin-packages-from-the-command-line

which is

~~~
wget http://apt-cyg.googlecode.com/svn/trunk/apt-cyg
chmod +x apt-cyg
mv apt-cyg /usr/local/bin/
apt-cyg install "package-to-install"
~~~

To simulate linux environment make sure to install the followings

~~~
apt-cyg install bash-completion
apt-cyg install curl
apt-cyg install git
apt-cyg install python
apt-cyg install ncurses
~~~

The `ncurses` include `tput` which allows nicer text-based interfaces.

