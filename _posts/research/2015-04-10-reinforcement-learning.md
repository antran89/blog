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
\begin{align}
V^\pi(s) & = E \left[\sum_0^\infty \gamma r_t | s_0 = s\right]\\
& = E \left[ r_0 + \gamma V^\pi (s_1) | s_0 = s\right]\\
V(s) & = r + \gamma V(s’)\\
V(s) & = \alpha \left[ r_t + \gamma V(s’) - V(s) \right] \\
Q(s,a)& += \alpha (r + \gamma Q(s’, a’))\\
\end{align}
$$

We do update the policy as well

$$
Q(s,a)\\
\pi(s) = \mathop{\arg\!\max}_a Q(s,a)
$$

SARSA d

SARSA, $s, a, r, s’, a’$.
Estimating the $Q$ value of the given  policy $\pi$.


## $Q$- learning

Instead of updating $Q$ with a learning rate $\alpha$,

$$
Q^\pi(s, a)  += \alpha (r + \gamma Q(s’, a’) -  Q(s,a)).
$$

We update using the optimal action.

$$
Q^\pi (s, a) += \alpha (r + \gamma \max_a Q(s’, a’) - Q(s,a)).
$$

In this way, we can learn the optimal policy $Q$ value.


## Eligibility Traces

Keep the history of the state and action to update the $Q$ value of the previous states and actions discounted by  $Q$. 

## Planning

Keep the history of environment and update $Q$ faster.

## Experience replay

TODO

## Andrej Karpathy’s Recommended Reading Materials

- Richard Sutton's Reinforcement Learning (free book online)
<http://webdocs.cs.ualberta.ca/~sutton/book/the-book.html>

- Unfortunately what I barely scratched surface of was DQN (from DeepMind):
<http://www.nature.com/nature/journal/v518/n7540/full/nature14236.html>

And especially Policy Gradients

- David Silver's class: <http://www0.cs.ucl.ac.uk/staff/d.silver/web/Teaching.html>

The recent papers that use policy gradients and I'm aware of are:

- Recurrent Models of Visual Attention: <http://arxiv.org/abs/1406.6247>
- For fine-grained classification: <http://arxiv.org/abs/1412.7054>
- Show Attend and Tell <http://arxiv.org/abs/1502.03044>
- Armand + Tomas paper: <http://arxiv.org/abs/1503.01007>


## Other RL Tutorials

[post](http://chrischoy.org/research/mdp-and-reinforcement-learning-tutorial-lectures)
