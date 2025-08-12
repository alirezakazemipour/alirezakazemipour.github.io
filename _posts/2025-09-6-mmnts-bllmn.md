---
title: "Random variables' moments and the second moment Bellman equation"  
permalink: /posts/2025/08/mmnts-bllmn/  
date: 2025-09-6  
tags:
  - learning
---
  
Let's see what Sir [Larry Wasserman](https://www.stat.cmu.edu/~brian/valerie/617-2022/0%20-%20books/2004%20-%20wasserman%20-%20all%20of%20statistics.pdf) has to tell us about it.

# Definitions 

__Definition__. The $k^{\text{th}}$ moment of a random variable $X$ is $\mathbb{E}\left[X^k\right]$, assuming $\mathbb{E}\left[|X|^k\right]$ exits. With this definition the mean is the first moment.

__Definition__ .The variance of a random variable $X$ with mean $\mu$ is defined by

$$\mathbb{V}(X) = \mathbb{E}[X - m]^2.$$

The relation between the first and second moment, and the variance is as follows (the variance is the difference between the second and the first moment square):

$$\mathbb{V}(X) = \mathbb{E}[X^2] - \mathbb{E}[X]^2 .$$

# Mean and variance in discounted MDPs

I follow the derivation of [Tamar, et al. 2013](https://jmlr.org/papers/volume17/14-335/14-335.pdf).

We assume we're in the episodic settings. The state space is finite $\mathcal{X} = \{1, \dots, n\}$ and there is a special terminal state $x_T$. We assume the agent follows a _fixed_ proper (definition below) policy $\pi$. Let $r_\pi \in \mathbb{R}^n$ and $P_\pi \in \mathbb{R}^{n \times n}$ denote the expected reward vector and the transition probability induced by $\pi$. Let $\tau = \min \{t > 0 \mid X_t = X_T \}$ denote the first visit time to the terminal state, and let the random variable $G$ denote the accumulated discounted reward along the trajectory until that time

$$G = \sum_{t = 0}^{\tau - 1} \gamma^t r_\pi(X_t).$$

Then, the first and the second moment of $G$ are equal to

$$J(x) = \mathbb{E}[G \mid X_0 = x] \\
M(x) = \mathbb{E}[G^2 \mid X_0 = x] .$$

Let's expand each. First $J(x)$:

$$\begin{align*} 
J(x)  &= \mathbb{E} \left[ \sum_{t = 0}^{\tau - 1} \gamma^t r_\pi(X_t) \middle\vert X_0 = x \right] \\
&= r_\pi(x) + \mathbb{E} \left[ \sum_{t = 1}^{\tau - 1} \gamma^t r_\pi(X_t) \middle\vert X_0 = x\right] \\
& = r_\pi(x) + \gamma \mathbb{E} \left[ \mathbb{E}\left[\sum_{t = 1}^{\tau - 1} \gamma^{t - 1} r_\pi(X_t) \middle\vert X_0 = x, X_1 = x' \right]\right] & \text{(tower rule)} \\
& = r_\pi(x) + \gamma \mathbb{E}\left[J(x') \right] \\
& = r_\pi(x) + \gamma \sum_{x'} P_\pi\left(x' \middle\vert x\right)J\left(x'\right). &  \text{(Bellman equation)}
\end{align*} $$

Now $M(x)$:
$$\begin{align*} 
M(x)  &= \mathbb{E} \left[ \left(\sum_{t = 0}^{\tau - 1} \gamma^t r_\pi(X_t)\right)^2 \middle\vert X_0 = x \right] \\
&= \mathbb{E} \left[ \left(r_\pi(x) + \sum_{t = 1}^{\tau - 1} \gamma^t r_\pi(X_t)\right)^2 \middle\vert X_0 = x \right] \\
& = r_\pi(x)^2 + 2r_\pi(x)\mathbb{E} \left[  \sum_{t = 1}^{\tau - 1} \gamma^t r_\pi(X_t) \middle\vert X_0 = x \right] + \mathbb{E} \left[ \left(\sum_{t = 1}^{\tau - 1} \gamma^t r_\pi(X_t)\right)^2 \middle\vert X_0 = x \right] \\
& r_\pi(x)^2 + 2\gamma r_\pi(x) \sum_{x' }P_\pi\left(x' \middle\vert x\right)J(x') + \gamma^2 \sum_{x' }P_\pi\left(x' \middle\vert x\right)M(x').
\end{align*} $$

So the variance of $G$ at state $x$ is equal to

$$\begin{align*}
V(x) = \mathbb{V}(G \mid X_0=x) & = M(x) - J(x)^2  \\
& = \gamma^2 \sum_{x' }P_\pi\left(x' \middle\vert x\right)M(x') - \left(\gamma \sum_{x'} P_\pi\left(x' \middle\vert x\right)J\left(x'\right)\right)^2 \\ 
& = \gamma^2\left[ \sum_{x' }P_\pi\left(x' \middle\vert x\right)M(x')  - \left(\sum_{x'} P_\pi\left(x' \middle\vert x\right)J\left(x'\right)\right)^2 \right] \\
& \geq \gamma^2\left[ \sum_{x' }P_\pi\left(x' \middle\vert x\right)M(x')  - \sum_{x'} P_\pi\left(x' \middle\vert x\right)^2 J\left(x'\right)^2 \right] & \text {(Cauchy Swhartz)} \\
& \geq \gamma^2\left[ \sum_{x'} P_\pi\left(x' \middle\vert x\right) \left(M\left(x'\right) - J(x')^2\right) \right] \\
& = \gamma^2 \mathbb{E}\left[V(x') \right].
\end{align*}$$

The above display shows that the variance of the return for a state is mostly influenced by the states that are temporally close to it. Further states' variance is discounted at a fast geometric rate. For example, the effect of the variance at the state just before the terminal state on the initial state is about $\gamma^{2n}$. This matches our intuition as well. 

__Definition__. [Bertsekas, Neuro dynamic programming] A stationary policy $\pi$ is said to be proper if, using this policy, there is positive probability that the termination state will be reached after at most $n$ stages, regardless of the initial state, that is, if

$$\max_{x = 1, \dots, n} \mathbb{P}_{\pi} \left( X_n \neq x_T \mid X_0=x\right) < 1$$.