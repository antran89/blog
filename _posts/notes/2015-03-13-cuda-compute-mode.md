---
layout: article
title: "CUDA compute mode"
date: 2015-03-14T01:46:37-0700
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

Nvidia CUDA compute mode

For each thread, once you set the device `cudaSetDevice`, you do not need to call the `setDevice` afterward.[^1]  However, for different compute modes, you might see different behaviros.

All available compute modes are[^2]

- DEFAULT
- EXCLUSIVE_THREAD
- PROHIBITED
- EXCLUSIVE_PROCESS


[^1]: <http://stackoverflow.com/questions/13900078/how-to-prevent-two-cuda-programs-from-interfering>
[^2]: <http://stackoverflow.com/questions/2658899/cuda-device-selection-with-multiple-cpu-threads>
