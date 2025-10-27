---
title: "Convergence of Positive Supermartingales"  
permalink: /posts/2025/10/cnvrgnc-sprmrtngl/ 
date: 2025-10-27  
tags:
- learning
---

 
[//]: Latex Macros

$$
\begin{align*}
\newcommand{\I}{\mathbb{1}}
\newcommand{\R}{\mathbb{R}}
\newcommand{\Q}{\mathbb{Q}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\sB}{\mathscr{B}}
\DeclareMathOperator{\EE}{\mathbb{E}}
\DeclareMathOperator{\PP}{\mathbb{P}}
\newcommand{\Ev}[2]{\EE^{#2}\left[ #1 \right]}
\newcommand{\Pr}[2]{\PP^{#2}\left( #1 \right)}
\end{align*}
$$

I really enjoyed reading Chapter two of [Discrete Parameter Martingales](https://www.sciencedirect.com/bookseries/north-holland-mathematical-library/vol/10/suppl/C) and I thought why not making a blog post. 

The book is quite advanced, but I'll try my best state everything clearly and simply given my temporal boredom threshold. Haha

Let me fix the probability space first. Our probability space (as usual) is $(\Omega, \mathscr{F}, \mathbb{P})$. HOWEVER, in what follows it is more convenient to define a subset of the above probability space as $(\Omega', \mathscr{B}, \mathbb{P})$ (because we're going to work with filterations and naturally they are sub-$\sigma$-algebras that are growing, so we're always in the subset case).  A mapping $X$ only defined on $\Omega'$ is random variable on $\Omega'$ if it is measurable with respect to (w.r.t.) the trace $\sigma$-algebra $\mathscr{F} \cap \Omega'$. So, what is a trace $\sigma$-algebra? I found it [here](https://proofwiki.org/wiki/Definition:Trace_Sigma-Algebra).

---
__Definition__. Let $\Omega$ be a set, and let $\mathscr{F}$ be a $\sigma$-algebra on $\Omega$. Let $\Omega' \subseteq \Omega$ be a subset of $\Omega$. Then, the trace $\sigma$-algebra (of $\Omega'$ in $\mathscr{F}$), $\mathscr{B}$, is defined as:

$$
\mathscr{B} := \{\Omega' \cap F : F \in \mathscr{F}\}.
$$

It is a $\sigma$-algebra on $\Omega'$. 

---
Now we can define what positive supermartingale is. Let $(\Omega, \mathscr{F}, \mathbb{P})$ be our probability space and $(\mathscr{B}_n, n \in \mathbb{N})$ an increasing sub-$\sigma$-algebrs of $\mathscr{F}$ (a.k.a a filteration).

__Definition__. An adapted sequence $(X_n, n \in \mathbb{N})$ of _positive_ random variables is called a positive supermartingale if the almost sure (a.s.) inequality 

$$
X_n \geq \mathbb{E}^{\mathscr{B}_n}(X_{n + 1})
$$

is satisfied for all $n \in \mathbb{N}$. A supermartingale is by definition a sequence of random variables (r.v.s) which "decrease in conditional mean". For a sequence positive r.v.s denoting the sequence of values of the fortune of a gambler, the supermartingale condition expresses the property that at each play the game is unfavourable to the player in conditional mean. 

We note that the inequality defining a supermartingale implies that

$$
X_m \geq \mathbb{E}^{\mathscr{B}_m}(X_p), \; \forall p > m.
$$

In fact the defintion implies that,

$$
\mathbb{E}^{\mathscr{B}_m}(X_n) \geq \mathbb{E}^{\mathscr{B}_m}\mathbb{E}^{\mathscr{B}_n}(X_{n + 1}) \stackrel{\text{tower rule}}{=} \mathbb{E}^{\mathscr{B}_m}(X_{n + 1}), 
$$

if $n \geq m$. Therefore, the sequence ($\mathbb{E}^{\mathscr{B}_m}(X_n)$) is decreasing. 

The following proposition is so cool and is so fundamental.

__Proposition__ (Maximal inequality). For every positive supermartingale, the r.v. $\sup_n X_n$ is a.s. finite on the set {$X_0 < \infty$}, and satisfies the following inequality

$$
\mathbb{P}^{\mathscr{B}_0}(\sup_n X_n \geq a) \leq \min \left(\frac{X_0}{a}, 1\right)
$$

for all constants $a > 0$.

Before going into the proof, remember that $\mathbb{P}^{\mathscr{B}}(A) = \mathbb{E}^{\mathscr{B}}(\mathbb{1}_A)$ and 

$$
\int_B\mathbb{P}^{\mathscr{B}}(A)d\mathbb{P} = \mathbb{P}(A \cap B), \quad B \in \mathscr{B}.
$$

___

First, we need an auxiliary lemma which constitutes the switching principle for supermartingales. But before that, I need to talk about the concept of the _stopping time_.

Stopping time (unsurprisingly) is a concept from gambling. A stopping rule for the player is a rule for leaving the game, based at each time on information it his/her disposal at that time. By this definition, a "dishonest" player who decides to leave the game any time already knowing certain subsequent outcomes of the game is excluded. Also, note that the stopping time could be $\infty$ meaning that the game never ends.

__Definition__. Let $\mathbb{N}^\*$ denote $\mathbb{N} \cup {\infty}$. A mapping $\nu: \Omega \to \mathbb{N}^*$ is called a stopping time if 

$$\{ \nu = n \} \in \mathscr{B}_n, \quad \forall n \in \mathbb{N}.$$

The $\sigma$-algebra $\mathscr{B}_\nu$ is associated with the stopping time as the subsets of $\Omega$ defined by

$$
\mathscr{B}_\nu = \{B: B \in \mathscr{B}_\infty, B \cap \{\nu = n\} \in \mathscr{B}_n, \forall n \in \mathbb{N}\}.
$$

Note that events in $\mathscr{B}_\nu$ are __prior__ to $\nu$.

Back to the considered lemma:

___
__Lemma__ (master). Given tow positive supermartingales $(X^{(i)}_n)(i =1, 2)$ and a stopping time $\nu$ such that $X_\nu^{(1)} \geq X_\nu^{(2)}$ on {$\nu < \infty$}, the formula

$$
X_n(\omega) = \begin{cases}
X_\nu^{(1)} & \mathrm{if}\; n < \nu(\omega)\\
X_\nu^{(2)} & \mathrm{if}\; n \geq \nu(\omega)
\end{cases} \quad (n \in \mathbb{N}),
$$

defines a new positive supermartingale.

___
_Proof_.  Indeed, the defining formula of the $X_n$ can also be written 

$$
X_n = \mathbb{1}_{\{v > n\}}X_n^{(1)} + \mathbb{1}_{\{v \leq n\}}X_n^{(2)},
$$

then it is clear that $X_n$ is $\mathscr{B}_n$ measurable. The supermartingale property of $X^{(i)}_n$ allows us to write

$$
\begin{align*}
X_n &= \mathbb{1}_{\{v > n\}}X_n^{(1)} + \mathbb{1}_{\{v \leq n\}}X_n^{(2)} \\
&\geq \mathbb{1}_{\{v > n\}} \mathbb{E}^{\mathscr{B}_n}\left[X_{n+1}^{(1)}\right] + \mathbb{1}_{\{v \leq n\}} \mathbb{E}^{\mathscr{B}_n}\left[X_{n+1}^{(2)}\right] \\
& = \mathbb{E}^{\mathscr{B}_n}\left[\mathbb{1}_{\{v > n\}} X_{n+1}^{(1)} + \mathbb{1}_{\{v \leq n\}}  X_{n+1}^{(2)}\right]. \tag{since $\nu$ is $\mathscr{B}_n$-measurable}
\end{align*}
$$

The assumption $X_\nu^{(1)} \geq X_\nu^{(2)}$ implies that $X_{n + 1}^{(1)} \geq X_{n + 1}^{(2)}$ on {$\nu = n + 1$}, and

$$
\mathbb{1}_{\{v > n\}} X_{n+1}^{(1)} - \mathbb{1}_{\{v > n + 1\}} X_{n+1}^{(1)} + \mathbb{1}_{\{v \leq n\}} X_{n+1}^{(2)} - \mathbb{1}_{\{v \leq n + 1\}}  X_{n+1}^{(2)} = X_{n+1}^{(1)} - X_{n+1}^{(2)}.
$$

Hence,

$$
\mathbb{1}_{\{v > n\}} X_{n+1}^{(1)} + \mathbb{1}_{\{v \leq n\}}  X_{n+1}^{(2)} \geq \mathbb{1}_{\{v > n + 1\}} X_{n+1}^{(1)} + \mathbb{1}_{\{v \leq n + 1\}}  X_{n+1}^{(2)} = X_{n + 1}. \quad\square
$$

Using the above lemma, let us associate with the positive supermartingale of the proposition the stopping time defined by

$$
\nu_a = \begin{cases}
\min(n:X_n > a) = 0 & \mathrm{if}\; \sup_n X_n > a\\
\infty & \mathrm{if}\; \sup_n X_n \leq a 
\end{cases}\; .
$$

Since $X_{\nu_a} > a$ on {$\nu_a < \infty$} and since the constant $a$ can be considered a supermartingale, the formula,

$$
Y_n = \begin{cases}
X_n & n < \nu_a\\
a & n \geq \nu_a
\end{cases}
$$
defines a new supermartingale by our previous lemma. 
Hence, $Y_0 \geq \mathbb{E}^{\mathscr{B}\_0}[Y_n]$.
Since $Y_0$ is equal to $X_0$ or $a$ according to the relation between $X_0$ and $a$,
and since $Y_n \geq a \mathbb{1}_{\{\nu_a \leq n\}}$, we have:

$$ a \mathbb{P}^{\mathscr{B}_0}(\nu_a \leq n) \leq \min(X_0, a).$$
Letting $n$ tend to infinity, we obtain

$$ \mathbb{P}^{\mathscr{B}_0}(\sup_n X_n > a) \leq \min\left(\frac{X_0}{a}, 1\right),$$

Since {$\nu_a < \infty$} = {$\sup_n X_n > a$}. It suffices to replace $a$ by $a\left(1 - k{^-1}\right)$ in the
inequality above and let $k$ tend to infinity to obtain the same inequality with $\geq$ instead of $>$ on the left-hand side. Let us integrate both sides over the event {$X_0 < \infty$}, which belongs to $\mathscr{B}_0$. Let $A := \{\sup_n X_n > a\}$ we find that 

$$
\begin{align*}
 \int_{\{X_0 < \infty\}}\mathbb{P}(A \mid \mathscr{B}_0) d\mathbb{P} & \leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
 \int_{\mathscr{B}_0}\mathbb{E}(\mathbb{1}_A \mid \mathscr{B}_0) d\mathbb{P} &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
\mathbb{E}[\mathbb{1}_{\mathscr{B}_0} \cdot \mathbb{E}(\mathbb{1}_A \mid \mathscr{B}_0)] &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
\mathbb{E}[\mathbb{E}(\mathbb{1}_{\mathscr{B}_0} \cdot\mathbb{1}_A \mid \mathscr{B}_0)] &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
\mathbb{E}(\mathbb{1}_{\mathscr{B}_0} \cdot\mathbb{1}_A) &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
\mathbb{P}(\mathscr{B}_0 \cap A) &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P} \Rightarrow \\
\mathbb{P}(X_0 < \infty, \sup_n X_n > a) &\leq \int_{\{X_0 < \infty\}} \min\left(\frac{X_0}{a}, 1\right)d\mathbb{P}.
\end{align*}
$$

As $a$ tends to infinity, the R.H.S. tends to zero by the dominated convergence theorem and we have 

$$
\mathbb{P}(X_0 < \infty, \sup_n X_n = \infty) = 0. \; \square
$$
___
__Box__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3))  A box $B$ in $\mathbb{R}^n$ is any set of the form

$$
B = \Pi_{i = 1}^n (a_i, b_i) := \{(x_1, \dots, x_n) \in \mathbb{R}^n: x_i \in (a_i, b_i)\: \text{for all}\: 1 \leq i \leq n\},
$$

where $b_i \geq a_i$ are real numbers. The volume of this box is defined as

$$
\mathrm{vol}(B) := \Pi_{i=1}^n (b_i - a_i).
$$

__Outer Measure__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) If $\Omega$ is a set, we define the outer measure $m^*(\Omega)$ of $\Omega$ to be quantity 

$$
m^*(\Omega) := \inf \left\{\sum_{j \in J}\mathrm{vol}(B_j): (B_j)_{j \in J}\: \mathrm{covers}\: \Omega;\: J\: \text{at most countable}\right\}.
$$

__Measurable Sets__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let $E$ be a subset of $\R^n$. We say $E$ is Lebesgue measurable, or measurable for short, iff we have the identity

$$
m^*(A) = m^*(A \cap E) + m^*(A-E)
$$
for every subset $A$ of $\R^n$.  If $E$ is measurable, we define the Lebesgue measure of $E$ to be $m(E)= m^*(E)$; if $E$ is not measurable, we leave $m(E)$ undefined. In words, $E$ being measurable means that if we use the set $E to divide up an arbitrary set $A$ into two parts, we keep the additivity property.

__Measurable Functions__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let  be a measurable subset of $\mathbb{R}^n$ and let $f: \Omega \to \R^m$ be a function. A function $f$ is measurable iff $f^{-1}(V)$ is measurable for **every** open set $V \subseteq \R^m$.  Another characterization of measurable functions is given by: Let $\Omega$ be a measurable subset of $\R^n$. A function $f: \Omega \to \R^*$ is said to be _measurable_ iff $f^{-1}((a, \infty]))$ is measurable for every real number $a$.

__Absolutely Integrable__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let $\Omega$ be a measurable subset of . A measurable function $f: \Omega \to \mathbb{R}^* := \mathbb{R} \cup \{-\infty, +\infty\}$ is said to be absolutely integrable if the integral $\int_\Omega f$ (w.r.t. the Lebesgue measure) is finite.

__Pointwise Convergence__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) The most obvious notion of convergence of functions is pointwise convergence, or _convergence at each point of the domain_. Let $\left(f^{(n)} \right)_{n = 1}^\infty$ be a sequence of functions from one metric space $(X, d_X$) to another $(Y, d_Y)$, and let $f: X \to Y$ be another function. We say that $\left(f^{(n)} \right)_{n = 1}^\infty$ converges pointwise to $f$ if we have

$$
\lim_{n \to \infty}f^{(n)}(x) = f(x), \quad \forall x \in X,
$$

i.e., 

$$
\lim_{n \to \infty}d_Y\left(f^{(n)}(x), f(x)\right) = 0.
$$

We call the function _f_ the pointwise limit of the functions $f^{(n)}$.

**Limits of measurable functions are measurable** ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let $\Omega$ be a measurable subset of $\mathbb{R}^n$, for each positive integer $n$, let $f_n: \Omega \to \mathbb{R}^*$ be a measurable function. Then, the functions $\sup_n f_n, \inf_n f_n, \limsup_{n \to \infty} f_n, \mathrm{and}\, \liminf_{n \to \infty} f_n$ are also measurable. In particular, if $f_n$ converge pointwise to another function $f: \Omega \to \mathbb{R}^*$, then $f$ is also measurable.

_Proof_. We first prove the claim about $\sup_n f_n$. Call this function $g$. We have to prove that $g^{-1}((a, \infty]))$ is measurable for every $a$. First we show that

$$
g^{-1}((a, \infty])) = \cup_{n \geq 1}f_n^{-1}((a, \infty])),
$$

