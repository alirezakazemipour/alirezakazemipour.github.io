---
title: "A reminder on Student's t-distribution and p-values"  
permalink: /posts/2025/08/t-dstrbtn-p-vls/
date: 2025-09-14  
tags:
  - learning
---
  
Let's see what Sir [Larry Wasserman](https://www.stat.cmu.edu/~brian/valerie/617-2022/0%20-%20books/2004%20-%20wasserman%20-%20all%20of%20statistics.pdf) has to tell us about it.  
  
So we're in the realm of hypothesis testing [[Aaditya Ramdas](https://stat.cmu.edu/~aramdas/icml25/ramdas1.pdf) calls it stochastic proof by contraction, _I  
love this phrase_]. Suppose we divide our parameter space $\Theta$ into two disjoint sets $\Theta_0$ and $\Theta_1$.  
We wish to test  
  
$$H_0: \theta \in \Theta_0 \quad\text{versus} \quad H_1: \theta \in \Theta_1$$.  
  
We call $H_0$ the null hypothesis that we'd like to reject. Because it says nothing interesting is going on   
(hence the name null). Let $\mathbb{P}\_\theta$ be a probability distribution with support $\mathcal{X}$  
parameterized by $\theta$. Define $\mathcal{R} \subset \mathcal{X}$ called the rejection region.  
Let $X \sim \mathbb{P}$<sub>$\theta$</sub>$(\cdot)$. Then, if   
  
$$X \in \mathcal{R} \Rightarrow \text{ reject } H_0, \\  
X \notin \mathcal{R} \Rightarrow \text{ retain } H_0.$$  
  
Usually, the rejection region is of the form  
  
$$R = \{x \in \mathcal{X}: T(x) > c \},$$  
  
where $T$ is a  test statistic and $c$ is a  critical value. The problem in hypothesis testing is to find an appropriate  test statistic $T$ and an appropriate critical value $c$.   
  
P.S.: A test statistic is  a single number calculated from sample data that is used to evaluate a hypothesis in statistical analysis. It quantifies the difference between the observed data and what would be expected if the null hypothesis were true. Essentially, it helps determine how compatible your data is with a specific hypothesis.  
  
__Definition__: The power function of a test with rejection region $\mathcal{R}$ is defined by  
  
$$\beta(\theta) = \mathbb{P}_\theta(X \in \mathcal{R}).$$  
  
The size of a test is defined to be   
  
$$\sup_{\theta \in \Theta_0} \beta(\theta).$$  
  
A test is said to have level $\alpha$ if its size is less than or equal to $\alpha$.  
Basically, the level $\alpha$ specifies the __maximum__ probability of rejection the null hypothesis.  

__Definition__ (The Wald test). Consider testing:

$$H_0: \theta = \theta_0 \quad \mathrm{versus} \quad H_1: \theta = \theta_1.$$

Assume under $H_0$, $\hat{\theta}$ is asymptotically normal, i.e., as $n$ tends to infinity $\frac{\hat{\theta} -  \theta_0}{\hat{\sigma}}$ tends to $\mathcal{N}(0, 1)$ due to central limit theorem. Then, the size $\alpha$ Wald test is: reject $H_0$, when $\|W\|\geq F_W^{-1}(\frac \alpha2)$, where $W = \frac{\hat{\theta} -  \theta_0}{\hat{\sigma}}$ and $F_W$ is the law of $W$. 


The Wald test uses the central limit theorem and is only asymptotically valid. The _t-test_ is instead used where we have finite sample size, with the caveat that we assume the data is coming from a Normal distribution. A random variable $T$ has a (Student's) t-distribution with $k$ degrees of freedom with the following density function:

$$f(t_k) = \frac{\Gamma(\frac{k + 1}{2})}{\sqrt{k\pi}\Gamma(\frac{k}{2})(1 + \frac{t^2}{k})^{\frac{k+1}{2}}},$$

where $\Gamma(\alpha) = \int_0^\infty y^{\alpha - 1}e^{-y}dy$ is the gamma function. The t-distribution is equal to the Cauchy distribution $\left(f(x) = \frac{1}{\pi(1 + x^2)}\right)$ when $k=1$, and as $k$ tends to infinity, it tends to the normal distribution. 

Now, assume $X\_1, X\_2 \dots, X\_n \sim \mathcal{N}(\mu, \sigma^2)$, where $\theta = (\mu, \sigma)$ are both unknown. Suppose we want to test $\mu = \mu_0$ versus $\mu \neq \mu_0$. Let

$$T = \frac{\bar{\mu} - \mu_0}{\bar{\sigma}}.$$
The same as the Wald test, for large $n$, $T \sim \mathcal{N}(0, 1)$ under $H_0$. However, the exact distribution of $T$ under $H_0$ has the $f(t_{n -1})$ density, i.e., the t-distribution with $n - 1$ degrees of freedom. Hence if we reject when $\|T\| \geq F_T^{-1}(\frac \alpha 2)$ then we get a size $\alpha$ test.

___

To define the $p$-values, unfortunately Larry Wassermann doesn't transition smoothly and uses $\alpha$ for different things that made me confused. That said, for the following don't assume that $\alpha$ was defined above.  
  
__Definition__. Let $X^n = (X_1, X_2, \dots, X_n)$ (n-fold Cartesian product of the random variable $X$). Suppose  
that for very $\alpha \in (0, 1)$,  we have a size $\alpha$ test with rejection region $\mathcal{R}_\alpha$, Then,  
  
$$p\text{-value} = \inf\{\alpha: T(X^n) \in \mathcal{R}_\alpha\} $$  
  
That is, the $p$-value is the smallest level (__probability__) at which we can reject $H_0$ [quite the opposite of the definition above that involved $\beta$].

