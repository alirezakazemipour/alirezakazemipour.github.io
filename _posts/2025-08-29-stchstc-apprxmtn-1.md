---
title: "Stochastic Approximation Part 1" 
permalink: /posts/2025/09/stchstc-apprxmtn-1/ 
date: 2025-09-7 
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
In this section I'll review some concepts needed throughout the document.

## Level sets of a function
The $\alpha$-sublevel set of a function $f: \mathbb{R}^n \to \mathbb{R}$ is defined as

$$C_\alpha = \{x \in \mathsf{dom}(f): f(x) \leq \alpha \}.$$

## Stationary point of a function
Stationary point of a function is where its derivative is zero

## Limit point of a sequence
Let $\left(a_i \right)^\infty_{i=m}$ be a sequence of real numbers,
let $x$ be a real number, and let $\varepsilon>0$ be a real number.
We say that $x$ is $\varepsilon$-adherent to $\left(a_i \right)^\infty_{i=m}$ if
and only if there exists $n \geq m$ such that $a_n$ is $\varepsilon$-close to $x$. We say that $x$ 
is _continually_ $\varepsilon$-adherent to $\left(a_i \right)^\infty_{i=m}$ if and only if it is $\varepsilon$-adherent
to $\left(a_i \right)^\infty_{i=N}$ for every $N \geq m$. We say that $x$ is a limit point of
$\left(a_i \right)^\infty_{i=m}$ if and only if it is _continually_ $\varepsilon$-adherent
to $\left(a_i \right)^\infty_{i=m}$ for every $\varepsilon > 0$.

## Smooth function
A continuously differentiable function $f: \mathbb{R}^n \to \infty$ is $\beta$-smooth if 

$$f(y) \leq f(x) + \nabla f(x)^\top(y - x) + \frac \beta2 \lVert y - x\rVert^2 \qquad \forall y,x \in \mathbb{R}^n,\, \text{and } \beta > 0.$$

The above condition is equivalent to a Lipschitz continuity over the gradients, i.e., 

$$\lVert \nabla f(y) - \nabla f(x)\rVert \leq \beta \lVert y - x\rVert, \qquad \forall y,x \in \mathbb{R}^n\, \text{and } \beta > 0.$$

This assumption is satisfied, in particular, if $f$ is twice differentiable and all of its second derivatives are bounded globally by some constant.

_proof_.
Assume $\lVert \nabla^2 f(x)\rVert \leq M$, then by mean value theorem we have $\lVert \nabla f(x) - \nabla f(y) \rVert \leq M \lVert x - y\rVert. \square$

_Proof of the equivalence_.
Fix two vectors $r$ and $z$, let $\xi$ be a scalar parameter, and let $g(\xi) = f(r + \xi z)$. The chain rule yields $\frac{d}{d\xi}g(\xi) = z^\top\nabla f(r + \xi z)$. We have
$$
\begin{align*}
f(r + z) - f(r) & = g(1) - g(0) = \int_0^1\frac{d}{d\xi}g(\xi)d\xi = \int_0^1\ z^\top\nabla f(r + \xi z) d\xi \\
& \leq \int_0^1\ z^\top\nabla f(r) d\xi + \left\lvert \int_0^1\ z^\top\Bigl(\nabla f(r + \xi z) - \nabla f(r)\Bigr) d\xi \right\rvert \\
& \leq z^\top\nabla f(r) + \int_0^1\ \lVert z \rVert \cdot \lVert\nabla f(r + \xi z) - \nabla f(r)\rVert d\xi.
\end{align*}
$$

Now assume there exists a $\beta \geq 0$ such that $\lVert \nabla f(x_1) - \nabla f(x_2)\rVert \leq \beta \lVert x_1 - x_2\rVert$ for all $x_1, x_2$ in the domain of $f. Then,

$$\begin{align*}
f(r + z) - f(r) & \leq z^\top\nabla f(r) + \int_0^1\ \lVert z \rVert \cdot \lVert\nabla f(r + \xi z) - \nabla f(r)\rVert d\xi \\
& \leq z^\top\nabla f(r) + \lVert z \rVert \int_0^1\ \beta\xi\lVert z \rVert d\xi \\
& = \leq z^\top\nabla f(r) + \frac \beta2 \lVert z \rVert^2.
\end{align*}$$

By replacing $r = x$ and $r + z = y$ the proof is completed. $\square$