then the claim follows since the countable union of measurable sets is measurable. To show the above display consdier two cases:

- $\subseteq$ 

If $x \in g^{-1}((a, \infty]))$, then $g(x) = sup_n f_n(x) > a. If every $f_n(x) \leq a$, then $\sup_n f_n(x) \leq a$ which is a contradiction. Hence there exists some $n$ with $f_n(x) > a$. Thus, $x \in f^{-1}((a, \infty]))$ for that $n$, so $x$ belongs to the union.

- $\supseteq$ 
If $x \in \cup_{n \geq 1}f_n^{-1}((a, \infty]))$, then for some $n$ we have $f_n(x) > a$. Therefore, $g(x) = \sup_n f_n(x) \geq f_n(x) > a$ and $x \in g^{-1}((a, \infty]))$.

Since both inclusions hold, the sets are equal.

One important note that we know {$g > a$} $= \cup_n$ {$f_n > a$}. But, {$g \geq a$} $\neq \cup_n$ {$f_n \geq a$}. Because if the supremum is at least $a$ does not mean that at least one of the $f_n$s will match it. We can fix this by using an approximation from below:

$$
 \{g \geq a\} = \cap_{m = 1}^\infty \cup_{n = 1}^\infty \{f_n \geq a - \frac 1m\}.
