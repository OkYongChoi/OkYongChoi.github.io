---
title: "Energy-Based Model"
toc: true
categories:
  - 
tags:
  - 
  - 
  - 

---
#


- Most of the models had the goal of classification or regression, 

- However, **energy-based models** are motivated from a different perspective: **density estimation**.

- Given a dataset with a lot of elements, we want to estimate the probability distribution over the whole data space. 

- However, how do we predict a probability distribution $p(x)$ over so many dimensions using a neural network?

- The problem is that we cannot just predict a score between 0 and 1, because a probability distribution over data needs to fulfill two propertie:  
    1. The probability distribution needs to assign any possible value of $\mathbf{x}$ a non-negative value: $p(\mathbf{x}) \geq 0$.
    2. The probability density must sum/integrate to 1 over all possible inputs: $\int_{\mathbf{x}} p(\mathbf{x}) d\mathbf{x} = 1$.
- Luckily, there are actually many approaches for this, and one of them are energy-based models.

- The fundamental idea of energy-based models is that you can turn any function that predicts values larger than zero into a probability distribution by dviding by its volume. 

$$
q_{\theta}(\mathbf{x}) = \frac{\exp\left(-E_{\theta}(\mathbf{x})\right)}{Z_{\theta}} \hspace{5mm}\text{where}\hspace{5mm}
Z_{\theta} = \begin{cases}
    \int_{\mathbf{x}}\exp\left(-E_{\theta}(\mathbf{x})\right) d\mathbf{x} & \text{if }x\text{ is continuous}\\
    \sum_{\mathbf{x}}\exp\left(-E_{\theta}(\mathbf{x})\right) & \text{if }x\text{ is discrete}
\end{cases}
$$

- We use a negative sign in front of E because we call $E_{\theta}$ to be the energy function:  data points with high likelihood have a low energy, while data points with low likelihood have a high energy.

- The main benefit of this formulation of the probability distribution is its great flexibility as we can choose $E_{\theta}$ in whatever way we like, without any constraints. 

- $Z_{\theta}$ is our normalization terms that ensures that the density integrates/sums to 1.

- Nevertheless, when looking at the equation above, we can see a fundamental issue: How do we calculate $Z_{\theta}$? 

- Although we can’t determine the exact likelihood of a point, there exist methods with which we can train energy-based models. Thus, we will look next at “Contrastive Divergence” for training the model.

# Constrastive Divergence
- When we train a model on generative modeling, it is usually done by **maximum likelihood estimation**.

- As the exact likelihood of a point cannot be determined due to the unknown normalization constant $Z_{\theta}$, we need to train energy-based models slightly different. 

- We cannot just maximize the un-normalized probability $\exp(-E_{\theta}(\mathbf{x}_{\text{train}}))$ because there is no guarantee that $Z_{\theta}$ stays constant, or that $\mathbf{x}_{\text{train}}$ is becoming more likely than the others. 

However, if we base our training on comparing the likelihood of points$p(\mathbg{x})$, we can create a stable objective. Namely, we can re-write our maximum likelihood objective where we maximize the probability of \mathbf{x}_{\text{train}} compared to a randomly sampled data point of our model:
$$
\begin{split}
    \nabla_{\theta}\mathcal{L}_{\text{MLE}}(\mathbf{\theta};p) & = -\mathbb{E}_{p(\mathbf{x})}\left[\nabla_{\theta}\log q_{\theta}(\mathbf{x})\right]\\[5pt]
    & = \mathbb{E}_{p(\mathbf{x})}\left[\nabla_{\theta}E_{\theta}(\mathbf{x})\right] - \mathbb{E}_{q_{\theta}(\mathbf{x})}\left[\nabla_{\theta}E_{\theta}(\mathbf{x})\right]
\end{split}
$$



- The loss is still an objective we want to minimize

- We try to minimize the energy for data points from the dataset ($q_{\theta}(\mathbf{x})$), while maximizing the energy for randomly sampled data points ($p(\mathbf{x})$) from our model

- Namely, the update is in the direction to minimize $\exp(-E_{\theta}(\mathbf{x}))$ for fake answers ($x_{\text{sample}}$), and maximize it for right answers ($x_{\text{train}}$)

- Nevertheless, how is it acually derived from our original distribution $q_{\theta}(\mathbf{x})$? The trick is that we approximate $Z_{\theta}$ by a single Monte-Carlo sample. 

# Sampling from Energy-Based Models
- For sampling from an energy-based model, we can apply a *Markov Chain Monte Carlo* using *Langevin Dynamics*.

- The idea of the algorithm is to start from a random point ($ x_{\text{sample}}) $), afterwards slowly move towards the direction of higher probability to reach a real point ($x_{\text{sample}}$) using the gradients of $E_{\theta}$.

- Nevertheless, this is not enough to fully capture the probability distribution. 

- We need to add noise $\omega$ at each gradient step to the current sample

- Under certain conditions such as that we perform the gradient steps an infinite amount of times, we would be able to create an exact sample from our modeled distribution. However, as this is not practically possible, we usually limit the chain to K steps (K a hyperparameter that needs to be finetuned). 





# Reference
[UvA DL Notebooks](https://uvadlc-notebooks.readthedocs.io/en/latest/tutorial_notebooks/tutorial8/Deep_Energy_Models.html)