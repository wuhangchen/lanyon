---
title: A Bare Bones Explanation of the Variational Autoencoder
layout: post
comments: true
category: research
tags: [machine-learning, variational-inference]
---

This will be an **extremely** bare bones explanation of the variational autoencoder(VAE). Below is a schematic of the VAE:

**diagram of VAE**


Take $$x$$ as your input data and $$z$$ as latent values which can produce your data. First, I will remind me us of Bayes Formula:

**bayes formula**

Essentially the VAE trie to simulatenously learn a likelihood for your data given latent values and a posterior for the latent values. T $$p(x \vert z)$$ and $$p(z \vert x)$$. 

First, the prior is modeled by a uniform something Gaussian $$\mathcal{N}(0,1)$$.