## Filterations
Given a measurable space $(\Omega, \mathcal{F})$,
a filteration is a sequence $\left(\mathcal{F}\_t\right)^n\_{t = 0}$ of sub-$\sigma$-algebras of $\mathcal{F}$, where
$\mathcal{F}\_t \subseteq \mathcal{F}\_{t + 1}$ for all $t < n$, $\mathcal{F}\_n \subseteq \mathcal{F}$,
and $\mathcal{F}\_0 = \{\varnothing, \Omega\}$ (note that the set of
$\mathcal{F}\_0$-measurable functions is the set of constant functions on $\Omega$).
A sequence of random variables $(X_t)^n_{t = 1}$ is adapted to filtration
$\mathbb{F} = \left(\mathcal{F}\_t\right)^n_{t = 0}$  if $X_t$ is $\mathcal{F}_t$-measurable for each t.
We also say in this case that $(X_t)_t$ is $\mathbb{F}$-adapted.

## (Super) Martingale difference sequence
A $\mathbb{F}$-adapted sequence of random variables $(X_t)_{t \in \mathbb{N}}$ is a $\mathbb{F}$-adapted martingale if 
1. $X_t$ is integrable, i.e., $\mathbb{E}\[\|x\|\] < \infty$.
2. $\mathbb{E}\[X_{t} \mid \mathcal{F}\_{t - 1}\] = X_{t - 1}$ almost surely for each $t \in \{2, 3, \dots\}$.
If the equality in the second point is replaced with a less-than, then we call $(X_t)_t$ a supermartingale

## Martingle convergence theorem
Let $X_t, t = 0, 1, 2, \dots$, be a sequence of random variables and let $\mathcal{F}\_t, t = 0, 1,2 \dots$, be sets
of random variables such that $\mathcal{F}\_t \subseteq \mathcal{F}_{t + 1}$ for all $t$. Suppose that:
1. $X_t$ is $\mathcal{F}\_t$-measurable.
2. For each $t$, we have $\mathbb{E}\[X_{t + 1} \mid \mathcal{F}_t\] = X_t$. 
3. There exists a constant $M$ such that $\mathbb{E}\[|X_t|\] \leq M$ for all $t$. 
Then $X_t$ converges to a random variable $X_\infty$ almost surely. 

The proof essentially comes from the fact that $\mathbb{E}\[|X_t|\] \leq M$ make any sub/super martingale upper/lower 
bounded, thus converging. Hence, the martingale will also converge. An example of the proof is given in
the supermartingale convergence section.

## Supermartingale convergence theorem
Let $Y_t, X_t,\, \mathrm{and}\, Z_t, t = 0, 1, 2, \dots,$ be three sequences of random variables and 
let $\mathcal{F}\_t, t = 0, 1, 2, \dots,$ be sets of random variables such that
$\mathcal{F}\_t \subseteq \mathcal{F}_{t+1}$ for all $t$. Suppose that:
1. The random variables $Y_t, X_t,\, \mathrm{and}\, Z_t$ are nonnegative, and are functions of the random variables in $\mathcal{F}_t$.
2. For each $t$, we have $\mathbb{E}\[Y_{t + 1} \mid \mathcal{F}_{t}\] \leq Y_t - X_t + Z_t$.
3. There holds $\sum_{t = 0}^\infty Z_t < \infty$.

Then, we have $\sum_{t = 0}^\infty X_t < \infty$, and the sequence $Y_t$ converges to a nonnegative random variable
$Y$, with probability (w.p.) 1.

_Proof_ (Gemini).
Define the following $\mathcal{F}_t$ measurable process:

$$U_t := Y_t + \sum_{i = 0}^{t - 1}(X_i - Z_i).$$

We show that $\{U_t\}$ is a supermartingale:

$$  
\begin{align*}   
\mathbb{E}[U_{t + 1} \mid \mathcal{F}\_t] &= \mathbb{E}[Y_{t + 1} \mid \mathcal{F}\_t] +  
\sum\_{i = 0}^{t}(Z_i - X_i) \leq Y_t +Z_t - X_t + \sum_{i = 0}^{t}(X_i - Z_i) \\  
& = Y_t + \sum_{i = 0}^{t - 1}(X_i - Z_i) = U_t.  
\end{align*}  
$$

We now show that $\{U_t\}$ is bounded below:

$$U_t := Y_t + \sum_{i = 0}^{t - 1}(X_i - Z_i) \geq 0 + 0 -\sum_{i = 0}^{t - 1}Z_i \geq -\sum_{i = 0}^{\infty}Z_i$$

Since $S_\infty := \sum_{i = 0}^{\infty}Z_i$ is bounded by the assumption, then $U_t \geq -S_\infty$.
Since $\{U_t\}$ is a supermartingale that is bounded below, the Supermartingale Convergence Theorem implies
that $U_t$ must converge to a finite limit w.p. 1. Let's call this limit $U$:

$$\lim_{t\to \infty} U_t = U < \infty.$$

