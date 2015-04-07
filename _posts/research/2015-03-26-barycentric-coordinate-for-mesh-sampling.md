---
layout: article
title: "Barycentric Coordinate for Surface Sampling"
date: 2015-03-26T17:36:52-0700
modified:
categories: research
excerpt:
comments: true
mathjax: true
tags:
toc: true
ads: true
image:
  feature:
  teaser:
---

To convert a mesh into a point cloud, one has to sample points that can
uniformly cover the surface. To do so, one must choose the number of samples
proportional to the area of a face (polygon).

First, we iterate through
all polygons, splitting them into triangles since measuring the area of a triangle
is much easier than computing the area of a polygon. Then, for each triangle,
we compute its area and store it in an array. Next, we select a triangle with
probability proportional to its area. 

Sampling uniformly on a triangle requires a trick: Barycentric coordinate
system.

## Barycentric Coordinate System

The Barycentric coordinate is a way to define a point on a $n$
dimensional simplex using $n$ coordinates. You define $n$ weights $[a_1 ,...,
a_n]$ to denote a point $p= a_1 \mathbf{x}_1 + \cdots + a_n \mathbf{x}_n$.
It is just a convex hull of the simplex.

Then, for each selected triangle (let’s denote the vertices of the triangle as
$A, B, C$ ), we sample a point on its surface by generating two random numbers,
$r_1$ and $r_2$ between 0 and 1, and compute the coordinate using the equation
on the next section.

## Computing the Area of Faces

First, you have to convert all polygons into triangles. You can easily do this
in many commercial mesh converters.

Let the vertices of a triangle $A, B, C$ in 3D coorindate as $\mathbf{a},
\mathbf{b}$ and $\mathbf{c}$. Then, the area of the triangle formed by the
three vectors is

$$
\frac12 \| (\mathbf{a} - \mathbf{c}) \times (\mathbf{b} - \mathbf{c}) \|_2
$$

## Setting the number of samples for each face

Just sum up all the triangle areas and convert them into a probability
distribution. If you multiply the number of points you want by the sample to the
distribution, you will get the number of samples per face.

## Sampling points using the Barycentric coordinate

For two random variables $r_1, r_2$ uniformly distributed from 0 to 1, we sample a new point $d$ as follows.

$$
\mathbf{d} = (1 - \sqrt{r_1})\mathbf{a} + \sqrt{r_1} (1 - r_2) \mathbf{b} + \sqrt{r_1} r_2 \mathbf{c}
$$

Intuitively, $r_1$ sets the percentage from vertex $a$ to the opposing edge.
Taking the square-root of $r_1$ gives a uniform random point with respect to surface area.

{% highlight python %}
def sample_faces(vertices, faces, n_samples=10**4):
  """
  Samples point cloud on the surface of the model defined as vectices and
  faces. This function uses vectorized operations so fast at the cost of some
  memory.

  Parameters:
    vertices  - n x 3 matrix
    faces     - n x 3 matrix
    n_samples - positive integer

  Return:
    vertices - point cloud

  Reference :
    [1] Barycentric coordinate system

    \begin{align}
      P = (1 - \sqrt{r_1})A + \sqrt{r_1} (1 - r_2) B + \sqrt{r_1} r_2 C
    \end{align}
  """
  vec_cross = np.cross(vertices[faces[:, 0], :] - vertices[faces[:, 2], :],
                       vertices[faces[:, 1], :] - vertices[faces[:, 2], :])
  face_areas = np.sqrt(np.sum(vec_cross ** 2, 1))
  face_areas = face_areas / np.sum(face_areas)

  n_samples_per_face = np.floor(n_samples * face_areas)
  n_samples = np.sum(n_samples_per_face)

  # Create a vector that contains the face indices
  sample_face_idx = np.zeros((n_samples, ), dtype=int)
  acc = 0
  for face_idx, _n_sample in enumerate(n_samples_per_face):
    sample_face_idx[acc: acc + _n_sample - 1] = face_idx
    acc += _n_sample

  r = np.random.rand(n_samples, 2);
  A = vertices[faces[sample_face_idx, 0], :]
  B = vertices[faces[sample_face_idx, 1], :]
  C = vertices[faces[sample_face_idx, 2], :]
  P = (1 - np.sqrt(r[:,0:1])) * A + np.sqrt(r[:,0:1]) * (1 - r[:,1:]) * B + \
      np.sqrt(r[:,0:1]) * r[:,1:] * C
  return P
{% endhighlight %}
