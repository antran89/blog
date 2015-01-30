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


I’m TAing CS231A, Computer Vision in winter quarter, 2015. Studying lecture materials as a student is very different from working as a teaching assistant You have to be fully prepared for the TA sessions, and there are subtleties which students ask often, so I want to summarize important things in these notes to remind myself or help readers understand concepts better.

I’ll follow the notations on *Multiple View Geometry in Computer Vision* by Hartley and Zisserman.

## Homogeneous representation of a line

A line in $\mathcal{R}^2$ can be represented as $ax+by+c = [x, y, 1] l = 0$ where $l = [a, b, 1]^T$. By adding an additional element, we can represent lines and points in a consistent way, which makes some computation easier. We call this a *homogeneous* representation or coordinate.

The point, in this *homogeneous* coordinate is $\mathbf{x} = [x,y,1]^T$. Then a line crossing $x_1$ and $x_2$ can be easily computed using $x_1 \times x_2$ since the cross product of vectors produces a vector perpendicular to the others. By the same logic, the intersection of two lines $l_1$ and $l_2$ is $l_1 \times l_2$. 


## Projective Transformations

Projectivity - projective transformation or homography - is an invertible mapping $h$ from $\mathcal{P}^2 \rightarrow \mathcal{P}^2$ such that points on a line map to also three other points on another line. A point after a projective transformation $H$ is $x’ = Hx$, and a line after the same transformation is $H^{-T}l$. Proof. $l^Tx = 0$ satisfies a line equation and $l^T H^{-1} Hx = 0$. Since $x’ = Hx$ we have $(H^{-T}l)^Tx’ = 0$. Thus, $l’ = H^{-T}l$.

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

The mapping from homogeneous to inhomogeneous is defined as

$$
\left(
\begin{array}{c}
X\\Y\\Z
\end{array}
\right)
\mapsto
\left(
\begin{array}{c}
X/Z\\Y/Z
\end{array}
\right)
$$

Similarly, for X in 3-space, homogeneous vector $\mathbf{X} = (X\_1, X\_2, X\_3, X\_4)$ with $X\_4\neq 0$ is mapped to $(X\_1/X\_4, X\_2/X\_4, X\_1/X\_4, X\_3/X\_4)$ in inhomogeneous coordinate.

Also we use lower case letters to denote vectors or points in 2D and capital letters to denote points, vectors in 3D.

Using this notation, we can represent a point at infinity as a point whose 4th element is 0 in 3D, $[X, Y, Z, 0]$, or 3rd element is 0 in 2D, $[x,y,0]$. We can define a plane that passes through all the points at infinity in 3D as $ \pi\_{\infty} = [0,0,0,1]^T$ or a line in 2D as $l\_\infty = [0,0,1]^T$ in 2D.

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

Since the homography $H$ is known up to scale, we can set $\|h\|=1$. Then the solution will be the right singular vector corresponding to the smallest singular value. Lastly, we can arbitrarily set $h\_{ij} = 1$ and solve for the 8 unknowns. However if infact $h\_{ij}$ is close to zero, the solution can be unstable and is not recommended in general.

## Camera Model

We use the basic pinhole camera model, which is $ (X,Y,Z)^T \mapsto (fX/Z, fY/Z)^T $. If we consider the principal point offset, difference between the origin of coordinates in the image plane an principal point, $(p_x, p_y)$, this can be compactly represented as in homogeneous coordinate.

$$
\left(
\begin{array}{c}
fX + Zp_x\\
fY + Zp_y\\
Z
\end{array}
\right) = \left[
\begin{array}{cccc}
f & & p_x & 0 \\
  & f & p_y & 0 \\
& & 1 & 0
\end{array}
\right]
\left(
\begin{array}{c}
X\\Y\\Z\\1
\end{array}
\right)
= K [I | \mathbf{0}] X_{cam}
$$

If the camera undergoes translation $\tilde{C}$ and rotation $R$, $\tilde{X}\_{cam} = R (\tilde{X} - \tilde{C})$. Putting this together, $\mathbf{x} = KR[I \| -\tilde{C}]X = K[R \| t]X $.