Substituting the definition of $U_t$:

$$\lim_{t \to \infty} \left( Y_t + \sum_{i = 0}^{t - 1}(X_i - Z_i)\right) = U,$$
which means

$$\lim_{t \to \infty} \left( Y_t + \sum_{i = 0}^{t - 1}X_i\right) = U + S_\infty.$$

For the sum of two non-negative sequences to converge to a finite limit, both sequences must be bounded. Hence,
$\lim_{t \to \infty} Y_t = Y_\infty < \infty$, and $\sum_{i = 0}^{\infty}X_i < \infty$, 
which means $X_t \to 0$. $\square$

## Mathematical optimization  
A mathematical optimization problem is finding the maximum/minimum of a real-valued function.  
  
## Iterative algorithms  
Iterative algorithms solve problems by moving towards the solution one iteration at a time under two conditions:  

1. The algorithm is loop invariant meaning that the found solution up until iteration $t$ that is not necessarily the terminal iteration is correct.
2. The algorithm terminates with probability one.
  
  
# (Iterative) Stochastic approximation  
Optimization problems are often solved by the means of the iterative algorithms.  (Iterative) Stochastic approximation algorithms are algorithms that perform the optimization iteratively even in the presence of noise in the available information.

Conceretly, let $H: \mathbb{R}^n \to \mathbb{R}^n$ be an operator and $r \in \mathbb{R}^n$ a real-valued vector. Then, we wanna solve

$$\begin{equation*}
Hr = r.
\end{equation*}
$$

One possible solution is $r := Hr$ or any convex combination of $r$ and $Hr$, i.e., $r:= (1 - \gamma)r + \gamma Hr$ for $\gamma < 1$, which is true since $r := Hr$. An interesting case is the optimization problem

$$\begin{equation*} r = Hr - \nabla f(r) \end{equation*},$$

where the solution $Hr = r$ mandates that $\nabla f(r) = 0$, which means $r$ is the minimizer of $f$ [Spoiler: the fixed point of $H$ is the minimizer of $f$].

In general, we might not have direct access to $Hr$,  but instead some noisy corrupted measurement of it. Then our optimization problem takes the following form, where $w$ denotes the random noise

$$\begin{equation*} r := (1 - \gamma) r + \gamma (Hr + w) \end{equation*}.$$

More explicitly, suppose $Hr = \mathbb{E}[g(r, v)]$, where $v$ is a random variable with the a known distribution $v \sim P(\cdot \mid r)$ and $g$ is a known function. We would like to the equation:

$$r := (1 - \gamma)r + \gamma \mathbb{E}[g(r, v)],$$

but computing $\mathbb{E}[g(r, v)]$ is generally intractable. Since $P(\cdot \mid r)$ is known, through simulation, we can obtain $k$ samples use the following target:

$$Hr = \frac 1k \sum_{i = 1}^k g(r, v_i),$$

and as $k$ gets large, the law of large numbers gives the confidence that we'll find the right answer. On the other extreme, we can set $k$ to one and use a single sample, an approach called the Robbins-Monro algorithm, and get

$$r := (1-\gamma)r + g(r, v_1).$$

In this case we have 

$$r := (1 - \gamma)r +\gamma \left(\mathbb{E}[g(r, v) + g(r, v_1) - g(r, v)] \right) = (1 - \gamma)r +\gamma \left(Hr + w \right),$$

where $w = g(r, v_1) - \mathbb{E}[g(r, v)]$ is the zero mean noise term.

So, in general we're looking for the fixed point of $H$ and we use the following iterative algorithm 

$$r_{t + 1}(i) = (1 - \gamma_t(i))r_t(i) + \gamma_t(i)((Hr_t)(i) + w(i)).$$

Note that we wrote the function equation for each component of $r$, and we made the stepsize $\gamma$ dependent on the iteration. The reason behind making stepsize dependent on the iteration is make sure our iterative algorithm eventually converges to the fixed point and a fixed stepsize doesn't necessary gives us this guarantee. Specifically, the stepsize should meet the following two conditions known as the Robbins-Monro conditions

$$\text{A)} \sum_{t = 0}^\infty \gamma_t(i) = \infty, \qquad \text{B) }\sum_{t = 0}^\infty \gamma^2_t(i) < \infty, \quad \forall i.$$

The first condition says that the stepsize should be big enough so we can make progress and also due to the upper limit of its summation, it mandates that every component should be updated infinitely-often. The second condition says that the stepsize should be small enough so we can converge (even in the presence of noise). Let's see why these to conditions are necessary:

## Why do we need $\sum_{t = 0}^\infty \gamma_t(i) = \infty$? 
The reason is that if we compute how much we have progressed from the initial iteration

