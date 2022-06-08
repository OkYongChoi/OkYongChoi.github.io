---
title: "Martingale Difference Sequence"
toc: true
categories:
  - 
tags:
  - stochastic process 
---

## Martingale Difference Sequence (MDS)
 - Zero-mean random variables (r.v.) can be frequently considered as noises.
 - If the r.v. of sequence at time t is uncorrelated with the past r.v. of it, then the r.v. can sometimes be thought of as noise.

The stochastic series ${S_t}$ is refered to as the martingale difference sequence (MDS), if it has zero-mean given the any past of itself. That is the both above statements hold true for the MDS.

$$
\mathbb{E}\left[S_{n+1}|\mathcal{F}_m , m\le n \right] = 0
$$

Additionally, the MDS is useful in that it is looser than independence condition.

Its name comes from the fact that the mean of the differences of martingales $X_t - X_{t-1}$ is zero.
For your information, differencing is frequently used to make random sequence stationary. It usually helps stabilize the mean of a time series by removing changes in the level of a time series, and so eliminating trends.

## Azuma-Hoeffding Inequality


https://en.wikipedia.org/wiki/Martingale_difference_sequence#:~:text=will%20be%20an%20MDS%E2%80%94hence,also%20hold%20for%20an%20MDS.