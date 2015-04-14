---
layout: article
title: "STAT218 Poisson Complementary Lecture Notes"
date: 2015-04-10T23:34:00-0700
modified:
categories: lectures
mathjax: true
excerpt: 
comments: true
mathjax:  true
tags: [statistics]
ads: true
image:
  feature:
  teaser:
---


This is the complementary material for the STAT218 <a href="http://chrischoy.github.io/blog/lectures/STATS-218-Lecture-4/">lecture 4</a>.


### Intfinite Server Poisson Queue (p.70)

Poisson Thinning process has very interesting property. Let Type I customers be customers if they complete their serice by $t$ and Type II if not. Then, there is a deley between arrival and departure but the thinning process probability $p = \int_0^t G(s) ds$ does capture the delay and the resulting process is again a Poisson process.


### Order Statistics

Another interesting property of the Poisson processes is that events are indepent uniformly distributed given the number of events by time $t$.

One thing though that you have to be careful about is

$$\begin{align}
f(U_{(1)},..., U_{(n)}) & = n! \Pi_{i = 1}^n f(U_{(i)})\\
\Pi_{i = 1}^n f(U_{(i)}) & = \Pi_{i = 1}^n f(U_i).
\end{align}$$

The joint p.d.f would give you extra $n!$ for converting an ordered list to an unordered list, whereas the independent probability distributions would not require $n!$.

### The $M/G/1$ Busy Period

A queueing system $M/G/1$ refers to a process where customers arrive in accordance with a Poisson process with rate $\lambda$ ($M$ denotes memoryless). Upon arrival, a customer will be served if the server is free and will wait in a queue if not. The service time is i.i.d according to $G$. The busy period refers to the length of time the server is occupied (if the queue is not empty, the busy period continues until the queue is empty).


Let $Y_1, Y_2,...$ be thesequence of service times. The busy period will last a time $t$ and will consist of $n$ services iff

- $S_k \le Y_1 + \cdots + Y_k, k = 1,...,n-1$.
- $Y_1 + \cdots + Y_n = t$.
- There are $n-1$ arrivals in $(0,t)$.


