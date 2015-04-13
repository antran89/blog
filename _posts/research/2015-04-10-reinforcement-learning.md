---
layout: article
title: "Reinforcement Learning"
date: 2015-04-10T13:16:21-0700
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


## Temporal difference method

$$
\begin{algin}
V^\pi(s) &= E [\sum_0^\infty \gamma r_t | s_0 = s]\\
& = E [ r_0 + \gamma V^\pi (s_1) | s_0 = s]\\
V(s) & = r + \gamma V(s’)\
V(s) & = \alpha [ r_t + \gamma V(s’) - V(s) ] \\
Q(s,a)& += \alpha (r + \gamma Q(s’, a’))\\
\end{align}
$$

We do update the policy as well

$$
Q(s,a)\\
\pi(s) = \argmax_a Q(s,a)
$$

SARSA d

SARSA, $s, a, r, s’, a’$.
Estimating the $Q$ value of the given  policy $\pi$.


## $Q$- learning

Instead of 
$$
Q^\pi(s, a)  += \alpha (r + \gamma Q(s’, a’) -  Q(s,a))
$$

We update using the optimal action.

$$
Q^\pi (s, a) += \alpha (r + \gamma \max_a Q(s’, a’) - Q(s,a))
$$

In this way, we can learn the optimal policy $Q$ value.


## Eligibility Traces

Keep the history of the state and action to update the $Q$ value of the previous states and actions discounted by  $Q$. 

## Planning

Keep the history of environment and update $Q$ faster.

## Experience replay


