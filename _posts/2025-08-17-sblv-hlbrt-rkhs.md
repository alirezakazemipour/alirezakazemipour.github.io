---
title: "Sobolev spaces (not too wiggly functions), Hilbert spaces, and RHKS"
permalink: /posts/2025/08/sblv/
date: 2025-08-17
tags:  
 - learning 
---

This post will be hell of a post! Haha 🤩

Ah before I forget, Sobolev spaces are useful in the regime of partial derivatives.

# Supplementary definitions and notation

In this section we review the required notation and concepts.

## Multi-index $\vert \alpha \vert$

A list $\alpha = (\alpha_1, \dots, \alpha_n)$ of integers $\alpha_i \geq 0$ is a __multi-inex__ of order 

$$\vert \alpha \vert := \alpha_1 + \dots \alpha_n.$$

## Closure of a set $A$

$$\bar{A} = A \cup \{\text{all limit points of } A\}.$$

### Intuitive meaning

-   Take  $A$  and “fill in” all the boundary points you can approach from within  $A$.
-   If  $A$ is already closed, its closure is just  $A$.
-   If  $A$  is open, its closure adds all the points “on the edge.”

## (Lebesgue) integrability

First, let's visit some background. 

### $\sigma$-algebra. 

 A $\sigma$-algebra on a set $\mathcal{X}$ is set of subsets of $\mathcal{X}$ that is closed under certain operations:
 
 1. The whole set and the empty set are in $\mathcal{F}$: $\varnothing, \mathcal{X} \in \mathcal{F}$.
 2. Closed under complement: If $A \in \mathcal{F}$, then $A^\complement \in \mathcal{F}$.
 3. Closed under countable unions: If $A_1, A_2, \dots \in \mathcal{F}$, then $\cup_{i = 1}^\infty A_i \in \mathcal{F}$. 

### Borel $\sigma$-algebra

The smallest $\sigma$-algebra on a topological  space $\mathcal{X}$ that contains all open sets is called the Borel σ-algebra: $\mathfrak{B}(\mathcal{X})$.

### A measure $\mu$

A measure $\mu: \mathcal{F} \to [0, \infty)$ is a function whose domain is a $\sigma$-algebra with he following conditions:

1.  At least for one element of $X \in \mathcal{F}, \, \mu(X) < \infty$.
2. $\mu$ is $\sigma$-additive: $\mu\left(\cup_{i = 1}^\infty X_i \right)  = \sum_{i = 1}^\infty \mu(X_i)$, where $\left(X_i \right)_{i = 1}^\infty \in \mathcal{F}$ are countable  pairwise disjoint sets.

Elements of $X \in \mathcal{F}$ are called __measurable sets__. Those with measure zero $\mu(X) = 0$ are called __null sets__.  On $\mathfrak{B}(\mathbb{R}^n)$, there is a unique measure,, called __Borel measure__ on $\mathbb{R}^n$, which is translation invariant and assigns measure 1 to the unit cube $[0, 1]^n$.

### Convergence

Let $(X, d)$ be a metric space. A sequence $x_n$ of points in $X$ is said to converge to a limit $x \in X$ if one has $d(x_n, x) \to \infty$. As $n \to \infty$. In this case, we say that $x_n \to x$ in the metric $d$, as $n \to \infty$ and that $\lim_{n \to \infty} x_n \to x$ in the metric space.

### Cauchy sequence 

Let $(X, d)$ be a metric space. A sequence $(x_n)_{n=1}^\infty$ of points in $X$ is Cauchy sequence if $d(x_n x_m) \to \infty$ as $n,m \to \infty$. In other words, for every $\varepsilon > 0$, there exists $N > 0$ such that $d(x_n, x_m) \leq \varepsilon$ for all $n,  m \geq N.$

### Completeness

A space is complete if every Cauchy sequence is convergent. 

#### Lebesgue measure

In the definition of the measure, the domain was a $\sigma$-algebra. So the input to the measure is set, but the subset of the chosen set is not necessarily in the $\sigma$-algebra. Hence, if the chosen set has a measure zero, the subset would not which would be wierd. This can happen in Borel measure space. Lebesgue $\sigma$-algebra with its corresponding  measure $\lambda$ completes this gap by including every subset of the null sets to the $\sigma$-algebra and thus making measure zero sets consistent.

