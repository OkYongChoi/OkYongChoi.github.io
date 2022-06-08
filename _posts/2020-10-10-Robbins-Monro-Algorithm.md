---
title: "Basic Stochastic Approximation Algorithms"
toc: true
categories:
  - stochastic approximation
tags:
  - 
---


# Robbins-Monro Algorithm
The Robbins-Monro alogrithm is for estimating $x$ with the $n-1$ th given data, and then estimates the new $x$ when the $N$ th data point of new data comes in. We use this algoritghm when the data are sequentially given.

$$ 
x^{(n)} = x^{(n-1)} + \textbf{a}_{n-1}N\left(x^{(n-1)}-\alpha \right)
$$

Here, $T$ denotes the $x$ value that changes due to the new $n$ th data point, and $a_{n-1}$ denotes the learning rate. And $a_n$ must satisfy the following three conditions for $x$ to converge.

$$ 
\lim_{N\to\infty}a_N = 0 \\
\sum_{N=1}^\infty a_N = \infty \\
\sum_{N=1}^\infty a_N^2 < \infty
$$

Actually, the above equation is the same as the equation of the gradient descent model (or the standard Euler scheme $x_{n+1} = x_n + \alpha (f(x_n)-x_n)$). $a_N$ becomes the learning rate, and as new data points come in, learning should gradually converge, but the above condition is attached because the total amount of learning must be finite.

2.3.5 Sequential estimation of Pattern Recognition and Machine Learning 


# Stochastic Appriximation
Stochastic approximation was first introduced by Robbins and Monro in a 1951. Stochastic approximation has provided the basis for many learning or 'parameter tuning' algorithms in soft computing. 

The theory of it has two somewhat distinct strands of research. One, popular with statisticians, uses the techniques of martingale theory and associated convergence theorems for analysis. The second, popular more with engineers, treats the algorithm as a noisy discretization of an ordinary differential equation (ODE) and analyzes it as such.

Vivek S. Borkar, STOCHASTIC APPROXIMATION : A DYNAMICAL SYSTEMS VIEWPOINT, 2008