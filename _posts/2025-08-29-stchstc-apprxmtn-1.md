---
title: "Stochastic Approximation Part 1"  
permalink: /posts/2025/08/stchstc-apprxmtn-1/  
date: 2025-08-29  
tags:  
 - learning   
---
  
What struck my curiosity to investigate why $Q$-learning and SARSA converge was the realization that these methods use 
their estimates of action values at time step $t$ to estimate the action values at time step $t + 1$ in their update
targets. This sounded really weired to me as though the convergence should not happen. So, I dug in and discovered the 
answer lies in the topic of stochastic approximation. Stochastic approximation is fairly a big topic, hence I cover it
in four separate parts. In the last part I'll turn my attention to Q-Learning and SARSA eventually.

I'll mention the assumption required to each proposition during their corresponding proofs to see where those
assumptions were inevitably needed.

# Needed background

## Mathematical Optimization
A mathematical optimization problem is finding the maximum/minimum of a real-valued function.

## Iterative Algorithms
Iterative algorithms solve problems by moving towards the solution one iteration at a time under two conditions:



# (Iterative) Stochastic Approximation
Optimation problems are often solved by the means of the iterative algorithms.



# References
- [Neuro-Dynamic Programming](https://web.mit.edu/dimitrib/www/NDP.pdf)
- [Convex Optimization](https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf)
- [CPSC 327: Data Structures and Algorithms Spring 2025](https://math.hws.edu/bridgeman/courses/327/s25/)
- [CSC 236 H1F, Lecture 8](https://www.cs.toronto.edu/~amir/teaching/csc236f15/materials/lec08.pdf)
- ChatGPT [A lot]