$$

Back to the main proposition, a similar argument works for $\inf_n f_n$. The claim for $\limsup$ and $\liminf$ then follow from the identities

$$
\limsup_{n \to \infty} f_n = \inf_{N \geq 1} \sup_{n \geq N} f_n, \quad \liminf_{n \to \infty} f_n = \sup_{N \geq 1} \inf_{n \geq N} f_n.
$$

__Lebesgue Monotone Convergence Theorem__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let $\Omega$ be a measurable subset of $\R^n$, and let  $f_n$ be a sequence of non-negative functions from $\Omega$ to $[0, \infty]$, which are increasing in the sense that 

$$
0 \leq f_1(x) \leq f_2(x) \leq \dots \quad \forall x \in \Omega,
$$

Note we are assuming that $f_n(x)$ is increasing with respect to $n$; this is a different notion from $f_n(x)$ increasing with respect to $x$. We have

$$
0 \leq \int_\Omega f_1 \leq \int_\Omega f_2 \leq \dots
$$

and

$$
\int_\Omega \sup_n f_n = \sup_n \int_\Omega f_n.
$$

_Proof_. For the first conclusion, we should prove that

If $0 \leq f(x) \leq g(x)$ for all $x \in \Omega$ for measurable non-negative functions $f, g$, then we have $\int_\Omega f \leq \int_\Omega g$. Let $h := g - f \geq 0$. We proceed by showing that $0 \leq \int_\Omega h \leq \infty$.  Since $f, g$ are measurable, $h$ is measurable and since $h$ is non-negative as well, its Lebesgue integral is equal to

