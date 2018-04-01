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

Let's define some notions:

1. \\( X \\): data that we want to model a.k.a the animal
2. \\( z \\): latent variable a.k.a our imagination
3. \\( P(X) \\): probability distribution of the data, i.e. that animal kingdom
4. \\( P(z) \\): probability distribution of latent variable, i.e. our brain, the source of our imagination
5. \\( P(X \vert z) \\): distribution of generating data given latent variable, e.g. turning imagination into real animal

Our objective here is to model the data, hence we want to find \\( P(X) \\). Using the law of probability, we could find it in relation with \\( z \\) as follows:

\\[ P(X) = \int P(X \vert z) P(z) dz \\]

that is, we marginalize out \\( z \\) from the joint probability distribution \\( P(X, z) \\).

Now if only we know \\( P(X, z) \\), or equivalently, \\( P(X \vert z) \\) and \\( P(z) \\)...

The idea of VAE is to infer \\( P(z) \\) using \\( P(z \vert X) \\). This is make a lot of sense if we think about it: we want to make our latent variable likely under our data. Talking in term of our fable example, we want to limit our imagination only on animal kingdom domain, so we shouldn't imagine about things like root, leaf, tyre, glass, GPU, refrigerator, doormat, ... as it's unlikely that those things have anything to do with things that come from the animal kingdom. Right?

But the problem is, we have to infer that distribution \\( P(z \vert X) \\), as we don't know it yet. In VAE, as it name suggests, we infer \\( P(z \vert X) \\) using a method called Variational Inference (VI). VI is one of the popular choice of method in bayesian inference, the other one being MCMC method. The main idea of VI is to pose the inference by approach it as an optimization problem. How? By modeling the true distribution \\( P(z \vert X) \\) using simpler distribution that is easy to evaluate, e.g. Gaussian, and minimize the difference between those two distribution using [KL divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) metric, which tells us how difference it is \\(P\\) and \\(Q\\).

Alright, now let's say we want to infer \\( P(z \vert X) \\) using \\( Q(z \vert X) \\). The KL divergence then formulated as follows:

$$ \begin{align}

D_{KL}[Q(z \vert X) \Vert P(z \vert X)] &= \sum_z Q(z \vert X) \, \log \frac{Q(z \vert X)}{P(z \vert X)} \\[10pt]
                            &= E \left[ \log \frac{Q(z \vert X)}{P(z \vert X)} \right] \\[10pt]
                            &= E[\log Q(z \vert X) - \log P(z \vert X)]

\end{align} $$

Recall the notations above, there are two things that we haven't use, namely \\( P(X) \\), \\( P(X \vert z) \\), and \\( P(z) \\). But, with Bayes' rule, we could make it appear in the equation:

$$ \begin{align}

D_{KL}[Q(z \vert X) \Vert P(z \vert X)] &= E \left[ \log Q(z \vert X) - \log \frac{P(X \vert z) P(z)}{P(X)} \right] \\[10pt]
                                        &= E[\log Q(z \vert X) - (\log P(X \vert z) + \log P(z) - \log P(X))] \\[10pt]
                                        &= E[\log Q(z \vert X) - \log P(X \vert z) - \log P(z) + \log P(X)]

\end{align} $$

Notice that the expectation is over \\( z \\) and \\( P(X) \\) doesn't depend on \\( z \\), so we could move it outside of the expectation.

$$ \begin{align}

D_{KL}[Q(z \vert X) \Vert P(z \vert X)] &= E[\log Q(z \vert X) - \log P(X \vert z) - \log P(z)] + \log P(X) \\[10pt]
D_{KL}[Q(z \vert X) \Vert P(z \vert X)] - \log P(X) &= E[\log Q(z \vert X) - \log P(X \vert z) - \log P(z)]

\end{align} $$

If we look carefully at the right hand side of the equation, we would notice that it could be rewritten as another KL divergence. So let's do that by first rearranging the sign.

$$ \begin{align}

D_{KL}[Q(z \vert X) \Vert P(z \vert X)] - \log P(X) &= E[\log Q(z \vert X) - \log P(X \vert z) - \log P(z)] \\[10pt]
\log P(X) - D_{KL}[Q(z \vert X) \Vert P(z \vert X)] &= E[\log P(X \vert z) - (\log Q(z \vert X) - \log P(z))] \\[10pt]
                                       &= E[\log P(X \vert z)] - E[\log Q(z \vert X) - \log P(z)] \\[10pt]
                                       &= E[\log P(X \vert z)] - D_{KL}[Q(z \vert X) \Vert P(z)]

\end{align} $$

And this is it, the VAE objective function:

$$ \log P(X) - D_{KL}[Q(z \vert X) \Vert P(z \vert X)] = E[\log P(X \vert z)] - D_{KL}[Q(z \vert X) \Vert P(z)] $$

At this point, what do we have? Let's enumerate:

1. \\( Q(z \vert X) \\) that project our data \\( X \\) into latent variable space
2. \\( z \\), the latent variable
3. \\( P(X \vert z) \\) that generate data given latent variable

We might feel familiar with this kind of structure. And guess what, it's the same structure as seen in [Autoencoder]({% post_url 2016-12-03-autoencoders %})! That is, \\( Q(z \vert X) \\) is the encoder net, \\( z \\) is the encoded representation, and \\( P(X \vert z) \\) is the decoder net! Well, well, no wonder the name of this model is Variational Autoencoder!
