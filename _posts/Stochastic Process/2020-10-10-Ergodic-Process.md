---
title: "Ergodic Process"
toc: true
categories:
  - 
tags:
  - stochastic process 
---
# Concept of Ergodic Process
Let's say we want to know the order of the film genres that people will watch the most for the next year.

Of course there are lots of ways to estimate the number of times people watched each film genre. But we will use only two ways; The first method is to select any day and count all the movie genres watched that day; the second is to select any one person and count which movie genres that person watches over the next year.

The first method has a problem in that it does not reflect the results accumulated over a long period of time, and in the second case, whether one person can represent all people.

This is where the concept of ergodic comes in. If the above two approaches give the same result, we say that the random process is ergodic. In other words, the time average and the ensemble average give the same result. Therefore, if any random process is ergodic, one single sample path can represent statistical properties.

In many cases, for some random process which is not an ergodic process, it is not realistically possible to have enough sample functions to compute the ensemble mean. However, if a random process is known to be ergodic, then only one sample function is needed to find the ensemble mean.

In the real world, many stationary processes are ergodic up to second-order statistics such as expectation and autocorrelation. As an example of using this, when dealing with a stationary process in connection with a linear system, only the first or second mean needs to be known, and therefore only one sample function is required in most cases.

# Mathematical Representation
Now, let's take a look at the mathematical representation now. For a stochastic process ${X_t}$, set $x(t)$ as a sample path (or sample function) of it. Then for a sample path $x(t)$, the sample average and the sample autocorrelation is as follows.

$$ 
\bar{x} = \lim_{T \to \infty} \frac{1}{2T} \int_{-T}^{T}x(t)dt \\
\bar{R}_{X} = \lim_{T \to \infty} \frac{1}{2T} \int_{-T}^{T}x(t)x(t+\tau)dt
$$

Ergodic process satisfy the following two properties

$$
\bar{x} = \mu_0 \\
\bar{R}_{X} =R_{X}(\tau)
$$

Here is the simple hierarchy of random processes. If a random process is ergodic, then it's stationary.  
 - i.i.d $\subset$ Markov $\subset$ Ergodic $\subset$ Stationary  
 $\subset$ W.S.S $\subset$ Unstationary $\subset$ Stochastic Process


