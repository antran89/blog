---
layout: article
title: "Interesting Properties of Matrix Norms and Singular Values"
date: 2015-01-13T01:40:06-0800
modified: 2015-01-14T01:53:11-0800
categories: research
comments: true
excerpt:
mathjax: true
tags: [matrix norm, nuclear norm, trace, rank, L1, sparsity]
ads: true
image:
  feature:
  teaser:
---

<div tyle="display:none">
  $
    \DeclareMathOperator*{\argmax}{arg\,max}
  $
</div>

Matrix norms and singular values have special relationships. In this post, I would like to summarize all matrix norms and related norms and derive relationships between them.

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

1. $ A \in \mathbf{S}^n \; tr(A) = \sum_i^n \lambda_i =\lVert A \rVert_{S_1}$

    Trace of a symmetric matrix $A$ is equal to the sum of eigen values.
    Let A be a symmetric matrix $A \in \mathcal{S}^{n}$. Then there exists a orthogonal matrix $U$ and diagonal matrix $\Lambda$ such that $A = U \Lambda U^T$. 

    $$
    \begin{align*}
    tr(A) & = tr(A^T)\\
    & = \sum_i^n e_i^T A^T e_i \\
    & = \sum_i^n e_i^T U^T \Lambda U e_i \\
    & = \sum_i^n \sum_j^n u_{ji}^T \lambda_j u_{ji} \\
    & = \sum_j^n \lambda_j \sum_i u{ji}^2\\
    & = \sum_j^n \lambda_j
    \end{align*}
    $$

    Where $U = \[ u_1, u_2, u_3, \dots u_n\]$. We used the fact that $u_i^T u_i = 1$.

2. $ A \in \mathbf{S}\_+^n \; tr(A) =\sum_i^n \lvert \sigma_i \rvert = \|A\|_{S_1} $

    Trace of a positive semi-definite matrix $A$ is equal to the L1 norm of singular values, or is equal to the Schatten 1-Norm (Nuclear Norm).

    This is the direct extension of Lemma 1.

    $$
    \begin{align*}
    tr(A) & = \sum_i^n \lambda_i\\
    & = \sum_i^n \sigma_i\\
    & = \sum_i^n \| \sigma_i \|\\
    & = \|A\|_{S_1}
    \end{align*}
    $$

    Since the L1 norm of singular values enforce sparsity on the matrix rank, yhe result is used in many application such as low-rank matrix completion and matrix approximation.

3. $ \lVert X\rVert_F = \sqrt{ \sum_i^n \sigma_i^2 } = \lVert X\rVert_{S_2} $

    Frobenius norm of a matrix is equal to L2 norm of singular values, or is equal to the Schatten 2 norm.

    $$
    \begin{align*}
    \|X\|_F & = \sqrt{ tr(X^T X) }\\
    & = \sqrt{ tr(V \Sigma U^T U \Sigma V^T) }\\
    & = \sqrt{ tr(V\Sigma^2 V^T)}\\
    & = \sqrt{ \sum_i \sigma_i^2 }\\
    & = \|X\|_{S_2}
    \end{align*}
    $$

4. $ \lVert A \rVert_1 = \max_j \sum_i^n \lvert a_{ij} \rvert $

    L1 matrix norm of a matrix is equal to the maximum of L1 norm of a column of the matrix.

    To begin with, the solution of L1 optimization usually occurs at the corner. If the function of interest is piece-wise linear, the extrema always occur at the corners. One can show this by showing that $\frac{\|Ax\|_1}{\|x\|_1}$ is Lipschitz except $x = 0$ and differentiate $\frac{\|Ax\|_1}{\|x\|_1}$ w.r.t. $x_i$. Then show that they are non-zero. Use the fact on line 1 and 2.

    Finally, we can just compute the L1 norms of each column. Let’s reformulate the problem.

    $$
    \begin{align*}
    \|A\|_1 & = \sup_{x \neq 0} \frac{\|Ax\|_1}{\|x\|_1}\\
    & = \max_{\|x\|_1 = 1} \sum_j \sum_i^n \lvert a_{ij}\rvert \lvert x_j \rvert\\
    & = \max_j \sum_i \lvert a_{ij} \rvert
    \end{align*}
    $$



5. $ \lVert A \rVert\_\infty = \max_i \sum_j^n \lvert a_{ij} \rvert $


    The supremum occurs at the corner of the hypercube since infinity nomr of a vector is the absolute value of the largest element in it. Thus,
    $$
    \begin{align*}
    \|A\|_\infty & = \max \frac{\|Ax\|_\infty}{\|x\|_\infty}\\
    & = \max_{\|x_j\| = 1} \|Ax\|_\infty \\
    & = \max_i \|a_i^T\|
    \end{align*}
    $$

    Where $a_i^T$ is the $i$ th row of the matrix $A$.


[^1]: B. Recht, M. Fazel, P. A. Parrillo, *Guaranteed Minimum-Rank Solutions of Linear Matrix Equations via Nuclear Norm Minimization*, SIAM Review, Volume 52, Issue 3, pp. 471-501 (2010)
[^2]: http://en.wikipedia.org/wiki/Schatten_norm
