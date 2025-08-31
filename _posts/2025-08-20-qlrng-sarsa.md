---  
title: "Why does Q-Learning and SARSA converge? (a.k.a Stochastic Approximation Part 4)"
 permalink: /posts/2025/08/qlrng-sarsa/ 
 date: 2025-08-20 tags:    
- learning   
---  

# In Progress!!! 👇

What struck my curiosity to investigate why $Q$-learning and SARSA converge was the realization that these methods use   
their estimates of action values at time step $t$ to estimate the action values at time step $t + 1$ in their update  
targets. This sounded really weird to me as though the convergence should not happen. So let's dive in what's going on.  

# Facts

**Law of large numbers still holds for ergodic processes.**  Even with correlated samples, averages converge under mild mixing conditions (this is why you can estimate expectations from Markov chains — same principle as MCMC). That's why Monte-Carlo methods do need iid assumption. 
    
Also, in RL, we often assume episodes are independent  _because_  the environment resets (to a start state distribution). This makes the return samples conditionally i.i.d. given the start state.


# Assumptions

The MDP is ergodic (aperiodic + irreducible). The policy the agent follows is fixed for a long time. These two assumptions make us assume that if the agent chooses actions according to a Markov policy $\pi$, we should expect to obtain tuples that roughly (because the aforementioned assumptions are not met in practice) follow the stationary distribution of the Markov chain corresponding to $\pi$.


# Stochastic approximation

Why do we need stochastic approximation techniques? To handle stochasticity. I'll show that $Q$-learning and SARSA are instantiations of stochastic approximation scheme, hence I'll spend almost the whole document on stochastic approximation, and $Q$-learning and SARSA just become immediate.
  
# $Q$-learning  

The update rule:

$$Q_{t + 1}(S_t, A_t) = Q_t(S_t, A_t) + \alpha_t(S_t, A_t)\left[R_{t + 1} + \gamma \max_a Q_t(S_{t + 1}, a) -  Q_t(S_t, A_t) \right]$$.

  
  
# SARSA  

The update rule:

$$Q_{t + 1}(S_t, A_t) = Q_t(S_t, A_t) + \alpha_t(S_t, A_t)\left[R_{t + 1} + \gamma Q_t(S_{t + 1}, A_{t + 1}) -  Q_t(S_t, A_t) \right]$$.
  
  
# References  
- [Reinforcement Learning: Foundations](https://sites.google.com/view/rlfoundations/home)
- [Neuro-Dynamic Programming](https://web.mit.edu/dimitrib/www/NDP.pdf)
- ChatGPT [A lot]