$$|r_t(i) - r_0(i)|
\stackrel{\text{traingle inequality}}{\leq} \sum_{\tau = 0}^{t - 1} \gamma_\tau(i) |(Hr_\tau)(i) + w_\tau - r_\tau(i)|,$$

and if the magnitude of the updates $|(Hr_\tau)(i) + w_\tau - r_\tau(i)|$
is bounded and $\sum_{t = 0}^\infty \gamma_t(i)  = A < \infty$, then the algorithm will be confined within
a fixed radius and if the desired solution is outside of that radius, we'll never succeed in getting it.

## Why do we need $\sum_{t = 0}^\infty \gamma^2_t(i) < \infty$? 
Suppose we want to apply the Robbins-Monro algorithm on sequence of i.i.d. random variable with a unknown mean $\mu$ and known variance of one.

$$r_{t + 1} = (1 - \gamma_t)r_t + \gamma_t v_t = (1 - \gamma_t)r_t + \gamma_t \mu + \gamma_t (v_t - \mu).$$

If we want the above equation converge to $\mu$ we need to make sue that the total variance converges to zero. So, we need to show, $\sum_{t = 0}^\infty \mathbb{E}\left[\left(\gamma_t (v_t - \mu)\right)^2\right] \leq \infty$ (Infinite sum of non-zero variables can't become bounded unless they become zero at some point). 

$$\sum_{t = 0}^\infty \mathbb{E}\left[\left(\gamma_t (v_t - \mu)\right)^2\right] \leq \sum_{t = 0}^\infty \gamma_t^2.$$

So, the only way that the total variance is bounded is that $\sum_{t = 0}^\infty \gamma^2_t(i) < \infty$.

Throughout, we implicitly assume that the stepsize sequence meet the Robbins-Monro conditions and I don't mention explicitly again. The stepsize can also easily become a random variable which is the case in reinforcement learning for example, which I'll touch upon it later.

There are three paradigm of optimization problems that can be solved by stochastic approximation. I'll dig into each of separately. Instead of mentioning the required assumptions in the beginning, I'll explain the required assumptions during proofs to see where they're needed (except the Robbins-Monro conditions on the stepsize that I've assumed are met implicitly throughout).
## Convergence under a smooth Lyapunov or potential function

One way if determining the convergence to the fixed point $r^\*$ is introducing a Lyapunov or in other words, a
potential function that act as a distance such that $f(r_{t +1}) < f(r_t)$ whenever $r\_t \neq r^\*$.
Since noise is involved, instead of requiring $f(r_{t +1}) < f(r_t)$, it's more appropriate to want the
*expected direction* of update is a _direction_ of $f$'s decrease. 

In this section our algorithm is of the form 

$$r_{t+1} = r_t + \gamma_t s_t,$$

and for simplicity we assume a universal stepsize sequence for all components of
$r,$ $\Vert \cdot \rVert$ represents the Euclidean norm, i.e., 
$\lVert r \rVert = r^\top \cdot r$, and $\mathcal{F}_t$
represents the history ($\sigma$-algebra) of the algorithm until time $t$ as 

$$\mathcal{F}_t = \{r_0, \dots, r_t, s_0, \dots, s_{t - 1}, \gamma_0, \dots \gamma_t \}.$$ Note that only the sequence of update until time $t-1$ is include in $\mathcal{F}_t$. I'll mention two examples to problems that fit into this paradigm. 

### Stochastic gradient algorithm
In this example we have that 

$$r_{t + 1} = r_{t} - \gamma_t (\nabla f(r_t) + w_t),$$

where $s_t = -\nabla f(r_t) - w_t$, the potential function is $f$, and the expected direction of the update, assuming that $\mathbb{E}[w_t \mid \mathcal{F}_t] = 0$, is $\mathbb{E}[s_t \mid \mathcal{F}_t] = -\nabla f(r_t)$.

### Euclidean norm pseudo-contractions
Recall the Robbins-Monro algorithm,
$$r_{t + 1} = (1 - \gamma_t)r_t + \gamma_t(Hr_t + w_t),$$
where $H$ is a pseudo-contraction with respect to (w.r.t) the Euclidean norm, i.e., 
$$\lVert Hr - r^*\rVert \leq \beta \lVert r - r^* \rVert, \beta \in [0, 1], \: \mathrm{and} \: r^* \, \text{is the fixed point of } H.$$
In this algorithm $f(r) = \frac 12 \lVert r - r^* \rVert^2$ is the potential function, $\nabla f(r) = r - r^*$, and $s_t = Hr_t - r_t + w_t$ is the update direction. Now, we can verify that the expected direction of the update $s_t$ is in a direction of $f$'s decrease:

