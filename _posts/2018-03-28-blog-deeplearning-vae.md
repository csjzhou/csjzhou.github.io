---
layout: post
title: "Variational Autoencoder: Intuition and Implementation"
show-avatar: true
tags:
  - blogs
  - python
  - VAE
published: true
---


From [blog](https://wiseodd.github.io/techblog/2016/12/10/variational-autoencoder/)

There are two generative models facing neck to neck in the data generation business right now: Generative Adversarial Nets (GAN) and Variational Autoencoder (VAE). These two models have different take on how the models are trained. GAN is rooted in game theory, its objective is to find the Nash Equilibrium between discriminator net and generator net. On the other hand, VAE is rooted in bayesian inference, i.e. it wants to model the underlying probability distribution of data so that it could sample new data from that distribution.

In this post, we will look at the intuition of VAE model and its implementation in Keras.

## VAE: Formulation and Intuition

Suppose we want to generate a data. Good way to do it is first to decide what kind of data we want to generate, then actually generate the data. For example, say, we want to generate an animal. First, we imagine the animal: it must have four legs, and it must be able to swim. Having those criteria, we could then actually generate the animal by sampling from the animal kingdom. Lo and behold, we get Platypus!

From the story above, our imagination is analogous to **latent variable**. It is often useful to decide the latent variable first in generative models, as latent variable could describe our data. Without latent variable, it is as if we just generate data blindly. And this is the difference between GAN and VAE: VAE uses latent variable, hence it's an expressive model.

Alright, that fable is great and all, but how do we model that? Well, let's talk about probability distribution.