$$
\int_\Omega h = \sup \left\{\int_\Omega s: s \text{ is simple, non-negative and dominated by } h \right\}.
$$

Fix an $s$. Since $s$ is simple, it can be written as $s = \sum_{j = 1}^{N} c_j m(E_j)$, where $c_j > 0, \cup_j E_j = \Omega$. The sum of non-negative numbers is between zero and $\infty$, hence their $\sup$ is between zero and infinity.

Now the second conclusion. Following a similar argument of above,

$$
\int_\Omega \sup_m f_m \geq \int_\Omega f_n
$$

for every $n$ including its superimum, i.e.,

$$
\int_\Omega \sup_m f_m \geq \sup_n \int_\Omega f_n.
$$

If we show that $\int_\Omega \sup_m f_m \leq \sup_n \int_\Omega f_n$, then the proof will be completed. To proceed, by the definition of $\int_\Omega \sup_m f_m$, it is sufficient to show that for **all** non-negative measurable function $s$ that it is dominated by $\sup_m f_m$ the following holds 

$$
\int_\Omega s \leq \sup_n \int_\Omega f_n.
$$
If we show that 

$$
\int_\Omega s \leq \sup_n \int_\Omega f_n + \epsilon\int_\Omega s
$$
for every $0 < \epsilon < 1$ (note that our functions are all non-negative hence the range of the epsilon); the claim follows by taking limits as $\epsilon \to 0$. $(\star)$ 