Using Hölder's inequality and the fact that $H$ us a pseudo-contraction w.r.t the Euclidean norm, we have

$$(Hr - r^*)^\top(r - r^*) \leq \lVert Hr - r^*\rVert \cdot \lVert r - r^* \rVert \leq \beta \lVert r - r^* \rVert^2.$$

Subtract $(r - r^\*)^\top(r - r^\*)$ from both sides, and we get

$$(Hr - r)^\top(r - r*) \leq -(1 - \beta) \lVert r - r^* \rVert^2.$$

With $r = r_t$, the inequality can be rewritten as 

$$\mathbb{E}[s_t \mid \mathcal{F}_t]^\top\nabla f(r_t) \leq -(1 - \beta)\lVert \nabla f(r_t) \rVert^2,$$

which means that $\mathbb{E}[s_t \mid \mathcal{F}_t]$ and $\nabla f(r_t)$ are not orthogonal, and they are in the opposite direction.

---
__Proposition__. Consider the algorithm $r_{t + 1} = r_t + \gamma_ts_t,$ with the potential function $f: \mathbb{R}^n \to \mathbb{R}$. Under certain assumptions that will be stated in the proof, the following holds with probability one:
a): The sequence $f(r_t)$ converges.
b): We have $\lim_{t \to \infty} \nabla f(r_t) = 0$.
c): Every limit point of $r\_t$ is a stationary point of $f$.

---

Note that the above proposition says nothing about the convergence or the boundedness of the sequence $r_t$, however if $f$ has __bounded level sets__, part (1) implies 
the sequence $r_t$ is bounded. Moreover, if $f$ has unique __stationary point__ $r^\*$, part (3) implies that $r^\*$
is the only __limit point__ of $r_t$, and hence $r_t$ converges to $r^\*$.

_Proof_.

We need our first __assumption__ to begin the proof. 

---
__One__: We need to assume $f$ is $L$-smooth.

---

Hence,

$$f(\bar r) \leq f(r) + \nabla f(r)^\top(\bar r - r) + \frac L2 \Vert \bar r - r \Vert^2.$$

By replacing $r = r_t$ and $\bar r  = r_{t + 1} = r_t + \gamma_t s_t$ we have 

$$f(r_{t + 1}) \leq f(r_t) + \gamma_t \nabla f(r_t)^\top s_t + \gamma_t^2 \frac L2 s_t^2.$$

Now we need the next two __assumptions__. We want the magnitude of the update to be comparable to the gradient of $f$, and the expected direction of the update and the direction of $f$'s gradient never get orthogonal. 

___
__Two__: There exists positive constants $K_2, K_2$ such that

$$\mathbb{E}\left[\Vert s_t \Vert^2 \mid \mathcal{F}_t\right] \leq K_1 + K_2 \Vert f(r_t) \Vert^2, \: \forall t.$$

Note that $s_t$ is allowed to be nonzero [because of the noise] even if $\nabla f(r_t)$ is zero.

__Three__: There exists a positive constant $c$ such that

$$c\Vert f(r_t) \Vert^2 \leq -\nabla f(r_t)^\top \mathbb{E}[s_t \mid \mathcal{F}_t], \: \forall t.$$

---

We have 

$$
\begin{align*}
\mathbb{E}[f(r_{t + 1}) \mid \mathcal{F}_t] & \leq f(r_t) - c\gamma_t \Vert \nabla f(r_t) \Vert^2 + \gamma_t^2 \frac L2 \left(K_1 + K_2 \Vert\nabla f(r_t)\Vert^2\right) \\
& = f(r_t) - \gamma_t \left( c - \frac{LK_2\gamma_t}{2} \right)\Vert \nabla f(r_t) \Vert^2 + \frac{LK_1\gamma_t^2}{2}.
\end{align*}
$$

We want to come to conclusion about the convergence of $f(r_t)$s and we already have relationship between $\mathbb{E}[f(r_{t + 1}) \mid \mathcal{F}_t]$ and $f(r_t$) which already is a reminisce of a (sub/super) martingale difference sequence. This relationship can be exploited if we can establish if we actually facing a (sub/super) martingale difference sequence. To so do, define

$$\begin{align*}
X_t &= \begin{cases}
\gamma_t \left( c - \frac{LK_2\gamma_t}{2} \right)\Vert \nabla f(r_t) \Vert^2, & \mathrm{if}\; LK_2\gamma_t \leq 2c, \\
0, & \mathrm{otherwise},
\end{cases}\\
&\mathrm{and}\\
Z_t &= \begin{cases}
\frac{LK_1\gamma_t^2}{2}, & \mathrm{if}\; LK_2\gamma_t \leq 2c, \\
\frac{LK_1\gamma_t^2}{2} - \gamma_t \left( c - \frac{LK_2\gamma_t}{2} \right)\Vert \nabla f(r_t) \Vert^2, & \mathrm{otherwise}.
\end{cases}
\end{align*}
$$

