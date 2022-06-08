---
title: "Martingale Process"
toc: true
categories:
  - 
tags:
  - stochastic process 
  - financial engineering
  - random work
---
# Martingale
We can check whether the probability process is fair or not by checking check whether the martingale property is satisfied for a certain stochastic process. The definition of the martingale process is as follows.

$$
\mathbb{E}\left[X_t|\{\mathcal{F}_\tau, \tau \le s\} \right] = X_s, \forall s \le t.
$$

Now, we can think about why I said that the martingale can represent the fairness. Let's think about the financial market. In the martingale process, the expected value is equal to the initial value (investment). If the expected value is greater than the investment, it becomes an advantageous process for the investor. Conversely, if the expected value is less than the investment, it is a process that is unfavorable to the investor. The former process is called sub martingale, and the latter is called a super martingale.

# 
Is the *random walk* process a fair probability process? What is the expected return on investment in this process? The answer to these questions can be found by examining the martingale property of this process. 

$$
\begin{align*}
\mathbb{E}\left[X_t|\mathcal{F}_\tau \right]
&= \mathbb{E}\left[(X_t - X_\tau) + X_\tau|\mathcal{F}_\tau \right] \\ 
&= \mathbb{E}\left[(X_t - X_\tau)|\mathcal{F}_\tau\right] +\mathbb{E}\left[X_t|\mathcal{F}_\tau \right] \\
&= \mathbb{E}\left[(X_t - X_\tau)|\mathcal{F}_\tau\right] + X_t  \\ 
&= \mathbb{E}\left[(X_t - X_\tau)\right] + X_t \\ 
&= X_t
\end{align*}
$$

The last equation represents that it is the martingale. Thus, the random walk process is a martingale. If calculated in the same way as above, both the the *Wiener process* and *geometric Brownian motion* satisfy the properties of the martingale. Since the stock price model is also based on the Wiener process, it satisfies the properties of the martingale. Then the stock price is also a martingale process and can be said to be a fair probability process. (For your information, the fairness of the market can be explained by *risk neutral probability* and *effective market hypothesis (EMH)*. However, the concept of martingale is used as a more specific expression in financial engineering.)