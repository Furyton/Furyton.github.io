---
title: Understanding Machine Learning 03
author: Shiguang Wu
date: 2022-03-11
categories: [understanding machine learning, note]
---

## Uniform Convergence

Before, the way of choosing $m_H$ and the effect of $\delta$ was related to the learner. However, we can borrow the concept of uniform convergence from analysis to make it independent of what learner you use.

here, we can treat $L_S(h)$ as $\sum_i^nf_i(x)$ since they are both intermediate vals during the convergence, and $L_D(h)$ to be the final end

### def $\epsilon$-representative sample

$S$ is $\epsilon$-representation sample (w.r.t domain, $H$, $l$ and $D$) if

$$
\forall h\in\mathcal{H},\, |L_S(h)-L_D(h)|\le \epsilon
$$

lemma

if S is $\frac{\epsilon}{2}$-representative, then $\forall h_S\in\argmin_{h\in\mathcal{H}}L_S(h)$

$$
L_D(h_S)\le \min_{h\in\mathcal{H}}L_D(h)+\epsilon
$$

through this lemma, we can immediately have

$S$ is $\frac{\epsilon}{2}$-representative with prob $1-\delta$ $\implies$ Agnostic PAC learnability

### def uniform convergence

H has the uniform convergence $\coloneqq$ exists a func $m_H^{UC}(\epsilon, \delta)$, for every $D$, if $|S|\gt m_H^{UC}$ then with $1-\delta$ prob, it is $\epsilon$-representative

It seems stronger than the original agnostic PAC, just like the rel between uniform conv and normal conv in analysis. normal conv only cares the situation in a certain area (here the decider generated by the learner), while uni conv holds on the whole area (all $h\in\mathcal{H}$)

#### corallary

$m_H\le m_H^{UC}$ if $H$ has the uni conv property

## situation of the finite H class

need to find $m$, so that

$$
D^m(\{S:\forall h\in \mathcal{H},|L_S(h)-L_D(h)|\le\epsilon\})\ge 1-\delta
$$

and we may convert it into a more familiar form (convenient for using inequalities)

$$
D^m(\{S:\exists h\in \mathcal{H},|L_S(h)-L_D(h)|\gt\epsilon\})\lt \delta
$$

using union bound and Hodeffing inequalities (note that $L_D(h)=\mathbb{E}_{S\sim D^m}(L_S(h))$), we have

$$
LHS\le\sum_{h\in\mathcal{H}}2exp(-2m\epsilon^2)
$$

as a corollary, we have the upper bound for finite hypothesis class which is agnostic PAC learnable

$$
m_H^{UC}(\epsilon,\delta)\le\left\lceil\frac{log(2|\mathcal{H}|/\delta)}{2\epsilon^2}\right\rceil
$$


::: {.callout-tip}
## summary

if uni conv holds, then in most cases, the empirical risks of h in H will faithfully represent their true risks
:::

---

exercises

- 4.1

1. $\forall \epsilon,\delta\gt 0,\exists m(\epsilon,\delta)\,s.t.$
$$
\forall m\ge m(\epsilon,\delta),\,\mathcal{P}_{S\sim D^m}[L_D(A(S))\gt\epsilon]\lt\delta$$

1. $\lim_{m\to \infty}\mathbb{E}_{S\sim D^m}[L_D(A(S))]=0$

1 $\iff$ 2
