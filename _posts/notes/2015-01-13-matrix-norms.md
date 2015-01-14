---
layout: article
title: "Interestins Properties of Matrix Norms and Singular Values"
date: 2015-01-13T01:40:06-0800
modified: 
categories: notes
comments: true
excerpt:
mathjax: true
tags: [matrix norm, nuclear norm, trace, rank, L1, sparsity]
ads: true
image:
  feature:
  teaser:
---

Randomly, I came across very a interesting sentence on a random paper.[^1]

> $l_1$ norm of singular values is nuclear norm. $l_2$ norm of singular values is Frobenius norm, $l_\infty$ norm of singular values is the spectral norm.

So in this post, I’ve decied to prove all the claims in the paper.

### Definitions
* Schatten p-Norm

The Schatten p-Norm is defined as the following.[^2]

$$
\|X\|_{S_p} := \left( \sum_i^n s_i(X)^p \right)^\frac{1}{p}
$$

* Nuclear Norm

The nuclear norm of a matrix is defined as a special case of the Schatten p-norm where $p=1$.


* Frobenius Norm

$$
\|X\|_F = \sqrt{ \sum_{i=1}^n \sum_{j=1}^n \lvert a_{ij} \rvert^2 }
$$

* Matrix p-Norm

Matrix p-norm is defined as

$$
\|A\|_p = \sup_{x \neq 0} \frac{\|Ax\|_p}{\|x\|_p}
$$

In another word, matrix p-Norm is defined as the largest scalar that you can get for a unit vector $e$.

* Spectral Norm

Largest singular value of a matrix $\sigma_1(X)$.

Special case of the matrix p-norm where $p=2$ when the matrix $X$ is positive semi-definite. For negative definite matrix, the matrix 2-norm is not necessarily the largest norm.

### Lemmas

1. $ A \in \mathbf{S}^n \; tr(A) = \sum_i^n \lambda_i = \|\|A\|\|_{S_1}$

Let A be a symmetric matrix $A \in \mathcal{S}^{n}$. Then there exists a orthogonal matrix $U$ and diagonal matrix $\Lambda$ such that $A = U \Lambda U^T$. 

$$
\begin{align*}
tr(A) & = tr(A)\\
& = \sum_i^n e_i^T A^T e_i \\
& = \sum_i^n e_i^T U^T \Sigma U e_i \\
& = \sum_i^n \sum_j^n u_{ji}^T \sigma_j u_{ji} \\
& = \sum_j^n \sigma_j \sigma_i^n u{ji}^2\\
& = \sum_j^n \sigma_j
& = \|A\|_{S_1}
\end{align*}
$$

Where $U = \[ u_1, u_2, u_3, \dots u_n\]$. We used the fact that $u_i^T u_i = 1$.

2. $ \|X\|_F =\sqrt\\{ \sum_i^n \sigma_i^2 \\} = \|X\|_\\{S_2\\} $

$$
\begin{align*}
\|X\|_F & = \sqrt{ tr(X^T X) }\\
& = \sqrt{ tr(V \Sigma U^T U \Sigma V^T) }\\
& = \sqrt{ tr(V\Sigma^2 V^T)}\\
& = \sqrt{ \sum_i \sigma_i^2 }
& = \|X\|_{S_2}
\end{align*}
$$

3. $\|A\|_1 = \max_j \sum_{i=1}^n \lvert a_{ij} \rvert$



4. $\|A\|_\infty = \max_j \sum_{i=1}^n \lvert a_{ij} \rvert$

[^1]: B. Recht, M. Fazel, P. A. Parrillo, *Guaranteed Minimum-Rank Solutions of Linear Matrix Equations via Nuclear Norm Minimization*, SIAM Review, Volume 52, Issue 3, pp. 471-501 (2010)
[^2]: http://en.wikipedia.org/wiki/Schatten_norm
