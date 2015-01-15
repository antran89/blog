---
layout: article
title: "Factorization for Reconstruction"
date: 2015-01-07T13:26-08:00
modified:
categories: research
excerpt: "How do we measure the distance in rotation space"
comments: true
mathjax: true
tags: [reconstruction, factorization, SfM]
ads: true
image:
  feature:
  teaser:
---

Extracting shape and motion from a video can be simply solved using various techniques. The most famous method is Tomasi-Kanade algorithm where you factorize a coordinate of feature points in a sequence of frame into shape and motion matrices. The idea is strikingly simple yet elegant.


### Tomasi-Kanade Algorithm

Suppose you have a $F$ frames of track of $P$ points. Each point being a $u,v$ pair indicating $x$ coordinate and $y$ coordinate respectively and $f$ be the frame index and $p$ be the point index $(u_f^p, v_f^p)$. Then we model the camera as an orthographic projection camera.

$$
M_f = 
\left[
\begin{array}{c}
i_f\\
j_f
\end{array}
\right]
$$

where $i,j \in \mathcal{R}$ being the unit vector pointing along $x$ and $y$ axes respectively. Then the camera projection ray is $k_f = i_f \times j_f$.


#### Stacking $U,V$.

First, we define a large matrix that concatenate $u$ and $v$ coordinate

$$
W =
\left[
\being{array}{c}
U \\
V
\end{array}
\right]
$$

The $U, V \in \mathcal{R}^{F \cross P}$ are again defined as a concatenated $u$ coordinates for all points $P$ and frames $F$.

#### Why center points

To prevent singular vectors affected by the centroid of the data points, they first center the point by computing  to  the centroid of points.


### Robust Factorization using Marques-Costeira Algorithm

Degenerate case

Missing Data


