---
layout: article
title: "Debugging CUDA + Mex in MATLAB"
date: 2015-03-03T10:58:37-0800
modified: 2015-10-14T01:21:48-0700
categories: notes
comments: true
excerpt:
tags: [cuda, matlab]
ads: true
image:
  feature:
  teaser:
---

Compiling a CUDA code in MATLAB and debugging it can be little tricky. Luckily, I succeed after few attemps and I’ll summarize what I did for the future reference.

First let’s compile your CUDA + Mex file. I made a simple MATLAB code that compiles CUDA files.

{% gist d293a10b99940e9db729 %}

The compilation source code is a part of my [Matlab Cuda Convolution](https://github.com/chrischoy/MatlabCUDAConv/) and you can see the compilation code in action at <https://github.com/chrischoy/MatlabCUDAConv>.

To debug, you can either use the last argument to be `true`. It will add the debug option `-g -G`.

Once you compiled using the debug flag, you have to run the MATLAB with the debug flag (`-Dgdb` for Linux, `-Dlldb` for Mac)

- Mac

    {% highlight bash %}
    #!/bin/bash
    export NSIGHT_CUDA_DEBUGGER=1
    /Applications/MATLAB_R2014a.app/bin/matlab -Dlldb 
    {% endhighlight %}

- Linux

    {% highlight bash %}
    #!/bin/bash
    export NSIGHT_CUDA_DEBUGGER=1
    matlab -Dgdb
    {% endhighlight %}

You will see a friendly termianl style debugging session. Run your code on matlab and the debugger will automatically stop when it goes into a mex function. If you are unfamiliar with `gdb` debugging, read through a post <http://www.cs.cmu.edu/~gilpin/tutorial/> that I found very useful.
