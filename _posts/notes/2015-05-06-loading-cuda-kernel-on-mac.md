---
layout: article
title: "Loading CUDA Kernel on MacOS"
date: 2015-05-06T22:45:21-0700
modified:
categories: notes
comments: true
excerpt:
tags: [cuda]
ads: true
image:
  feature:
  teaser:
---

Even if you have CUDA capable device, on OSX, you might the experience `no CUDA-Capable device` error.

Verify that you are running cuda kernel [^1].

`kextstat | grep -i cuda`

Run CUDA kernel [^2]

[^1]: http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-mac-os-x/#axzz3ZQR5QyGY
[^2]: http://forum.blackmagicdesign.com/viewtopic.php?f=18&t=8990