By construction,

$$
s(x) \leq \sup_n f_n (x), \quad \forall x \in \Omega.
$$

Hence, for all $x \in \Omega$, there exists an $N$ such that

$$
f_N(x) \geq (1 - \epsilon)s(x).
$$
Since $f_n$ are increasing, this implies taht $f_n(x) \geq (1 - \epsilon)s(x)$ for all $n \geq N$.

Define the sets

$$
E_n := \{x \in \Omega: f_n(x) \geq (1 - \epsilon) s(x)\}.
$$
Since $f_n$ is measurable by assumption, $s$ is measurable because it's a simple function, the function $f_n(x) - s(x) + \epsilon f(x)$ is measurable, hence is each $E_n$. We also have that $E_1 \subseteq E_2 \subseteq \dots$, and $\cup_{n = 1}^\infty E_n = \Omega$. We have that

$$
(1 - \epsilon) \int_{E_n}s = \int_{E_n} (1 - \epsilon)s \leq \int_{E_n} f_n \stackrel{(\star\star)}{\leq} \int_{\Omega} f_n, 
$$

if we show that $(\star\star)$ holds. Then by taking limits as $\epsilon \to 0$ and taking suprema, it suffices to show that 

$$
\sup_n \int_{E_n}s = \int_{\Omega}s,
$$
which would prove $(\star)$. 

Since $s$ is a simple function, $\int_\Omega s = \sum_{j = 1}^N c_j \cdot m(F_j)$, where $F_1, F_2, \dots$ are disjoint and in $\Omega$. Similarly, $\int_{E_n} s = \sum_{j = 1}^N c_j \cdot m(F_j \cap E_n)$. Since the sums are finite and the summands are positive, it suffices to show that $\sup_n m(F_j \cap E_n) = m(F_j)$. $(\star\star\star)$

So to summarize, we need to prove the following that prove $(\star)$.

1. $\int_{E_n} f_n \leq \int_{\Omega} f_n$. $(\star\star)$
2. $\sup_n m(F_j \cap E_n) = m(F_j)$. $(\star\star\star)$

The following proposition proves $(\star\star)$:

