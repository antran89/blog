---
layout: article
title: "Measuring a Rotation"
date: 2015-01-06T23:17:02-08:00
modified: 2015-03-05T14:45:39-0800
categories: research
excerpt:
comments: true
mathjax: true
tags: [rotation, quaternion]
ads: true
image:
  feature:
  teaser:
---

Measuring the distance between two viewpoints can be ambiguous. If we want to measure azimuth and elevation only, we can uniquely identify a viewpoint as a point on a unit sphere. Measuring distance between two viewpoints in this case can be geometrically interpreted as a geodesic distance between two points on the unit sphere. The geodesic distance is the radian angle between two viewpoints. This measure assumes the up-direction and cannot represent in-plane rotation.

Alternatively, one can use the Euler angles: yaw, pitch, and roll, or in other words, $\phi, \theta, \psi$. These three angles represent full 3D rotation, but measuring the distance between two rotations is not defined.

One of the most classic ways to measure distance is to measure an angle between two quaternions $q_i, q_j$, unit quaternions,

$$
\theta = \cos^{-1}\left( 2 \langle q_i, q_j \rangle^2 - 1 \right)
$$

or for $q = a + b\mathbf{i} + c \mathbf{j} + d \mathbf{k}$,

$$
\begin{align*}
q_d    & = q_i^{-1} q_j\\
q_d    & = s + \mathbf{x} \mbox{ where } s \mbox{ is the scalar part}\\
\theta & = 2 \tan^{-1}\left( \frac{s}{\| x \|} \right)
\end{align*}
$$

{::comment}
There are some interesting properties of quaternions. First, let $q = a + b \mathbf{i} + c \mathbf{j} + d \mathbf{k}$ then $\mathbf{i} \mathbf{j} = \mathbf{k}, \mathbf{i}^2 = -1, \mathbf{ijk} = -1$. Easy way to compute the product is to represent a quaternion as a matrix and do matrix-vector multiplication.

To rotate a quaternion $p$ by $q$, conjugate $p$ by $q$:

$$
p’ = qpq^{-1}
$$

The quaternion has long been used in gaming and graphics for nice spherical linear interpolation (SLERP).

{:/comment}

Finally, we can use a logarithm of a matrix to compute a rotation. For instance let $R_i, R_j \in \mathcal{R}^{3 \times 3}$ be two rotation matrices between which we want to measure a distance. Since these are rotations, the natural thing to do is to measure an angle (3D) between these rotations.

$$
d_g(R_i, R_j) = \theta
$$

Let’s analyze some properties of a rotation matrix. First, a rotation matrix is orthogonal $R^{-1} = R^T$. Second, an exponential of a skew-symmetric matrix is always a rotation matrix! Rodrigues’s rotation formula uses this fact and the Lie algebra group $so(3)$ to represent a rotation using the exponential of a matrix.

$$
R = \exp \left( \theta (\mathbf{u} \cdot \mathbf{L} )  \right)
$$

where $u$ is the rotation axis and $\theta$ is the angle of rotation. The $L$ is the Lie group basis in $so(3)$ (three Hermitian matrices).

Thus rotation from $R_i$ to $R_j$ is $R_i^T R_j$. The angle between two rotations is

$$
\begin{align*}
\| \log( R_i^T R_j) \|_F & = \| \theta \mathbf{u} \cdot \mathbf{L} \|_F\\
& = \theta
\end{align*}
$$

where we used the fact that a Frobenius norm of a matrix is equal to an L2 norm of singular values[^2]. Thus using a logarithm of a matrix, we can extract the angle of rotation. [^1]

[^1]: http://en.wikipedia.org/wiki/Axis–angle_representation
[^2]: {% post_url /research/2015-01-13-matrix-norms %}