Note that $X_t$ and $Z_t$ are nonnegative and $\mathcal{F}\_t$ measurable. Using the assumption
$\sum\_{t=0}\infty \gamma_t^2 < \infty$, $\gamma_t$ converges to zero and 
there exists some finite time after which $LK_2\gamma_t \leq 2c$. 
Hence, after some finite time we have $Z_t = \frac{LK\_1\gamma_t^2}{2}$ and 
therefore $\sum_{t = 0}^\infty Z_t < \infty$. Therefore, to use the positive
supermartingale convergence theorem, we introduce the next __assumption__ we need.

---
__Four__: $f(r) \geq 0, \forall r \in \mathbb{R}^n$.

---

Now, the positive supermartingale convergence applies: $f(r_t)$ converges and $\sum_t X_t < \infty$. Since $\gamma_t$ converges to zero, we have $LK_2\gamma_t \leq c$ after some finite time, and 

$$X_t = \gamma_t \left( c - \frac{LK_2\gamma_t}{2} \right)\Vert \nabla f(r_t) \Vert^2 \geq \frac c2 \gamma_t\lVert \nabla f(r_t)\rVert^2.$$

Hence, 

$$\sum_{t = 0}^\infty \gamma_t\lVert \nabla f(r_t)\rVert^2 < \infty.$$

If $\lVert \nabla f(r_t)\rVert$ doesn't converge to zero, then the condition $\sum_{t =0}^\infty \gamma_t = \infty$ creates a contradiction to the above finite inequality. Hence, it must be case that $\lVert \nabla f(r_t)\rVert$ gets infinitely-often arbitrarily close to zero, i.e., $\liminf_{t \to \infty} \lVert \nabla f(r_t)\rVert=0$. Why $\liminf$ and not simply $\lim$? Because of the noise $\lVert \nabla f(r_t)\rVert$ fluctuates, so now we need to prove that its fluctuations dampen as well so that actually $\lim_{t \to \infty}\lVert \nabla f(r_t)\rVert = 0$. 

To prove that $\lVert \nabla f(r_t)\rVert$ won't be oscillating, now we show that it has finite _upcrossings_.

Fix  a positive constant $\epsilon$. We say that the interval $\{t, t+1, \dots, \bar{t}\}$ is an upcrossing interval from $\epsilon/2$ to $\epsilon$ if

$$\Vert \nabla f(r_t) \Vert < \frac{\epsilon}{2}, \quad \Vert \nabla f(r_\bar{t}) \Vert > \epsilon,$$

and

$$\frac{\epsilon}{2} \leq \Vert \nabla f(r_\tau) \Vert \leq \epsilon, \quad t < \tau < \bar{t}.$$

To show the finiteness of the upcrossings for any sample path $\{r_t\}$, we need to show that the effect of the noise terms $w_t$ will be dampened out. Define $\bar{s}_t = \mathbb{E}[s_t \mid \mathcal{F}_t]$, so $w_t = s_t - \bar{s}_t$. Using assumption 2 we have

$$\Vert \bar{s}_t \Vert^2 + \mathbb{E}\left[\Vert w_t\Vert^2 \mid \mathcal{F}_t\right] = \mathbb{E}\left[\Vert s_t\Vert^2 \mid \mathcal{F}_t\right] \leq K_1 + K_2 \Vert \nabla f(r_t) \Vert^2, \quad \forall t.$$

We define the following $\mathcal{F}_t$ measurable indicator random variable that indicates whether an upcrossing has occurred or not:

$$\chi_t = \begin{cases}1, & \mathrm{if}\, \Vert \nabla f(r_\tau) \Vert \leq \epsilon \\ 0, & \mathrm{otherwise}. \end{cases}$$

The following lemma states that the cumulative discounted effect of noise on the events that upcrossings happen converges almost surely, which we will show later convergence is to zero.

---
__Lemma__. The sequence defined by $u_t$ converges w.p. 1:

$$u_t = \sum_{\tau =0}^{t - 1}\chi_\tau\gamma_\tau w_\tau, \quad u_0 := 0.$$

_Proof_.
Initially assume that $\sum_{t=0}^\infty \gamma_t^2 \leq A$, for some constant $A$. Note that:

$$\mathbb{E}[\chi_t\gamma_t w_t \mid \mathcal{F}\_t] = \chi_t\gamma_t \mathbb{E}[w_t \mid \mathcal{F}\_t]=0,$$

