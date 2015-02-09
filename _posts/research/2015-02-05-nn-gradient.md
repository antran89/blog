---
layout: article
title: "Computing Neural Network Gradients"
date: 2015-02-05T13:55:18-0800
modified:
categories: research
excerpt:
mathjax: true
comments: true
tags: [gradient, neural network]
toc: true
ads: true
image:
  feature:
  teaser:
---

Computing neural network gradient requires very simple calculus yet can be hairsplitting.

## Affine Transformation (Fully Connected Layer) Gradients

Simple fully connected neural network with batch size $n$ and the feature dimension $d_i$ and output feature dimension is $d_o$ or $d_i$ neurons as an input and $d_o$ neurons as outputs. The weight $W \in \mathbb{R}^{d_o \times d_i}$ and $b \in \mathbb{R}^{d_o}$. The resulting response $Y \in \mathbb{R}^{d_o \times n}$

$$
Y = AX + b
$$

The summation is a broadcasting sum that makes the summation consistent.

The gradient $\frac{\partial Y\_{ij}}{\partial W\_{pq}}$ can be computed as

$$
\begin{align*}
\frac{\partial Y_{ij}}{\partial W_{pq}} & = \frac{\partial}{\partial W_{pq}}\left( \sum_l W_{il}X_{lj} + b_j\right) \\
& = X_{iq} \delta_{ip} \delta_{jq}
\end{align*}
$$

also the gradient for the scaler $b$ is 

$$
\begin{align*}
\frac{\partial Y_{ij}}{\partial b_{p}} & = \frac{\partial}{\partial b_{p}}\left( \sum_l W_{il}X_{lj} + b_j\right) \\
& = \delta_{jp}
\end{align*}
$$

where $\delta_{ij} = 1$ if $i = j$ otherwise $0$.

$$
\begin{align*}
\frac{\partial Y_{ij}}{\partial X_{pq}} & = \frac{\partial}{\partial X_{pq}}\left( \sum_l W_{il}X_{lj} \right) \\
& = W_{iq} \delta_{ip} \delta_{jq}
\end{align*}
$$

Let’s say that the gradient from the upper layer propagated gradient $\frac{\partial E}{\partial Y} \in \mathbb{R}^{n \times d_o}$ to the current layer. The resulting gradient for the weight $W$ is

$$
\begin{align*}
\frac{\partial E}{\partial W} & = \frac{\partial E}{\partial Y} \frac{\partial Y}{\partial W} \\
& = \frac{\partial E}{\partial Y} X^T
\end{align*}
$$

$$
\begin{align*}
\frac{\partial E}{\partial b} & = \frac{\partial E}{\partial Y} \frac{\partial Y}{\partial b} \\
& = \sum_i \frac{\partial E}{\partial Y_i}
\end{align*}
$$

where $Y_i$ is the $i$ the datapoint in the batch.
The gradient propagated back to the lower layer is

$$
\begin{align*}
\frac{\partial E}{\partial X} & = \frac{\partial E}{\partial Y} \frac{\partial Y}{\partial X} \\
& = W^T \frac{\partial E}{\partial Y}
\end{align*}
$$

The only thing that you have to be careful is to match the dimension of the matrices. Everything will work out if you just match the dimensions of matrices that you multiply.

## ReLU Gradients

Rectified Linear Unit (ReLU) has been widely used for simplicity and faster convergence. The ReLU allows stronger gradients to propagate back to the lower layers unlike sigmoid units or hyperbolic tangent units. Since there is no weights to learn, we can just compute the gradients $\frac{\partial E}{\partial X}$. where $Y = f(X)$ and the $f(\cdot)$ is the element-wise ReLU.

$$
\begin{align*}
\frac{\partial E}{\partial X} & = \frac{\partial E}{\partial Y} \frac{\partial Y}{\partial X}\\
& = \frac{\partial E}{\partial Y} \circ \Delta(X)
\end{align*}
$$

where $\circ$ is the Hadammard product and $\Delta(X)$ has elements $\delta\_{ij} = I\left(x\_{ij} > 0 \right)$.

## Generalized Convolution Gradients

The convolution is a conventional signal filtering technique. In here, you can also think of it as 2D or ND filter that extract edges, blobs, or high frequency changes. In this example, we follow the *caffe* *Blob* convention and compute 2D convolution.

Unlike traditional signal processing convolution, neural network *convolution* is a generalization of convolution $$\ast$$. For instance, there is the *stride* which controls the step size, and you can think of the standard convolution as a convolution with the stride 1. Also the output convolution size is not $n + k - 1$ where $n$ is the input signal size and $k$ is the kernel size. The neural network convolution kernel always fits within the input size. Thus the output size is $n - k + 1$.

I will use $\ast_s$ to denote the convolution with stride $s$. Suppose that the result after convolution is $ Y = F \ast_s X + b$ where $ Y \in \mathbb{R}^{n \times h_y \times \times w_y}$ and $F \in \mathbb{R}^{c \times h_f \times w_f}$ and $X \in \mathbb{R}^{c \times h_x \times w_x}$ and $b \in \mathbb{R}$.

The summation is a broadcasting sum that makes the summation consistent.

If we add paddings along width and height, the problem becomes simplified since we do not have to account the borderline cases where filter only sees not enough data.

$$
\begin{align*}
\frac{\partial Y_{nhw}}{\partial X_{ncij}} & = \sum_{k\in \{i, i+s, i+2s, \dots, i+rs\}} \sum_{l\in \{j, j+s, j+2s, \dots, j+os\}} W_{ckl}\\
& = \sum_{k = 0}^{r-1} \sum_{l = 0}^{o-1} W_{c,(i + ks),(j + ls)}
\end{align*}
$$

Where $s$ is the stride (this is not standard convolution parameter). In standard convolution, the stride is 1. $$o$$ and $$r$$ are the number of strides it can make to make the indexing plausible.

For parameters,

$$
\begin{align*}
\frac{\partial Y_{nhw}}{\partial W_{cij}} & = X_{n,(h + i),(w + j)}\\
\frac{\partial Y_{nhw}}{\partial b} & = 1
\end{align*}
$$

Thus the gradient propagated through the network will be

$$
\begin{align*}
\frac{\partial E}{\partial X_{ncij}} & = \sum_k \sum_l \frac{\partial E}{\partial Y_{nkl}} \frac{\partial Y_{nkl}}{\partial X_{ncij}}\\
& = \sum_k \sum_l \frac{\partial E}{\partial Y_{nkl}} \sum_{k = 0}^{r-1} \sum_{l = 0}^{o-1} W_{c,(i + ks),(j + ls)}\\
\end{align*}
$$

Also for the weight update,

$$
\begin{align*}
\frac{\partial E}{\partial W_{cij}} & = \sum_p \sum_q \frac{\partial E}{\partial Y_{nij}} \frac{\partial Y_{nij}}{\partial W_{cij}}\\
& = \sum_p \sum_q \frac{\partial E}{\partial Y_{npq}} \sum_{k = 0}^{r-1} \sum_{l = 0}^{o-1} X_{c,(i + ks),(j + ls)}\\
\frac{\partial E}{\partial b} & = \sum_k \sum_l \frac{\partial E}{\partial Y_{nkl}} \frac{\partial Y_{nkl}}{\partial b}\\
& = \sum_k \sum_l \frac{\partial E}{\partial Y_{nkl}}
\end{align*}
$$
