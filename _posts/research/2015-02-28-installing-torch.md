---
layout: article
title: "Installing Torch"
date: 2015-02-28T22:18:56-0800
modified:
categories: research
excerpt:
comments: true
tags: [torch]
toc: true
ads: true
image:
  feature:
  teaser:
---

While installing torch 7 on my Linux machine, I encountered several errors which was not documented on the torch wiki page.

Especially, make sure that all the steps in https://raw.githubusercontent.com/soumith/fblualib/master/install_all.sh are installed correctly. In my case, `folly`, `fbthrift` failed.

## Folly

1. double conversion

    If your luarocks fails to find FOLLY_DIR or LIBRARY path, it is most likely that folly installation has failed. First
    download double conversion library from https://github.com/floitsch/double-conversion/

    {% highlight bash %}
    git clone https://github.com/floitsch/double-conversion/
    cd double-conversion
    cmake .
    make
    sudo make install
    {% endhighlight %}

2. libevent

    {% highlight bash %}
    sudo apt-get install libevent-dev 
    {% endhighlight %}

3. undefined reference to symbol ‘pthread_rwlock_trywrlock@@GLIBC_2.2.5’[^1]

    In the `Makefile`, add `-pthread` to `AM_LDFLAGS`

4. relocation R_X86_64_32 against

    Go to `gflag` installation path and 
    
    {% highlight bash %}
    cmake . -DCMAKE_CXX_FLAGS= -fPIC
    make
    sudo make install
    {% endhighlight %}

## FBThrift

1. dependencies

    {% highlight bash %}
    sudo apt-get install libnuma-dev libsasl2-dev
    {% endhighlight %}

## FBLuaLib

1. Dependency

    {% highlight bash %}
    sudo apt-get install libmatio-dev
    {% endhighlight %}

[^1]: http://stackoverflow.com/questions/16257564/linker-error-undefined-reference-to-symbol-pthread-rwlock-trywrlockglibc-2-2