and therefore $\mathbb{E}\left[u_{t + 1} \mid \mathcal{F}\_t\right] = \mathbb{E}\left[u_t + \chi_t\gamma_t w_t \mid \mathcal{F}_t\right] = u_t,$
and $\{u_t\}$ is a martingale difference sequence.

If $\chi_t$ is zero, then $\mathbb{E}\left[\Vert u_{t+1} \Vert^2 \mid \mathcal{F}\_t \right] =
\mathbb{E}\left[\Vert u_{t}\Vert^2 \mid \mathcal{F}\_t \right] = \Vert u_{t}\Vert^2$. If on the other hand,
$\chi_t$ is one then,

$$
\begin{align}
\mathbb{E}\left[\Vert u_{t+1} \Vert^2 \mid \mathcal{F}_t \right]& = \mathbb{E}\left[\Vert u_t + \gamma_t w_t\Vert^2 \mid \mathcal{F}_t \right] \stackrel{\text{triangle inequality}}{\leq} \mathbb{E}\left[\left(\Vert u_t \Vert + \Vert \gamma_t w_t\Vert \right)^2 \mid \mathcal{F}_t \right] \\
& \leq \Vert u_{t} \Vert^2 + 2u^\top_t\gamma\mathbb{E}[w_t \mid \mathcal{F}_t] + \gamma^2_t\mathbb{E}[\Vert w_t \Vert^2 \mid \mathcal{F}_t] \\
& \leq \Vert u_{t} \Vert^2  + \gamma^2_t\left(K_1 + K_2\epsilon^2\right).
\end{align}
$$

Now, we take an unconditional expectation from the both sides and apply the tower rule, and sum over $t$ and apply the telescopic sum to obtain

$$\mathbb{E}\left[\Vert u_t \Vert^2 \right] \leq (K_1 + K_2\epsilon^2)\sum_{\tau = 0}^\infty \gamma_\tau^2 \leq (K_1 + K_2\epsilon^2)A, \quad \forall t,$$

and since $\Vert u_t \Vert \leq 1 + \Vert u_t \Vert^2$, we have $\sup_t \mathbb{E}[\Vert u_t \Vert] < \infty$. Now, we can apply the martingale convergence theorem to $\{u_t\}$ and conclude it almost surely converges. 

Now let's consider the case that $\gamma_t$ is stochastic and $\sum\_{t=0}^\infty \gamma^2\_t$ is finite (which implies
$\gamma_t$ has finite variance) but not by a deterministic constant. Consider any arbitrary positive integer $k$ and
let $u^k_t$ represent the process that is equal to $u\_t$ as long as $\sum\_{t=0}^\infty \gamma_t \leq k$ and stays
constant afterward.
Let $\Omega\_k$ denote the set of sample paths $(r_0, r_1, \dots)$ for which $u^k_t$ doesn't converge.
Since $\sum\_{t=0}^\infty \gamma^2\_t < \infty$ is finite, for every sample path there and $k$, there exists a
time $t_0$, where  $\sum^\infty\_{t=t\_0}\gamma^2\_t \leq k$ almost surely, hence the set $\cup\_{k=1}^\infty \Omega_k$
has measure zero, for every sample path and $k$, there exists a time $u_t = u_t^k$ for all $t \geq t_0$ and $u_t$
converges almost surely. 

---

Let us now consider a sample path with an infinity of upcrossings and let $\{t_k, \dots, \bar{t}\_k\}$ be the $k$th such interval. Using the above lemma we obtain:
$$\lim_{k \to \infty} \sum_{t = t_k}^{\bar{t}\_k - 1} \gamma_t w_t = 0,$$
which also implies that $\lim_{k \to \infty} \gamma_{t_k} w_{t_k} = 0$. Now we have

$$
\begin{align*}
\Vert \nabla f(r_{t_k + 1}) \Vert - \Vert \nabla f(r_{t_k}) \Vert & \leq \Vert \nabla f(r_{t_k + 1}) - \nabla f(r_{t_k})\Vert \\
&\leq L \Vert r_{t_k + 1} - r_{t_k} \Vert \\
&= L \gamma_{t_k} \Vert \bar{s}_{t_k} + w_{t_k} \Vert \\
& \leq L \gamma_{t_k} \Vert \bar{s}\_{t_k} \Vert +  L \gamma_{t_k} \Vert w_{t_k} \Vert.
\end{align*}
$$

