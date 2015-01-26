---
layout: article
title: "Multiple View Geometry Summary"
date: 2015-01-25T16:23:55-0800
modified:
categories: lectures
excerpt:
comments: true
mathjax: true
tags: [multiple view geometry, homography, fundamental matrix, computer vision]
ads: true
toc: true
image:
  feature:
  teaser:
---


I’m TAing CS231A, Computer Vision 2015 winter quarter. Studying lecture materials as a student is very different from as working as a teaching assistance. You have to be fully prepared for the TA sessions and there are subtleties which students ask often so  I want to summarize important things on this notes to remind myself or help readers understand concepts better.

I’ll follow the notations on *Multiple View Geometry in Computer Vision by Hartley and Zisserman*.

## Homogeneous representation of a line

A line in $\mathcal{R}^2$ can be represented as $ax+by+c = [x, y, 1] l = 0$ where $l = [a, b, 1]^T$. By adding additional element, we can represent lines and points in a consistent way which ease some computation. We call this *homogeneous* representation or coordinate.

The point, in this *homogeneous* coordinate is $\mathbf{x} = [x,y,1]^T$. Then a line crossing $x_1$ and $x_2$ can be easily computed using $x_1 \times x_2$ since the cross product of vectors produces a vector perpendicular to the others. By the same logic, the intersection of two lines $l_1$ and $l_2$ is $l_1 \times l_2$. 

## Projective Transformations

Projectivity, projective transformation or homograpghy, is an invertible mapping $h$ from $\mathcal{P}^2 \rightarrow \mathcal{P}^2$ such that points on a line maps to also three points on a line. A point after a projective transformation $H$ is $x’ = Hx$ and a line after the same transformation is $H^{-T}l$. Proof. $l^Tx = 0$ satisfies a line equation and $l^T H^{-1} Hx = 0$. Since $x’ = Hx$ we have $(H^{-T}l)^Tx’ = 0$. Thus, $l’ = H^{-T}l$.

## Inhomogeneous coordinate

To distinguish a vector in homogeneous coordinate and a vector transformed back to regular coordinate, we use $\tilde{\cdot}$ to denote inhomogeneous, or a vector transformed back to a $\mathcal{R}^n$ and a letter to denote a vector in homogeneous coordinate in $\mathcal{R}^{n+1}$.

$$
X =
\left[
\begin{array}{c}
\tilde{X}\\
1
\end{array}
\right]
$$

Also we use lower case letters to denote vectors or points in 2D and capital letters to denote points, vectors in 3D.

## Projective Geometry in 3D

Suppose a point in 3D $X$, and a plane that passes through the point as $\pi$ then $\pi^T X = 0$. The point under a projective transformation $H$ is $X’ = HX$ and the plane $\pi’ = H^{-T}\pi$ using the same technique for 2D lines after homography.


## Direct Linear Transformations

To find the transformation $H$, whose degree of freedom is 8, one must find at least 4 point correspondences (each gives x-coordinate and y-coordinate correspondences). Suppose a point $x$ after transformation $H$ corresponds to $x’$. Since the points are in homogeneous coordinate, the scale cannot be determined but the direction of the points are the same. Note that $\mathbf{a} \times \mathbf{b} = 0$ if $\mathbf{b} = s\mathbf{a}$ where $\mathbf{a},\mathbf{b} \in \mathcal{R}^3$ and $c$ is a scaler. Thus, corresponding points $\mathbf{x}$ and $\mathbf{x}’$ must satisfy $\mathbf{x}’\times H\mathbf{x} = 0$. If we denote $j$ the row of the $H$ as $h_j^T$ then the equation can be written as the following.
$$
\begin{align*}
\mathbf{x}’\times H\mathbf{x} & = 
\left(
\begin{array}{c}
y’ h_3^T \mathbf{x} - w’ h_2^T \mathbf{x}\\
w’ h_1^T \mathbf{x} - x’ h_3^T \mathbf{x}\\
x’ h_2^T \mathbf{x} - y’ h_1^T \mathbf{x}
\end{array}
\right)\\
& = 
\left[
\begin{array}{ccc}
\mathbf{0}^T & -w’ \mathbf{x}^T & y’\mathbf{x}^T \\
w’ \mathbf{x}^T & \mathbf{0}^T  & -x’\mathbf{x}^T \\
-y’ \mathbf{x}^T & x’\mathbf{x}^T & \mathbf{0}^T
\end{array}
\right]
\left(
\begin{array}{c}
h_1\\h_2\\h_3
\end{array}
\right)
\end{align*}
$$

Since the homography $H$ is known up to scale, we can set $\|h\|=1$. Then the solution will be the right singular vector corresponding to the smallest singular value. Lastly, we can arbitrarily set $h_{ij} = 1$ and solve for the 8 unknowns. However if infact $h_{ij}$ is close to zero, the solution can be unstable and is not recommended in general.


