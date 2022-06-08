---
# permalink: /posts/
title: "Reinforcement Learning"
toc: true
categories:
  - reinforcement learning
tags:
  - reinforcement learning
---

# The state variable $s_t$ is sufficient statistics
## Sufficient Statistic
For example, 

The state variable $s_t$ is sufficient to describe the  environment at time step t containing all information relevant to make decision or inference. Sufficiency is with respect to the target optimal inference or decision.

## Markovianness
The sequence of states over time ${s_t , t = 0, 1, 2, · · · }$ is a Markov process (A Markov process is also called a Markov chain (MC)). That is,
$$ \mathbb{P}\left[s_{t+1}|s_1, s_2, · · · , s_t\right] = \mathbb{P}\left[s_{t+1}|s_t \right], \forall t
$$

 - Levels of random process
    - i.i.d.  
    $\cap$
    - Markov ( $\subset Ergodic$ )  
    $\cap$
    - (Strictly) stationary  
    $\cap$
    - Wide sense stationary  
    $\cap$
    - Non-stationary


A random process or sequence ${S_t}$ with $S_t \in \mathcal{S}$, ∀t is Markov if and only if
$$
\mathbb{P}\left[s_{t+1}|s_1, s_2, · · · , s_t\right] = \mathbb{P}\left[s_{t+1}|s_t \right]
$$