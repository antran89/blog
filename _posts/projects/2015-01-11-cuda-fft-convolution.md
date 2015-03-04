---
layout: article
title: "CUDA FFT Convolution"
date: 2015-01-05T17:34:17-08:00
modified: 2015-03-04T01:14:30-0800
categories: projects
comments: true
excerpt:
tags: [FFT, CUDA, Convolution]
ads: true
mathjax: true
image:
  feature:
  teaser: fftconv.png
---

Cooley-Tukey FFT algorithm computes Discrete Fourier Transform (DFT) in $O(n \log n)$ time. This speed up on the Fourier Transformation significantly changed how we filter signals.

The convolution theorem states that

$$
\mathcal{F}(f \ast g ) = \mathcal{F}(f) \cdot \mathcal{F}(g)
$$

Where $\cdot$ denotes point-wise (element-wise) multiplication.

Thus, instead of convolving the function $g$ in the time domain, we can compute the convolution in the frequency domain. By doing so, we can speed up the convolution.

i.e. for a length $n$ signal $f$ and a length $m$ signal $g$, the convolution in the time domain would take $O(nm)$ time whereas in the frequency domain it only takes linear time $O(n + m)$. If we consider the cost for transforming the signal into the frequency domain and back to the time domain, the total cost is $O((n+m) \log (n+m))$.

Thus, for a larger filter $g$, the convolution using FFT would asymptotically runs faster.


## Cuda Convolution

For my CVPR 15 paper, [**Enrich Object Detection with 2D-3D Registration and Continuous Viewpoint Estimation**]({{ post_url /projects/2015-03-02-enrich-object-detection }}), I made a CUDA FFT convolution package to speed up the convolution.

You can download the source code from <https://github.com/chrischoy/MatlabCUDAConv>
