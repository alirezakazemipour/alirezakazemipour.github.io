---
title: "Induction by Contradiction" 
permalink: /posts/2025/09/indctn-by-cntrdctn/ 
date: 2025-09-12 
tags:        
  - learning      
---


In this post, we wanna prove _the correctness of the proof by induction itself_. Lol

__Axiom__ (Well-ordering property). The well-ordering property of $\mathbb{N}$ states that if $S \subseteq \mathbb{N}$ and $S \neq \varnothing$, then there exists an $x \in S$ such that $x \leq y$ for all $y \in S$. In other words, there is always a smallest element.

__Theorem__ (Induction). This concept was invented by Pascal in 1665. Let $P (n)$ be a statement depending on $n \in \mathbb{N}$. Assume that

1. (Base case) $P(1)$ is true and

2. (Inductive step) if $P(m)$ is true then $P(m + 1)$ is true.

Then, $P(n)$ is true for all $n \in \mathbb{N}$.

_Proof_.

Let $S=$ {$n ∈N |P (n)$ is not true}. We wish to show that $S= \varnothing$. We will prove this by contradiction. When we prove something by contradiction, we assume the conclusion we want is false, and then show that we will reach a false statement. Rules of logic thus imply that the initial statement must be false.

Suppose that $S \neq \varnothing$. Then, by the well-ordering property of $\mathbb{N}$, $S$ has a least element $m \in S$. Since according to the theorem $P(1)$ is true, then it must be that $m > 1$. Since $m$ is a least element of $S$, $m - 1 \notin S \Rightarrow P(m−1)$ is true. According to the theorem, this implies that $P(m)$ is true which implies that $m \in S$ by assumption. But, this is a contradiction. Thus $S = \varnothing$, and hence $P(n)$ is true for all $n \in \mathbb{N}$.

# References
- [Real Analysis](https://ocw.mit.edu/courses/18-100a-real-analysis-fall-2020/mit18_100af20_lec110.pdf)