---
layout: article
title: "Iterative Conjugate Gradient method in CUDA"
date: 2015-01-11T22:39:12-08:00
modified:
categories: projects
comments: true
excerpt:
tags: [cuda, conjugate gradient method]
ads: true
mathjax: true
image:
  feature:
  teaser:
---

For a large sparse or low conditioned positive definite matrix $A \in \mathcal{R}^{n\times n}$, solving $Ax = b$ for $x$ using Cholesky decomposition can be very slow and numerically unstable (comparatively). Iterative Conjugate Gradient method takes gradient steps that minimizes the residual by taking steps conjugate to previous gradient steps. It would take at most $n$ gradient steps if there is no numerical round off error.

## Source Code

TODO
