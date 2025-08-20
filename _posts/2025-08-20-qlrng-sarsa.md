---
title: "Why does Q-Learning and SARSA converge?"  
permalink: /posts/2025/08/qlrng-sarsa/  
date: 2025-08-20  
tags:  
 - learning   
---
  
What struck my curiosity to investigate why $Q$-learning and SARSA converge was the realization that these methods use 
their estimates of action values at time step $t$ to estimate the action values at time step $t + 1$ in their update
targets. This sounded really weired to me as though the convergence should not happen. So let's dive in what's going on.

# $Q$-learning


# SARSA


# References
- [Reinforcement Learning: Foundations](https://sites.google.com/view/rlfoundations/home)