To find out the matrix $P = K[R \| t]$, we need at least 6 points correspondences between $X_i \leftrightarrow x_i$. Let $P_i^T$ be the $i$ th row of the matrix $P$.

$$
\left[
\begin{array}{ccc}
\mathbf{0}^T & -w’ \mathbf{x}^T & y’\mathbf{x}^T \\
w’ \mathbf{x}^T & \mathbf{0}^T  & -x’\mathbf{x}^T \\
-y’ \mathbf{x}^T & x’\mathbf{x}^T & \mathbf{0}^T
\end{array}
\right]
\left(
\begin{array}{c}
P_1\\P_2\\P_3
\end{array}
\right)
= \mathbf{0}
$$

as we did for finding homography $H$, or 

$$
\left[
\begin{array}{ccc}
\mathbf{0}^T & -w’ \mathbf{x}^T & y’\mathbf{x}^T \\
w’ \mathbf{x}^T & \mathbf{0}^T  & -x’\mathbf{x}^T 
\end{array}
\right]
\left(
\begin{array}{c}
P_1\\P_2\\P_3
\end{array}
\right)
= \mathbf{0}
$$

Since the rows of the left matrix is linear dependent.

## Homography of camera movements

First, consider two cameras with the same camera center.

$$
P = KR[I | -\tilde{C}], \; P’ = K’R’[I|-\tilde{C}]
$$

Thus the homography $H$ can be easily found

$$
x’ = P’X = (K’R’)(KR)^{-1}PX = (K’R’)(KR)^{-1}x = Hx
$$

## Camera Calibration

TODO : arbitrary camera rotation and translation

A ray that passes camera center will project to a point on an image plane. Let $\mathbf{d}$ be the direction vector and $\lambda \mathbf{d}$. If we assume that the camera is aligned with the world reference frame, $x = K[I \| 0](\lambda \mathbf{x}^T, 1)^T = K\mathbf{x}$. Thus we can find the direction of the line $d = K^{-1}x$.

Thus the angle between two rays $\mathbf{d}\_1 and \mathbf{d}\_2$ ban be easily computed as

$$
\cos \theta = \frac{d_1^T d_2}{\|d_1\|_2  \|d_2\|_2}
$$

## Conics and the absolute Conic

A conic is curves described by a second-degree equation in the plance. Conics are of three major types: hyperbola, ellipse, and parabola. We will show that the conics are equivalent under similarity transformations. A generic conic is defined as

$$
ax^2 + bxy + cy^2 + dx + ey + f = 0
$$

In matrix form

$$
\mathbf{x}^T C \mathbf{x} = 0
$$

where the conic matrix $C$ is given by

$$
C = \left[
\begin{array}{ccc}
a & b/2 & d/2\\
b/2 & c & e/2\\
d/2 & e/2 & f
\end{array}
\right]
$$

A line that tangent to $C$ at a point $x$ is given by $l = Cx$. Proof: since $x^T l = x^T Cx = 0$ the line passes the point $x$ on the conic. If it crosses one point, we know that the line is tangent to the conic. Suppose there is another point $y$ that crosses the conic. Then $l’ = Cy$ must cross $x$ and $y$ thus $x^TCy = y^TCy = 0$. Thus, (\alpha x + \beta y)^T C (\alpha x + \beta y) = 0$ and the whole line crosses $x$ and $y$ must lies on the conic $C$.

We defined a line tangent to the conic $C$, $l = Cx$. The line must passes the point $x$ and satisfies $l^T x = 0$. Since $x = C^{-1}l$, $l^T C^{-1} l = 0$. The inverse of conic $C$ is denoted as the dual conic, $C^* = C^{-1}$ up to scale. 
The absolute conic $\Omega\_\infty$ d, is a conic on a plane at infinity, $\pi\_\infty$. Points on the conic satisfy

$$
\begin{align}
X_1^2 + X_2^2 + X_3^2 & = 0\\
X_4 & = 0
\end{align}
$$



