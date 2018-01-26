---
title: Deep Q-Network
comments: true
layout: post
comments: true
category: tutorial
tags: [machine-learning, reinforcement-learning]
---

[Original paper: Human Level Control Through Deep Reinforcement-Learning](http://www.nature.com/nature/journal/v518/n7540/full/nature14236.html)
<!-- ##### Aside
<span style="font-size:0.8em">
I want to get into the habit of writing up instruction for everything that I learn as a way to solidify it in my brain. Many neuroscientists believe the brain employs a generative model to model the world. I hope that by forcing my brain to generate information I'm trying to learn that it will force it to better capture it. 
</span>
 -->
## Model

The premise of the Deep Q-Network is that one can design a reinforcement learning agent that uses a [deep neural network](https://en.wikipedia.org/wiki/Deep_learning) to choose its actions. It does so by estimating the optimality (so called "q-value") of each available action. The network is essentially a 3-layer [convolutional neural network](http://cs231n.github.io/convolutional-networks/) with a 2-layer traditional [fully connected neural network](https://en.wikipedia.org/wiki/Feedforward_neural_network) appended to the front.

<img class="regular materialboxed responsive-img" src="{{ site.baseurl }}/files/dqn/architecture.png">

The size of the final layer is determined by the number of available actions, with each neuron outputting a q-value for its associated action. 
<!-- This architecture has been adapted for a number of tasks but was originally showcased for its ability to perform well on a number of Atari 2600 video games. -->

---

## Algorithm
The general scheme of gameplay is as follows:
1. A game state image $$x_t \in \mathbb{R}^d $$ is observed by the agent at time $$t$$
1. A resultant action is chosen from a set of possible actions $$ \mathcal{A} = \{1, \ldots, K\}$$, $$a_t$$
1. A reward is obtained from the environment $$r_t$$
1. And a subsequent game state image $$x_{t+1}$$ is observed

### Bellman Optimality Equation
How do we combine these things for a reinforcement learning algorithm? We need some way to estimate the expected reward from choosing an action at a particular state. The [Bellman optimality equation](https://en.wikipedia.org/wiki/Bellman_equation) addresses this with a few assumptions. First, that the re

$$
  R_t = \sum_{t'=t}^{T} \gamma^{t'-t}r_t
$$

The bedrock of this algorithm is the [Bellman optimality equation](https://en.wikipedia.org/wiki/Bellman_equation), which is used to approximate the utility of choosing a particular action. 


<!-- Assume that you have some policy that maps states to actions, $$\pi : S \to A $$.  -->

<!-- based on the simple assumption that if you know the utility for  -->

<!-- $$ 
Q_{i+1}(\phi,a)=E[r = \gamma \mathop{\max_{a'}} Q_i(\phi_{i+1},a_{i+1}) |s,a]
$$  -->

### Q-Network



---

## Optimization and Training

---

## Efficiency Tricks

---

## Limitations of the DQN

---

### Traditional Approached

<!-- ## Human-level control through deep reinforcement learning -->

<!-- 
In order to reduce dimensionality of the visual data, RBG Tensors are transformed into 84X84X1 Tensors as follows. The maximum value is taken over each pizel colour value over the frame and the previous frame. -->

<!-- #### Preprocessing
* maximum pixel colour value is taken over frame and previous frame (removes flickering)
* Y-channel is extracfed from RBG Tensor and rescaled to 64 X 84.
* applied to m most revent frames, which are then stacked to produce input for Q-function $$a^2$$ 



#### Architecture

-->