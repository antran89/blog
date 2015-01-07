---
layout: article
title: "Measuring Rotation"
date: 2015-01-06T23:17:02-08:00
modified:
categories: research
excerpt: "How do we measure the distance in rotation space"
comments: true
mathjax: true
tags: [rotation, quaternion]
ads: true
image:
  feature:
  teaser:
---

Measuring the distance between two viewpoint can be tricky. Let’s say we create a view shpere. We can uniquely identify a point on the surface using longitude and altitude as we do on the Earth. We also call them as azimuth and elevation. Measuring distance between two viewpoint in this case would be the angle between two points on the view sphere each point being the viewpoint. Or we can just directly use the geodesic distance between them.

However if we use these distances, we cannot capture in-plane rotation.

Another way to represent a rotation is Euler angle. It uses yaw, pitch and roll, or in other terms $\phi, \theta, \psi$, to represent full 3D rotation but it does not give a good way to measure distance between two rotations.

The most classic way to measure distance is angle between two quaternion distance. For $q_i, q_j \in \mathcal{R}^4$ be the unit quaternions. Then innerproduct of the quaternions $\langle q_i, q_j \rangle$ will give a measure of distance thus

$$
\theta = \cos^{-1}\left( 2 \langle q_i, q_j \rangle^2 - 1 \right)
$$

or

$$
\begin{align*}
q_d    & = q_i^{-1} q_j\\
\theta & = 2 \tan^{-1}\left( \frac{[q_d]_4}{\| q_d \|} \right)
\end{align*}
$$

There are some interesting properties of quaternions. First, let $q = a + b \mathbf{i} + c \mathbf{j} + d \mathbf{k}$ then $\mathbf{i} \mathbf{j} = \mathbf{k}, \mathbf{i}^2 = -1, \mathbf{ijk} = -1$. Easy way to compute the product is to represent a quaternion as a matrix and do matrix-vector multiplication.

To rotate a quaternion $p$ by $q$, conjugate $p$ by $q$:

$$
p’ = qpq^{-1}
$$

The quaternion has long been used in gaming and graphics for nice spherical linear interpolation (SLERP).


Finally, using matrix logarithm to compute rotation. For instance let $R_i, R_j \in SO(3)$. Then

$$
d_g(R_i, R_j) = \| \log( R_i^T R_j) \|_F
$$

is the geodesic distance on 3D manifold of rotation matrix.