The right hand side of the above display goes to zero as $k$ goes to infinity
because $\Vert \bar{s}\_{t_k} \Vert^2$ is bounded by $K_1 + K_2 \epsilon^2$ and $\gamma_t$ goes to zero,
and we have just proved it for $\Vert w_{t_k} \Vert$ as well. Then, since $\Vert \nabla f(r_{t_k + 1})
\Vert \geq \epsilon/2$ (the condition of an upcrossing interval in the definition of $\chi_{t}, \, 
t \in [t_k, \, \bar{t}\_k]$), we have $\Vert \nabla f(r_{t_k}) \Vert \geq \epsilon/4$. Hence, for every $k$ we have

$$
\begin{align*}
\frac \epsilon2 & \leq \Vert \nabla f(r_{\bar{t}_k}) \Vert - \Vert \nabla f(r_{t_k}) \Vert \\
& \leq \Vert \nabla f(r_{\bar{t}_k}) - \nabla f(r_{t_k}) \Vert \\
& \leq L \Vert r_{\bar{t}_k} - r_{t_k} \Vert \\
& \leq L \sum_{t = t_k}^{\bar{t}\_{t_k} -1} \gamma_t\Vert \bar{s}_t\Vert + L \sum_{t = t_k}^{\bar{t}_{t_k} -1} \gamma_t\Vert w_t\Vert.
\end{align*}
$$

We have proved that the second term on the right-hand side of the above display goes to zero. Also,
for $t_k \leq t \leq \bar{t}\_k - 1, \, \Vert \bar{s}\_t \Vert^2 \leq K\_1 + K\_2 \epsilon^2$, which by the inequality
$x \leq x^2 +1$ implies that $\Vert \bar{s}\_t \Vert \leq 1 + K\_1 + K\_2 \epsilon^2:=d$. By taking the $\liminf_{k \to \infty}$ from the above display we have

$$
\liminf\_{k \to \infty} \sum_{t = t_k}^{\bar{t}\_k - 1} \gamma_t \geq \frac{\epsilon}{2Ld}.
$$

For $t_k \leq t \leq \bar{t}_k - 1$ we have

$$
\begin{align*}
\liminf_{k \to \infty} \sum_{t = t_k} ^{\bar{t}_k - 1} \gamma_t \Vert \nabla f(r_t)\Vert^2 \geq \frac{\epsilon}{2Ld} \cdot \frac{\epsilon^2}{16}.
\end{align*}
$$

This means by summing over all upcrossing intervals we get that
$\sum_{t=0}^{\infty} \gamma_t \Vert \nabla f(r_t)\Vert^2 =\infty$
(infinite sum of positive numbers is infinite). This is a contradiction because after assumption 4 we had shown
that $\sum\_{t=0}^{\infty} \gamma_t \Vert \nabla f(r_t)\Vert^2 < \infty$, hence the number of upcrossings should be finite.

Given that $\Vert \nabla f(r_t) \Vert$ comes infinitely often arbitrarily close to zero and since there 
are finitely many upcrossings, it follows that $\Vert \nabla f(r_t) \Vert$ can exceed $\epsilon$ only a 
finite number of times, and lim $\limsup_{t \to \infty}\Vert \nabla f(r_t) \Vert \leq \epsilon$.
Since $\epsilon$ was arbitrary, it follows that $\limsup_{t \to \infty}\Vert \nabla f(r_t) \Vert  = 0$, and
part (b) of the proposition has been proved. Finally, if $r$ is a limit point of $r_t$,  is the limit of some
subsequence of $\nabla f(r_t)$ and must be equal to 0, which establishes part (c).

$$\begin{equation*}\textbf{END OF PART 1!}\end{equation*}$$

# References  
- [Neuro-Dynamic Programming](https://web.mit.edu/dimitrib/www/NDP.pdf)  
- [Convex Optimization](https://web.stanford.edu/~boyd/cvxbook/bv_cvxbook.pdf)  
- [CPSC 327: Data Structures and Algorithms Spring 2025](https://math.hws.edu/bridgeman/courses/327/s25/)  
- [CSC 236 H1F, Lecture 8](https://www.cs.toronto.edu/~amir/teaching/csc236f15/materials/lec08.pdf)  
- [Calculus](https://ocw.mit.edu/courses/res-18-001-calculus-fall-2023/mitres_18_001_f17_full_book.pdf)
- [Bandit algorithms](https://tor-lattimore.com/downloads/book/book.pdf)
- [Instructor: Parimal Parag](https://ece.iisc.ac.in/~parimal/2018/spqt/lecture-20.pdf)
- [Introduction to Online Convex Optimization](https://arxiv.org/pdf/1909.05207)
- [Analysis 1](https://tiu-edu.uz/media/books/2024/05/28/1664976801.pdf)
- ChatGPT and Google Gemini [Both a lot]