A function $f: \mathbb{R}^n \to \mathbb{R}$ is called (Lebesgue) measurable if $\mathbb{R}^n$ equipped with Lebesgue $\sigma$-algebra $\mathfrak{L}(\mathbb{R}^n)$, and $\mathbb{R}$ equipped with Borel $\sigma$-algebra $\mathfrak{B}(\mathbb{R})$ and pre-images $f^{-1}(B) \in \mathfrak{L} (\mathbb{R^n})$, for all Borel measurable sets $B \in \mathfrak{B}(\mathbb{R})$ are $\mathfrak{L}(\mathbb{R}^n)$ measurable. $f$ will be called Borel measurable $\mathfrak{B}(\mathbb{R}^n)$ if $f^{-1}(B) \in \mathfrak{B}(\mathbb{R}^n) \subset \mathfrak{L}(\mathbb{R}^n)$.

### Almost everywhere

Two function $f, g: \mathbb{R}^n \to \mathbb{R}$ are  equal _almost everywhere_ if the set
{$f \neq g$} $\subset \mathbb{R}^n$ is a Lebesgue null set.  

## $\mathcal{L}^p(\Omega)$ spaces

Let $f: \mathbb{R}^n \to \mathbb{R}$ be a measurable function. For $p \in [1, \infty]$ the $L^p$ norm of $f$ is the extended real defined by

$$\lVert f \rVert_p := \left( \int_{\mathbb{R}^n} |f|^p\right)^{\frac{1}{p}} \: \in [0, \infty].$$

On the other hand, consider the following (possibly empty) set:

$$I_f = \left\{ c \in [0, \infty): \text{ the set } \{|f| > c\} \text{ has zero measure}\right\} \subset [0, \infty).$$

The infimum of such $c$ is called the $L^\infty$ norm of $f$:

$$ \lVert f \rVert_\infty := \inf I_f = \mathrm{ess}\,\sup |f|.$$

By convention, $\inf \varnothing = \infty$. Hence $\lVert f \rVert_\infty \in [0, \infty].$

Let $p \in [0, \infty]$. A measurable function $f: \mathbb{R}^n \to \mathbb{R}$ with finite $L^p$ norm is called $p$-integrable or an $L^p$-function.  A $1$-integrable function is called integrable. $\mathcal{L}^p(\mathbb{R}^n)$ is the space of real $L^p$-functions. Note that non-degeneracy does not apply to $L^p$ so it is not a norm. The reason is that $\lVert f \rVert_p = 0 \Rightarrow f = 0$ is an almost everywhere statement which doesn't mean $f$ is absolutely zero.

### Locally (Lebesgue) integrable functions $\mathcal{L}^p_\textsf{loc}(\Omega)$

### $Q \Subset \Omega$, pre-compact subset $Q$ of $\Omega$

An open subset of $\Omega$ whose closure $Q^\complement$ is compact and contained in $\Omega$.

### The characteristic function $\chi_E: \mathbb{R}^n \to \mathbb{R}$ of $E$ is defined to be

$$ \chi_E(x) =
 \begin{cases}
1, & x \in E; \\
0, & x \notin E.
\end{cases}$$

Let $p \in [1, \infty]$. $\mathcal{L}^p_\textsf{loc}(\Omega)$ is set of all measurable functions $f: \mathbb{R}^n \to \mathbb{R}$ such that $\chi_Q f$ is $p$-integrable for every pre-compact $Q \Subset \mathbb{R}^n$.

## The space $C^\infty_c(\Omega)$

The space of infinitely often diﬀerentiable real functions with compact (closed and bounded) support, hence the subscript $c$, in $\Omega$ is defined by

$$
C^\infty_c(\Omega) = \{v: v \in  C^\infty(\Omega), \, \mathrm{supp}(v) ⊂ Ω \},$$

where

$$ \mathrm{supp}(v) = \overline{\{\mathbf{x} \in \Omega: v(\mathbf{x}) \neq 0 \}}.$$

