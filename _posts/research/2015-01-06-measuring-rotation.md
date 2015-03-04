---
layout: article
title: "Measuring A Rotation"
date: 2015-01-06T23:17:02-08:00
modified: 2015-03-03T23:44:16-0800
categories: research
excerpt: "How do we measure a distance between rotations"
comments: true
mathjax: true
tags: [rotation, quaternion]
ads: true
image:
  feature:
  teaser:
---

Measuring the distance between two viewpoints can be ambiguous. If we want to azimuth and elevation only, we can uniquely identify a viewpoint as a point on a sphere. Measuring distance between two viewpoint in this case can be geometrically interpreted as a geodesic distance between two points on the unit sphere. The geodesic distance is the radian angle between two view points. This measure assumes the up-direction and cannot represent in-plane rotation.

Alternatively, one can use the Euler angles : yaw, pitch and roll, or in other terms $\phi, \theta, \psi$. These three angles represent full 3D rotation but measuring the distance between two rotations is not defined.

One of the most classic ways to measure distance is the measure an angle between two quaternions. For $q_i, q_j$ , unit quaternions,

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

Finally, using a logarithm of a matrix to compute a rotation. For instance let $R_i, R_j \in \mathcal{R}^{3 \times 3}$ be two rotation matrices that we want to measure a similarity or a distance between them. Since these are rotations, natural thing to do is to measure an angle (3D) between these rotations.

$$
d_g(R_i, R_j) = \theta
$$

Let’s analyze some properties of a rotation matrix. First, a rotation matrix is orthogonal $R^{-1} = R^T$. Second, an exponential of a skew-symmetric matrix is always a rotation matrix!. The Rodrigues’ rotation formula uses the fact and Lie algebra group $so(3)$ to represent a rotation using the exponential of a matrix.

$$
R = \exp \left( \theta (\mathbf{u} \cdot \mathbf{L} )  \right)
$$

Where $u$ is the rotation axis and $\theta$ is the angle of rotation. The $L$ be the Lie group basis in $so(3)$ (three Hermitian matrices).

Thus rotation from $R_i$ to $R_j$ is $R_i^T R_j$. The angle between two rotations is

$$
\begin{align*}
\| \log( R_i^T R_j) \|_F & = \| \theta \mathbf{u} \cdot \mathbf{L} \|_F\\
& = \theta
\end{align*}
$$

Where we used the fact that a Frobenius norm of a matrix is equal to an L2 norm of singular values [(Interesting properties of matrix norms)]({% post_url /research/2015-01-13-matrix-norms %}).

Thus using a logarithm of a matrix, we can extrac the angle of rotation. [^1]

[^1]: http://en.wikipedia.org/wiki/Axis–angle_representation
