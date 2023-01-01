---
title: Understanding Machine Learning 06
author: furyton
date: 2022-03-30
categories: [understanding machine learning, note]
---

we will summarize different kinds of defs of learnability first

## learnability

### PAC learnability

H is PAC learnable if the realizability assumption holds and

$\forall \epsilon,\delta\gt 0$, there exists a learner A and $m_H(\epsilon,\delta)$ s.t. if $m\ge m_H$

for any D, with probability greater than $1-\delta$ over the choice of $S\sim D^m$

$$
L_D(A(S))\le \epsilon
$$

### agnostic PAC learnability

H is agnostic PAC learnable if

$\forall \epsilon,\delta\gt 0$, there exists a learner A and $m_H(\epsilon,\delta)$ s.t. if $m\ge m_H$

for any D, with probability greater than $1-\delta$ over the choice of $S\sim D^m$

$$
L_D(A(S))\le \min_{h\in H}\{L_D(h)\}+\epsilon
$$

### uniform convergence property

H enjoys the uniform convergence property if

$\forall \epsilon,\delta \gt 0$, there exists $m_H^{UC}(\epsilon, \delta)$ s.t. if $m\ge m_H^{UC}$

for any D, with probability greater than $1-\delta$ over the choice of $S\sim D^m$, $\forall h\in H$

$$
|L_S(h) - L_D(h)|\le\epsilon
$$

these defination of learnibility are equal according to the fundamental theory

---

here we have another def which has different power with the above

## nonuniform learnability

we allow $m_H$ to be non-uniform over h

H is nonuniform learnable if

$\forall \epsilon,\delta\gt 0$ there exists a learner A and $m_H^{NU}(\epsilon,\delta,h)$, s.t. for all $h\in H$, if $m\ge m_H^{NU}(\epsilon,\delta,h)$

for any D, with probability greater than $1-\delta$ over the choice of $S\sim D^m$

$$
L_D(A(S))\le L_D(h)+\epsilon
$$

note when m is decided, and it is that makes non-uniform learnability weaker than PAC

### property

H is nonuniform learnable iff H can be expressed as a **union** of countable $H_i$ with uniform convergence property

we can easily construct H that is nonuniform learnable but not PAC learnable which means PAC learnability is stronger

### generic learner

ERM is a fittable learer for PAC learnability

SRM (structural risk minimization) is a fittable learner for NU learnability

SRM requires us to provide more prior knowledge on the priority (weights) of $H_i$'s.

def $\epsilon_n(m,\delta)=\min\{\epsilon\in (0,1):m_{H_n}^{UC}(\epsilon,\delta)\le m\}$ which means the minimum est error we can get with m samples

so given m samples

$$
\forall h\in H_n,|L_D(h)-L_S(h)|\le \epsilon_n(m,\delta)
$$

::: {.callout-note}
note m here can be varied (I'm not so sure about this) or large enough (s.t. $\forall n,\epsilon_n(m,\delta)\le \epsilon$) during training
:::

when we put this on a larger range ($H_n\rightarrow H$) directly, we can't garantee we satisfy the $\delta$ constraint, since each one has relatively low confidence $1-\delta$. So we have to split the confidence to each $H_i$, that is providing weights $\sum_{n\in\mathbb{N}}w(n)\le 1$ on each $\delta$ (use union bound inequality to merge it back)

to put it formally

given $H=\cup_{n\in\mathbb{N}}H_n$ and $\sum_{n\in\mathbb{N}}w(n)\le 1$, where $H_i$ satisfy UC property, then

with probatility of at least $1-\delta$ over the choice $S\sim D^m$

for any $n\in\mathbb{N}$ and $h\in H_n$

$$
|L_D(h)-L_S(h)|\le \epsilon_n(m,w(n)\cdot \delta)
$$

which implies for $\forall h\in H$

$$
L_D(h)\le L_S(h)+\min_{n:h\in H_n}\epsilon_n(m,w(n)\cdot\delta)
$$

if we make it simpler (but looser), let $n(h)=min\{n:h\in H_n\}$

$$
L_D(h)\le L_S(h)+\epsilon_n(m,w(n(h))\cdot\delta)
$$

SO, SRM is to minimizing the RHS

we can proof that $L_D(A(S))\le L_D(h)+\epsilon$ with p at least $1-\delta$ over the choice of S (if $m\ge m_{H_{n(h)}}^{UC}(\epsilon/2,w(n(h))\cdot \delta)$)

---

in fact, any converged sumations should be ok for w, like $w(n)=\frac{6}{n^2\pi^2}$

intuitively, $H_n$ with larger $w(n)$ will need less samples since it is required for less confidence, we actually focus on **some** hypothesis classes instead of treat all $H_n$ evenly.

second, if $h_1$ and $h_2$ has the same empirical risk, we will prefer the one with higher weight if using SRM

seems familiar? sounds like the principle Occam's razor

### description length

we now consider a countable $H$. it can be expressed as a union of singleton class $H_i=\{h_i\}$ and for each $H_i$, $m_{H_i}^{UC}(\epsilon,\delta)= \left\lceil\frac{log(2/\delta)}{2\epsilon^2}\right\rceil$

then $e_n(m,w(n(h))\cdot \delta)= \sqrt{\frac{-logw(n(h)) +log(2/\delta)}{2m}}$

def description language $\{0,1\}^\star$

we assign each $H_i$ with a description $d(h)$ and denote $|h|=|d(h)|$

::: {.callout-note}
if S is a **prefix-free** set of strings, then

$$
\sum_{\sigma\in S}\frac{1}{2^{|\sigma|}}\le 1
$$
:::

so $w(h)=\frac{1}{2^{|h|}}$ is a legal weight function

and the hypothesis with smaller description length is preferable if they have the same risk

that's the principle of Occam's razor

## consistency

::: {.callout-note}
if we let $m_H$ further be dependent on the distribution, we have the def of consistency
:::

a learner A is consistency with respect to H and P where P is the set of possible distribution D's, if

$\forall \epsilon,\delta\gt 0$ there exists a learner A and $m_H^{NU}(\epsilon,\delta,h,D)$, s.t. for all $h\in H$ and $D\in P$, if $m\ge m_H^{NU}(\epsilon,\delta,h,D)$

with probability greater than $1-\delta$ over the choice of $S\sim D^m$

$$
L_D(A(S))\le L_D(h)+\epsilon
$$

::: {.callout-note}
if P is the set of all distributions, then A is universally consistent with respect to H
:::

this def of learnability is even weaker than NU

the algorithm `Memorize` is universally consistent which will be overfit in the context of PAC learnability!!! (for every countable domain and finite label set w.r.t. zero-one loss)

```python
def Memorize(x)
    return y if (x,y) in S else 0 # any default value 
```

---

why different ability?

note when we determine the $m$

---

## exercise

- 7.5. H that contains all functions is not nonuniform learnable

  - conclusion: $H=\cup_{n\in\mathbb{N}}H_n$, if H shatters an infinite set, the some $H_n$ has infinite VC dim
