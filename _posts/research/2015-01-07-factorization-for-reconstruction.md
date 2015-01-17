---
layout: article
title: "Factorization for Reconstruction"
date: 2015-01-16T18:01:25-0800
modified: 2015-01-16T19:28:21-0800
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
\begin{array}{c}
u_1^T \\
v_1^T \\
u_2^T \\
v_2^T \\
\vdots \\
u_F^T \\
v_F^T
\end{array}
\right]
$$

The $u_f, v_f \in \mathcal{R}^{P}$ are concatenated $x,y$ coordinates of $p$th point in $f$ the frame.

#### Center Points

To prevent singular vectors affected by the centroid of the data points, let’s first center the point by subtracting the mean of the points for each frame from the data.

$$
\tilde{W} = 
\left[
\begin{array}{c}
\tilde{u}_1^T \\
\tilde{v}_1^T \\
\tilde{u}_2^T \\
\tilde{v}_2^T \\
\vdots \\
\tilde{u}_F^T \\
\tilde{v}_F^T
\end{array}
\right]
$$

Where $\tilde{u}\_{if} = u\_{if} - \sum\_{j=1}^Pu\_{jf}$ and $\tilde{v}\_{if} = v\_{if} - \sum\_{j=1}^Pv_{jf}$

#### Factorization


Finally we can factorize the points and the affine camera matrix using Singular Value Decomposition.

$$
\begin{align}
\tilde{W} & = MS\\
& = \left[
\begin{array}{c}
M_1\\
M_2\\
\vdots \\
M_F
\end{array}
\right]
\left[
\begin{array}{cccc}
s_1 & s_2 & \dots & s_P
\end{array}
\right]
\end{align}
$$

The matrix $M_f  = [i_f^T j_f^T]^T \in \mathcal{R}^{2 \times 3}$ where $i_f$ and $j_f$ are vectors for camera reference frame axes and $s_p \in \mathcal{R}^3$ is the world coordinate of the point. Using singular value decomposition, one can factorize the matrix $\tilde{W}$ into $U \Sigma V^T$ where singular values on $\Sigma$ are sorted in decreasing order. For noisy observation, the matrix $\tilde{W}$ would be a full rank matrix. Thus, we use best approximation of the observation by taking the rank-3 approximation of the measurement which is $\tilde{W} \approx \sum_{i=1}^3 \sigma_i u_i  v_i^T$

More formally,

$$
\begin{align}
\tilde{W} & = U\Sigma V^T \\
& = 
\left[ \begin{array}{cc}
U_1 & U_2
\end{array}
\right] 
\left[ \begin{array}{cc}
\Sigma_1 & 0 \\
0 & \Sigma_2 \end{array}\right]
\left[ \begin{array}{c}
V_1^T\\
V_2^T
\end{array}\right]\\

 & = U_1 \Sigma_1  V_1^T + U_2 \Sigma_2 V_2^T\\
 & \approx U_1 \Sigma_1  V_1^T \\
 & = \left(U_1 \Sigma^{\frac{1}{2}} \right) \left( \Sigma^{\frac{1}{2}} V_1^T \right)\\
 & = M S
\end{align}
$$

The factorization is not unique and for arbitrary invertible matrix $Q$, $MQ$ and $Q^{-1}S$ also satisfy the observations.

Thus, we further constrain the problems so that axes of a frame $f$ are orthonormal vectors.

$$
\begin{align}
i_f^T Q Q^T i_f & = 1\\
j_f^T Q Q^T j_f & = 1\\
i_f^T Q Q^T j_f & = 0
\end{align}
$$

The $Q$ can be found by fitting data and the solution is unique up to a rotation.

### Robust Factorization using Marques-Costeira Algorithm

Degenerate case

Missing Data


### Reference

[1] C. Tomasi, T. Kanade, <em>Shape and Motion from Image Streams under Orthography: a Factorization Method</em>, IJCV 1992