Let $\Omega$ be a measurable set, and $f: \Omega \to [0, \infty]$ be a non-negative measurable function. If $\Omega' \subseteq \Omega$ is measurable, then $\int_{\Omega'} f = \int_{\Omega} f \mathbb{1}_{\{\Omega'\}} \leq \int_\Omega f$. 

_Proof_. By the definition of the Lebesgue integral

$$
\begin{align*}
\int_\Omega f &= \sup \left\{\int_\Omega s: s \text{ is simple, non-negative and dominated by } f \right\}, \\
\int_{\Omega'} f &= \sup \left\{\int_{\Omega'} s: s \text{ is simple, non-negative and dominated by } f \right\}.
\end{align*}
$$

Fix $s$. We have

$$
\begin{align*}
\int_\Omega s &= \sum_{j = 1}^N c_j \cdot m(F_j), \\
\int_{\Omega'} s &= \sum_{j = 1}^N c_j \cdot m(F_j \cap \Omega').
\end{align*}
$$

But, $F_j \cap \Omega' \subseteq F_j$. Therefore, $m\left(F_j \cap \Omega'\right) \leq m(F_j)$. By multiplying both sides by positive constants $c_j$ and summing over $j$ we get that $\int_{\Omega'} s \leq \int_{\Omega} s$. Taking suprema from both sides competes the proof.

The following proposition helps proving $(\star\star\star)$:

If $A_1 \subseteq A_2 \subseteq \dots$ is an increasing sequence of measurable sets, then

$$
m\left(\cup_{j = 1}^\infty A_j \right) = \lim_{j \to \infty} m(A_j).
$$

_Proof_.

Define 

$$
B_1 := A_1, \quad B_j = A_j \setminus A_{j - 1}, \quad j \geq 1.
$$

Then, $A :=  \cup_{j = 1}^\infty A_j = \cup_{j = 1}^\infty B_j$, and $A_N :=  \cup_{j = 1}^N A_j = \cup_{j = 1}^N B_j$. Hence,

$$
m(A) = m\left(\cup_{j = 1}^\infty B_j\right) = \lim_{N \to \infty} m\left(\cup_{j = 1}^N B_j\right) = \lim_{N \to \infty} m(A_N).
$$

So in $(\star\star\star)$, we have

$$
\sup_n m(F_j \cap E_n) \stackrel{\text{(monotonicity)}}{=} \lim_{n \to \infty} m(F_j \cap E_n) = m\left(F_j \cap \left(\cup_{n = 1}^\infty E_n\right) \right) = m\left(F_j \cap \Omega \right) = m(F_j).
$$

__Fatou's Lemma__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) Let $\Omega$ be a measurable subset of $\mathbb{R}^n$, and let $f_1, f_2, \dots$ be a sequence of non-negative functions from $\Omega$ to $[0, \infty]$. Then

$$
\int_\Omega \liminf_{n \to \infty} f_n \leq \liminf_{n \to \infty}\int_\Omega f_n.
$$

_Proof_. 

$$
\begin{align*}
\int_\Omega \liminf_{n \to \infty} f_n & = \int_\Omega \sup_{m \geq 1} \inf_{n \geq m} f_n \\
& = \sup_{m \geq 1} \int_\Omega \inf_{n \geq m} f_n \tag{monotone convergence}. 
\end{align*}
$$
$\inf_{n \geq m} f_n \leq f_j$ for all $j \geq m$. Hence, $\int_\Omega \inf_{n \geq m} f_n \leq \int_\Omega f_j$. By taking infima w.r.t. $j$, we have

$$
\int_\Omega \inf_{n \geq m} f_n \leq \inf_{j \geq m}\int_\Omega f_j.
$$

Therefore, we obtained the following that completes the proof

$$
\int_\Omega \liminf_{n \to \infty} f_n \leq \sup_{m \geq 1}\int_\Omega \inf_{n \geq m} f_n \leq \sup_{m \geq 1}\inf_{j \geq m}\int_\Omega f_j = \liminf_{n \to \infty} \int_{\Omega} f_n.
$$

__Lebesgue Dominated Convergence Theorem__. ([Analysis II](https://link.springer.com/book/10.1007/978-981-19-7284-3)) 
Let $\Omega$ be a measurable subset of $\mathbb{R}^n$, and let $f_1, f_2, \dots$ be a sequence of measurable functions from 
$\Omega$ to $\mathbb{R} \cup \{-\infty, +\infty\}$ which converge pointwise. Suppose also that there is an absolutely
integrable function $F: \Omega \to [0, \infty]$ such that $\|f_n(x)\| < F(x)$ for all
$x \in \Omega$ and all $n = 1, 2, 3, \dots$. Then,

$$\int_{\Omega} \lim_{n \to \infty} f_n = \lim_{n \to \infty}\int_{\Omega} f_n.$$

_Proof_. If $F$ was infinite on a set of non-zero meaure, then $F$ would not be absolutely integrable thus the set
where $F$ is infinite has zero measure. We may delete this set from (this does not affect any of the integrals) and 
thus assume without loss of generality that $F(x)$ is finite for every $x \in \Omega$, which implies the same assertion for the $f_n(x)$. 

Let $f:\Omega \to \mathbb{R} \cup \{-\infty, +\infty\}$ be the function $f(x) := \lim_{n \to \infty}f_n(x)$ which
exists by assumption. Since $f$ is the limit of measurable functions, it's measurable. Also, 
since $\|f_n(x)\| \leq F(x)$ for all $n$ and all $x \in \Omega$ , we see that each $f_n$ is absolutely integrable, and by taking limits we obtain $\| f(x)\| \leq F(x)$ for all $x \in \Omega$ , so $f$ is also absolutely integrable. Our task is to show that $\lim_{n \to \infty}\int_{\Omega} f_n = \int_\Omega f$. The functions $F + f_n$ are non-negative and converge pointwise to $F + f$. So by Fatou’s lemma

$$
\int_\Omega F + f \leq \int_\Omega F + \liminf_{n \to \infty} \int f_n \Rightarrow \int_\Omega f \leq \liminf_{n \to \infty} \int f_n.
$$

But also, $F - f_n$ are non-negative and converge pointwise to $F + f$. So, again by Fatou's lemma

$$
\int_\Omega F - f \leq \int_\Omega F + \limsup_{n \to \infty} \int f_n \Rightarrow \int_\Omega f \geq \limsup_{n \to \infty} \int f_n.
$$

Hence, we showed that

$$ \int_\Omega f \leq \liminf_{n \to \infty} \int f_n \leq \limsup_{n \to \infty} \int f_n \leq \int_\Omega f.$$

Hence, the $\liminf$ and $\limsup$ of $\int_\Omega f_n$ are equal as we wanted.

___
_Remark_. The preceding proof is valid without any change if we change the constant $a$ with any $\sB_0$-measurable r.v. Say $A$ is such an r.v. Then, it means if $\mathbb{P}^{\sB_0}(A \leq \sup_n X_n) = 1$, then $A \leq X_0$, i.e., $X_0$ is largest $\sB_0$-measurable r.v. dominated by $\sup_n X_n$. More generally, for any $\sB_p$-measurable r.v. $A$ that satisfies $A \leq \sup_{n \leq p} X_n$ can be seen as applying the preceding results to the supermartigale $(\sup_{n \leq p} X_n, X_{p+1}, X_{p+2}, \dots)$ adapted to the $(\sB_p, \sB_{p + 1}, \dots)$.

__Theorem__. Every positive supermartingale $(X_n)$ almost surely converges. Furthermore, the limit $X_\infty = \lim_{n \to \infty} X_n$ a.s. satisfies the following inequality 

$$\Ev{X_\infty}{\sB_n} \leq X_n, \quad n \ \in \mathbb{N}.$$

_Proof_. 

First we'll discuss what it means that a sequence of real numbers converge. 

Given a sequence of real number $(x_n)$ in $\R^*$ and pair of real number $a, b$ where $a<b$, let us define $\nu_k (k \geq 1)$ as some special moments in time, where

$$
\begin{align*}
\nu_1 &= \min\{n: n \geq 1, x_n \leq a\} \\
\nu_2 &= \min\{n: n \geq \nu_1, x_n \geq b\} \\
\nu_3 &= \min\{n: n \geq \nu_2, x_n \leq a\} \\
\nu_4 &= \min\{n: n \geq \nu_3, x_n \geq b\} \\
& \vdots .
\end{align*}
$$

If some $\nu_k$ is not defined, we put it equal to $\infty$ and all subsequent indices. Let $\beta_{a, b}$ be the largest value of $p$ where $\nu_{2p}$ is finite. If all $\nu_k$ are finite, then $\beta_{a, b} = \infty$. $\beta_{a, b}$ denote the number of upcrossings of the sequence $(x_n$) on $[a, b]$. We can see that

$$
\liminf_{n \to \infty} x_n < a < b < \limsup_{n \to \infty} x_n \Rightarrow \beta_{a, b} = \infty \Rightarrow \liminf_{n \to \infty} x_n \leq a < b \leq \limsup_{n \to \infty} x_n.
$$

Therefore, we can deduce that a sequence in $\R^*$ is convergent iff $\beta_{a, b} <\infty$ for every $a < b \in R$. Now, let's considering the case of the sequence of r.v.s $(X_n)$. Note that the r.v.s $\nu_k(\omega$) are $\sB_n$-measurable. This is due to

$$
\{\nu_{2p} = n\} = \cup_{m < n} \{\nu_{2p - 1} = m\: \mathrm{and}\: X_{m + 1} < b, \dots, X_{n - 1} < 1, X_n \geq b\},
$$
and a similar argument for the odd indices. The event {$\beta_{a, b} \geq p$} $=$ {$\nu_{2p} < \infty$} shows that $\beta_{a, b}$ is also an r.v. Hence, the convergence criterion is 

$$
\{X_n \to \cdot\} = \cap_{a < b \in \R} \{\beta_{a, b} < \infty\} \stackrel{\Q\, \text{is dense}}{=} \cap_{a < b \in \Q} \{\beta_{a, b} < \infty\}.
$$

___
__Rationals are dense.__ ([Analysis 1](https://link.springer.com/book/10.1007/978-981-19-7261-4)) If $x$ and $y$ are two rationals that $x < y$, then there exists a third rational $z$ such that $x < z < y$.

_Proof_. 

Set $z := \frac{x + y}{2}$. Since $x <y$, then $\frac{x}{2} < \frac{y}{2}$. If we add $\frac{y}{2}$ to the both sides, we get $z < y$. A symmetrical argument shows $x < z$, hence $x < z < y$. $\square$ 

__A homeomorphism__. ([Topology](https://www.amazon.ca/Topology-2nd-James-Munkres/dp/0131816292)) Let $X$ and $Y$ be
topological spaces (like open intervals); let $f: X \to Y$ be a bijection. 
If both the function $f$ and the inverse function $f^{-1}: Y \to X$ are continuous, 
then $f$ is called homeomorphism. You may have studied in modern algebra the notion of an isomorphism between
algebraic objects such as groups or rings. An isomorphism is a bijective 
correspondence that preserves the algebraic structure involved. The analogous concept in topology i s that of
homeomorphism; i t is a bijective correspondence that preserves the topological structure involved.

___

Hence, to prove our proposition, we need to prove $\beta_{a, b} < \infty$ a.s. for every $0 < a < b \in \R$. We only considered positive numbers since $(X_n)$ is positive and we have a homeomorphism between $\R^*$ and $[0, \infty]$ using $f(x) = x, x \geq 0$.

To this end, we need the Dubin's inequalities.

___
__Dubin's inequalities__.  For every positive super martingale $(X_n)$ the upcrossing numbers are r.v.s satisfying

$$
\Pr{\beta_{a, b} \geq k}{\sB_0} \leq \left(\frac{a}{b}\right)^k \min\left(\frac{X_0}{a}, 1\right),
$$

for every integer $k \geq 1$ and real numbers $a < b$. Therefore, r.v.s $\beta_{a, b}$ are a.s. finite.

_Proof_. First note that the set of stopping times {$\omega: \nu_k(\omega) =n$} belong to $\sB_n$ so $\nu_k$ are $\sB_n$-measurable. Now extending the master lemma, we can define the following supermartingale:

$$
\begin{align*}
Y_n & =1 \quad \mathrm{if}\: 0 \leq n < \nu_1,
\\
&= \frac{X_n}{a} \quad \mathrm{if}\: \nu_1 \leq n < \nu_2,
\\
&= \frac{b}{a} \cdot 1 \quad \mathrm{if}\: \nu_2 \leq n < \nu_3, \\
& \vdots
\\
& = \left(\frac{b}{a}\right)^{k -1} \cdot \frac{X_n}{a} \quad \mathrm{if}\: \nu_{2k - 1} \leq n < \nu_{2k},
\\
& = \left(\frac{b}{a}\right)^{k} \quad \mathrm{if}\: n \geq \nu_{2k}.
\end{align*}
$$

In fact we have that:

$$
\begin{align*}
1 &\geq \frac{X_{\nu_1}}{a},
\\
\frac{X_{\nu_2}}{a} &\geq \frac{b}{a},
\\
&\vdots
\\
\left(\frac{b}{a}\right)^{k - 1} \cdot \frac{X_{\nu_{2k}}}{a} &\geq \left(\frac{b}{a}\right)^k.
\end{align*}
$$

By construction we have that $Y_0 = \min\left(1, \frac{X_0}{a}\right)$.  Since {$Y_n$} is a supermartingale we have that $Y_0 \geq \Ev{Y_n}{\sB_0}$. Also on the set {$n \geq \nu_{2k}$}, $Y_n \geq \left(\frac{b}{a}\right)^{k}$, or equivalently $Y_n \geq \left(\frac{b}{a}\right)^{k} \cdot \I${$n \geq \nu_{2k}$}. Using all these facts, we have:

$$
\begin{align*}
\left(\frac{b}{a}\right)^k \Pr{\nu_{2k} \leq n}{\sB_0} \leq \min\left(1, \frac{X_0}{a}\right).
\end{align*}
$$

All that remains is to let $n \to \infty$ and noting that {$\nu_{2k} < \infty$} $=$ {$\beta_{a,b} \geq k$}. $\square$
___

The final part of the proof involves showing that the limit $X_\infty = \lim_{n \to \infty} X_n$ a.s. satisfies the following inequality 

$$\Ev{X_\infty}{\sB_n} \leq X_n, \quad n \ \in \mathbb{N}.$$
The inequality

$$
\Ev{\inf_{m \geq n} X_m}{\sB_p} \leq \Ev{X_n}{\sB_p} \leq X_p
$$

is valid if $n > p$. Let $n$ tend to infinity, then

$$
\Ev{X_\infty}{\sB_p} = \lim_{n \to \infty} \Ev{\inf_{m \geq n} X_m}{\sB_p} \leq X_p, \quad p \in \mathbb{N}.
$$

where we used the Lebesgue dominated convergence theorem to exchange the expectation and the limit because $\left(\inf_{m \geq n} X_m, \, n \in \mathbb{N}\right)$ is an increasing bounded convergent sequence in $n$. The proof is completed.

We had already showed that $\sup_n X_n < \infty$ on {$X_0 < \infty$}, which implies that $X_\infty < \infty$ on {$X_0 < \infty$}; applying this result to the supermartingale $(X_n, n \geq p)$ adapted to the sequence $(\sB_n, n \geq p)$,  we find that $X_\infty$ a.s. on {$X_p < \infty$} for all $p \in \mathbb{N}$ which is another way of completing the last part of our proof.

_The end! :)_

<iframe src="https://giphy.com/embed/3o7TKEP6YngkCKFofC" width="480" height="365" style="" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/tvlandclassic-andy-griffith-the-show-3o7TKEP6YngkCKFofC">via GIPHY</a></p>
