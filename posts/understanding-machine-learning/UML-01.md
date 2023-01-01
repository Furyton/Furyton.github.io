---
title: Understanding Machine Learning 01
date: 2022-03-03
author: Shiguang Wu
categories: [understanding machine learning, note]
---

## formal def

- domain set. a set of instances $\mathcal{X}$

- label set. $\mathcal{Y}$

- training data. finite **sequence** $\mathcal{S}$ in $\mathcal{X}\times \mathcal{Y}$

- learner. output a rule, $h:\mathcal{X}\rightarrow\mathcal{Y}$. learning algorithm $\mathcal{A(S)}$

- data-generation model. ideally, we assume (for sim) there exists a \"correct\" labeling funciton, $f$. and $\mathcal{S}$ is generated under an unknown distrib $\mathcal{D}$, then labeling it using $f$.

- measurement. actually generalization error (risk or true error of $f$)

$$
L_{\mathcal{D},f}(h):=\mathbb{P}_{x\sim D}[h(x)\neq f(x)]:=\mathcal{D}(\{x:h(x)\neq f(x)\})
$$

where $\mathcal{D}$ desc the prob of the event of observation.

$\mathcal{D}(A):=\mathbb{P}_{x\sim D}[\pi(x)]$, where $\pi$ is the char func of whether it was observed. $A={x\in \mathcal{X}:\pi(x)=1}$

## ERM (empirical risk minimization)

use training loss to approx generalization loss.

### overfitting

we may obtain a bunch of funcs just by ERM. so we need an inductive bias to set a preference on a certain funcs.

we choose the **hypothesis space** H before seeing the data. restric our search space of the ERM, otherwise we got a trivial useless solution.

**before seeing the data** $\rightarrow$ should be based on some prior knowledge.

## H

examples of H

### finite H space

$\mathcal{H}$ will not overfit provided a sufficiently large training set.

note: the class of axix-aligned rectangles could be finite if we consider it on a computer. (discrete repr of real numbers)

$h_S\in argmin_{h\in H}L_s(h)$

since S are randomly chosen, so $h_S,L_{D,f}$ are actually **random vars**.

### a few assumptions

a few assumptions on the PAC learnability

#### **def** the realizability assumption

there exists $h^\star\in\mathcal{H}$ s.t. $L_{D,f}(h^\star)=0$

further, we have

$\rightarrow L_S(h^\star)=0 \text{ with prob 1 over the S }\rightarrow L_S(h_S)=0$

we are interested in $L_{D,f}(h_S)$

#### confidence param

we address a prob $\delta$ of getting a very nonrepresentative training set (e.g all lie in class A). and $(1-\delta)$ is the confidence of our prediction.

#### accuracy param

$\epsilon$

we call $L_{D,f}(h_S)\ge \epsilon$ as a failure of the learner, otherwise approx correct predictor.

So we\'re interested in the upper bound of prob to sample S that leads to the learner's failure.

upper bound of

$$
D^m\{S|_x:L_{D,f}(h_S)\gt \epsilon\}
$$

let $H_B$ be the set of bad hypotheses

$$
\{h\in H: L_{D,f}\gt \epsilon\}
$$

let M be the set of the misleading training set

$$
\{S|_x:\exists h\in H_B, L_S(h)=0\}
$$

where $S|_x$ is the instances of tr set

due to real.. assumption, only M will cause failure.

so only a subset of S from M will cause $h_S$ to fail.

$$
\begin{align}
&D^m\{S|_x:L_{D,f}(h_S)\gt \epsilon\} \\
\le& \sum_{h\in H_B}D^m\{S|_x:L_S(h)=0\}\\
=&\sum_{h\in H_B}\prod D\{x_i:h(x_i)=f(x_i)\}
\end{align}
$$

here, the countability of $\mathcal{H}$ is used, and I think if we can control the order of $|\mathcal{H}|$ and with more careful scaling (with more assumption or knowledge about the h\'s, like $D^m\{S|_x:L_S(h)=0\}$ can be approx related to h) then we could have the inf conclusion, though maybe not that interesting, and there are other ways on it.

and

$$
D\{x_i:h(x_i)=f(x_i)\}=1-L_{D,f}(h)\le 1-\epsilon
$$

so using a series of loose relaxation, we have

$$
D^m\{S|_x:L_{D,f}(h_S)\}\le |H_B|e^{-\epsilon m}\le |H|e^{-\epsilon m}
$$

finite is used here

LHS is $\delta$

Note sometimes m should be really large to ensure with at least $1-\delta$ confidence over the choice of S, every ERM hypothesis, $h_S$ is approx correct.

## small corollary

when hypothesis space is finite, then we can immediately have an upper bound for $m_{\mathcal{H}}$

$$
m_{\mathcal{H}}(\epsilon,\delta)\le \left \lceil \frac{log\left(|\mathcal{H}|/\delta\right)}{\epsilon} \right \rceil
$$
