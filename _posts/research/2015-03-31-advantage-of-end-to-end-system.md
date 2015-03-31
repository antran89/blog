---
layout: article
title: "Advantage of an end-to-end system: data processing inequality"
date: 2015-03-31T15:24:33-0700
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

These days, making an end-to-end system using CNN or RNN becomes increasingly
popular. For instance, the new RNN for image description [^1] and for object
recognition [^2] are the good examples of the end-to-end system.

From an information theory viewpoint, this has a clear advantage: Data processing inequality[^3].
The data processing inequality states that information loss from  going through a channel is none zero. More formally, let $I(A; B)$ be the mutual information between the set of random variables $A$ and $B$. Then, for a Markov Chain $X\rightarrow Y \rightarrow Z$, where $X \bot Z | Y$, it must be that

$$
I(X,Y) \ge  I(X,Z)
$$

<b>Proof</b>:

$$
\begin{align}
I(X; Y,Z) & = I(X; Z) + I(X; Y | Z)\\
& = I(X; Y) + I(X; Z | Y)
\end{align}
$$

Since it is Markov, $I(X;Z \| Y) = 0$ and $I(\cdot; \cdot) \ge 0$, we have

$$
\begin{align}
I(X; Y) + I(X; Z | Y) & = I(X; Z) + I(X; Y | Z)\\
I(X; Y) & = I(X; Z) + I(X; Y | Z)\\
I(X; Y) & \ge I(X; Z)
\end{align}
$$


Thus, according to the data processing inequality, it is always better to create an end-to-end system.



[^1]: **Translating Videos to Natural Language using Deep Recurrent Neural Networks** (http://arxiv.org/abs/1412.4729)
[^2]: **Mutliple Object Recognition with Visual Attention** (http://arxiv.org/abs/1412.7755)
[^3]: Tom Cover, **Information Theory** Chapter 2.