These functions are called *test functions*. They are used to “localize” problems without worrying about boundary effects. Because they vanish in a neighborhood of the boundary.

##  Weak derivatives $D^\alpha$

$$D^\alpha f(x) := \frac{\partial^{\vert \alpha \vert}f(x)}{\partial x_1^{\alpha_1} \dots \partial x_n^{\alpha_n}}.$$

Weak derivatives are the extension of the classical derivatives to non-differentiable functions (like $\vert x \vert$).

__Definition__. Suppose $\Omega$ is an open set in $\mathbb{R}^n$. Let $F, f \in L_\textsf{loc}^1(\Omega)$.
A function $u$ belongs to $L_\textsf{loc}^1(\Omega)$, if for each compact (closed bounded) subset $\Omega' \subset \Omega$, it holds

$$\int_{\Omega'} |u(\mathbf{x})|d\mathbf{x} \leq \infty.$$

If for all functions $g \in C^\infty_c(\Omega$), it holds that

$$\int_\Omega f(\mathbf{x}) g(\mathbf{x})d\mathbf{x} =  (-1)^{\vert \alpha \vert} \int_\Omega F(\mathbf{x}) D^\alpha g(\mathbf{x})d\mathbf{x} ,$$

then $f(x)$ is called weak derivative of $F(x) $with respect to the multi-index $\vert \alpha \vert$.

###  On the weak derivatives

• The notion ‘weak’ is used in mathematics if something holds for all appropriate test elements (test functions).

• If $F(x)$ is classically diﬀerentiable on $\Omega$, then the classical derivative is also the weak derivative.

• The assumptions on $F(x)$ and $f(x)$ are such that the integrals in the definition of the weak derivative are well defined. In particular, since the test functions vanish in a neighborhood of the boundary, the behavior of $F(x)$ and $f(x)$ if $x$ approaches the boundary is not of importance.

• The main aspect of the weak derivative is due to the fact that the (Lebesgue) integral is not influenced from the values of the functions on a set of (Lebesgue) measure zero. Hence, the weak derivative is defined only up to a set of measure zero. It follows that $F(x)$ might be not classically diﬀerentiable on a set of measure zero, e.g., in a point, but it can still be weakly diﬀerentiable.

• The weak derivative is uniquely determined, in the sense described above.

__Example__.  The weak derivative of the function $F(x) = \vert x \vert$ is

$$
f(x) = 
\begin{cases}
1, & x < 0; \\
0, & x = 0; \\
1, & x > 0.
\end{cases}
$$



# Main definitions

In this section, we'll give the main definitions we were looking for.

## Sobolev space

Suppose $\Omega$ is an open set in $\mathbb{R}^n, k \in \mathbb{N}$, and $1 \leq p \leq \infty$. The Sobolev space $W^{k, p}(\Omega)$ consists of all locally integrable functions $f: \Omega \to \mathbb{R}$ such that

$$
\begin{equation*}
	D^\alpha f \in L^p(\Omega), \quad \text{for all } 0 \leq \lvert \alpha \rvert \leq k.
\end{equation*}  
$$
Or, in other words, 

$$
W^{k,p}(\Omega) = \left\{f \in L_{\textsf{loc}}^1(\Omega): D^\alpha f \in L^p(\Omega) \text{ with } \vert \alpha \vert \leq k \right\}.
$$

For $u \in W^{k, p}(\Omega)$, this space is equipped with the norm

$$ \lVert u \rVert_{k, p} = \left(\int_\Omega \sum_{\vert \alpha \vert \leq k} \left|D^\alpha u(x)\right|^p dx\right)^\frac{1}{p} = \left( \sum_{\vert \alpha \vert \leq k} \left\lVert D^\alpha u(x)\right\rVert_p^p \right)^\frac{1}{p} \: \in [0, \infty].$$


## Hilbert space

Hilbert space $H$ is a complete normed vector space, where the norm is defined to be the square root of the inner product for $f, g \in H$ $\langle f, g \rangle := \sqrt{\int fg}$. 

 Note that $\mathcal{L}^2(\Omega)$ is a Hilbert space when the non-degeneracy is taken care of through the quotient space. Complete normed vector spaces are called _Banach spaces_, hence Hilbert spaces are Banach spaces. $W^{2, k}(\Omega)$ is a Hilbert space as well. 

## Reproducing kernel Hilbert space (RKHS)

Consider the following linear real-valued function class, where $\psi$ is a predefined feature vector:

$$\mathcal{F} = \left\{f(x; w): f(x; w) = \psi(x) w^\top = \left\langle w, \phi(x) \right\rangle \right\}.$$

In supervised learning we smetimes look for the following minimizer of the empirical risk measure:

$$\hat{w} = \arg\min_w \left\{\frac{1}{n} \sum_{i=1}^{n} \ell \left(\langle w, \phi(X_i) \rangle, Y_i \right) + \frac \lambda2 \Vert w \Vert^2  \right\}.$$

The problem is that if $\psi(x)$ is infinite-dimensional, the inner product can't be computed. The remedy is to use the kernel trick. Assume $k(x_1, x_2) = \langle \psi(x_1), \psi(x_2) \rangle$. If $w = \sum_{i =1}^{n}\alpha_i \psi(x_i)$, then $f(x) = \sum_{i =1}^{n}\alpha_i k(x_i, x)$, and $\langle w, w \rangle = \sum_{i = 1}^{n} \sum_{j = 1}^{n} \alpha_i \alpha_j k(x_i, x_j).$ This observation gives way to the reproducing Hilber kernel space's definition:

Given a kernel $k$ such that $\sum_{i = 1}^{n} \sum_{j = 1}^{n} \alpha_i \alpha_j k(x_i, x_j) \geq 0$, we define RKHS of $k$ as the function space $\mathcal{H}_0$ of the form 

$$\mathcal{H}_0 = \left \{f(x): f(x) = \sum_{i = 1}^m \alpha_i k(x_i, x) = k\pmb{\alpha}^\top\right\},$$

with the inner product (norm) defines as

$$\lVert f(x) \rVert_\mathcal{H}^2 = \sum_{i = 1}^m f(x_i)^2 = \mathbf{f}^\top\mathbf{f}  = \pmb{\alpha}k^\top k\pmb{\alpha}^\top =  \pmb{\alpha} \mathbf{K}\pmb{\alpha}^\top = \sum_{i = 1}^{m} \sum_{j = 1}^{m} \alpha_i \alpha_j k(x_i, x_j) = \langle w, w\rangle,$$

where 

$$\mathbf{K} = \begin{bmatrix}
k(x_1, x_1), & \dots&  k(x_1, x_m) \\
 \vdots & \cdots & \vdots \\
 k(x_m, x_1), & \dots & k(x_m, x_m) 
\end{bmatrix}
$$ 

is the kernel Gram matrix.


# References

 1. [Mathematical Analysis of Machine Learning Algorithms](https://tongzhang-ml.org/lt-book.html)
 2. [# 245B, Notes 8: A quick review of point set topology](https://terrytao.wordpress.com/2009/01/30/254a-notes-8-a-quick-review-of-point-set-topology/)
 3. [Topology and Modern Analysis](https://archive.org/details/introduction-to-topology-and-modern-analysis-simmons)
 4. [ChatGPT](ChatGPT.com)
 5. [Introduction To Functional Analysis](https://ocw.mit.edu/courses/18-102-introduction-to-functional-analysis-spring-2021/)
 6. [Functional Analysis, Lecture notes for 18.102, Spring 2020](https://ocw.mit.edu/courses/18-102-introduction-to-functional-analysis-spring-2021/3d4cc88026d44a01f936cd6a0aa995cb_MIT18_102s20_lec_FA.pdf)
 7. [Notes on Partial Diﬀerential Equations, Chapter 1 & 3](https://www.math.ucdavis.edu/~hunter/pdes/pde_notes.pdf)
 8. [Introduction to Sobolev Spaces, Lecture Notes MM692 2018-2](https://www.math.stonybrook.edu/~joa/PUBLICATIONS/SOBOLEV.pdf)
 9. [Numerical Mathematics 3, Chapter 3](https://www.wias-berlin.de/people/john/LEHRE/lehre.html)

 
 