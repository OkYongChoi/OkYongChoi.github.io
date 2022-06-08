---
title: "Linear Quadratic Gaussian (LQG)"
toc: true
categories:
  - 
tags:
  - machine learning
  - estimation
  - control
---

## LQG = LQR + LQE
The linear quadratic gaussian (LQG) is a method of obtaining the estimated state using the control gain obtained through LQ optimal control and the Kalman filter, that is, through the combination of linear quadratic regulator (LQR) and linear quadratic estimator (LQE). This is also called *stochastic optimal control*. The LQG could be very sensitive to model uncertainties, which means that even very small uncertainties can lead to an unstable LQG.  
The LQR can be applied if the system is *fully controllable*. It takes the full state vector and transforms it into an input signal to achieve desired properties of the dynamical system. In many practical problems, the full state vector is hardly measured. In order to estimate the full state vector we must check if the system is *observable* (e.g. by Kalman's observability criterion), if the system is observable then it is possible to estimate the full state vector by a Kalman filter and use this estimate of the full state vector as input for the LQR controller. 

## Linear Quadratic Regulator (LQR, also called linear quadratic controller)

## Linear Quadratic Estimator (LQE, also known as Kalman filer)