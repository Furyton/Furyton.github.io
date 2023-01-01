---
title: Understanding Machine Learning 04
author: Shiguang Wu
date: 2022-03-16
categories: [understanding machine learning, note]
---

## No-Free-Lunch Theorem

What is a *universal learner*? It means it doesn't need any prior knowledge. To be more specific, $\mathcal{H}$ that contains all possible functions is PAC-learnable. ? that is certainly not true since this is about the NFL theorem.

Typically, prior knowledge could be assumptions of knowing $\mathcal{D}$ comes from some family of distribution or assuming $\exists h\in\mathcal{H}$ that $L_D(h)$ is small enough.

### NFL theorem

we consider binary classification problem and 0-1 loss over $\mathcal{X}$

$m$ be the tr set size smaller than $\frac{|\mathcal{X}|}{2}$, A be any learner

there is a D that

- there exists $f$, with zero error on $\mathcal{D}$

- with a probability of at least 1/7 over the choice of $S\sim \mathcal{D}^m$ we have the error of $A(S)\ge 1/8$

Let's prove this famous theorem!!!

---

let C be a subset of $\mathcal{X}$ with size 2m as the finite 'domain' we will consider

the intuition is that since the learner can only see at most half of the domain set, we can then make the other part of distribution anything we want to defeat A

we need to find the D and f

there could be like $2^{2m}$ possible f's; we can prove that

$$
\max_{D,f}\mathbb{E}_{S\sim D^m}[L_{D,f}(A(S))]\ge 1/4
$$

which means there exists some bad f that will make A fail

note D and f should be matched, so we construct $D_i$ as follows for each $f_i$, $i=1\dots T$ where $T=2^{2m}$

$$
D_i(x,y) = \begin{cases}
            1/|C| &\text{if } f_i(x)=y \\
            0 &\text{otherwise}
           \end{cases}
$$

since $C$ is finite, we can enumerate all S

denote as $S_i$, $i=1\dots M$ where $M=(2m)^m$, note instances in S can be duplicated

$$
\begin{aligned}
   &\max_i\mathbb{E}_{S\sim D_i}[L_{D_i}(A(S))]\\
   &=\frac{1}{M}\max_i\sum_j^ML_{D_i}(A(S_j^i))\\
   &\ge\frac{1}{MT}\sum_i^T\sum_j^ML_{D_i}(A(S_j^i))\\
   &=\frac{1}{TM}\sum_j^M\sum_i^TL_{D_i}(A(S_j^i))\\
   &\ge\frac{1}{T}\min_j\sum_{i}^{T}L_{D_i}(A(S_j^i))
\end{aligned}
$$

here the $i$ in $S_j^i$ means the *label* is assigned by $D_i$, hence $f_i$

We now convert the problem of finding the $f$ into $S_j^i$ that has a significant error averaged on all functions. Intuitively, since we consider **all** functions, they must have some disagreement on the samples

for $S_j^i$, we let $P=\{x_i\in C|x_i\notin S_j^i\}$ and $p=|P|$, where $p\ge m$

$$
\begin{aligned}
    &L_{D_i}(A(S_j^i))\\
    =&\frac{1}{2m}\sum_k^{2m}\mathbf{1}[f_i(x_k)\neq A(S_j^i)(x_k)]\\
    \ge&\frac{1}{2p}\sum_{x\in P}\mathbf{1}[f_i(x)\neq A(S_j^i)(x)]\\
\end{aligned}
$$

then

$$
\begin{aligned}
    &\frac{1}{T}\sum_{i}^{T}L_{D_i}(A(S_j^i))\\
    \ge&\frac{1}{T}\sum_i^T\frac{1}{2p}\sum_{x\in P}\mathbf{1}[f_i(x)\neq A(S_j^i)(x)]\\
    =&\frac{1}{2}*\frac{1}{p}\sum_{x\in P}\frac{1}{T}\sum_{i}^{T}\mathbf{1}[f_i(x)\neq A(S_j^i)(x)]\\
\end{aligned}
$$

there are details here, note we have removed out the instances that are in $S_j$ (no $i$ here), that is because $\mathbf{1}[f_i(x)\neq A(S_j^i)(x)]$ will always be zero if $x\in S_j$ and we can certainly not consider them

second, since we take $f$ from func space that contains all possible funcs, so they can be separated into pairs that can cancellate each other

there must always exist a pair of f's ($f_a$ and $f_b$) that only differ on one instance $x$ which is not in $S_j$ ($x\in P$)

s.t.

$\mathbf{1}[f_a(x)\neq A(S_j^a)(x)]+\mathbf{1}[f_b(x)\neq A(S_j^b)(x)]=1$

then the ave above actually is $\frac{1}{4}$

now we have proved that

$$
\max_{D,f}\mathbb{E}_{S\sim D^m}[L_{D,f}(A(S))]\ge 1/4
$$

use some prob inequality can give us the conclusion that $\exists D$
$$
\mathbb{P}_{S\sim D^m}[L_D(A(S))\ge1/8]\ge1/7
$$

---

using the def, $H$ **that contains every possible h is not PAC learnable**

## bias-complexity trade-off

Usually, we see a bias-variance trade-off. I think there may be some connection, but I haven't seen it

the idea is we divide the true risk as below

$$
\mathcal{L}_D(h)=\epsilon_{app}+\epsilon_{est}
$$

where $\epsilon_{app}=\min_{h'\in H}L_D(h')$ and $\epsilon_{est}=\mathcal{L}_D(h)-\epsilon_{app}$

- $\epsilon_{app}$, approximation error, it measures how well your hypothesis space is

- $\epsilon_{est}$, estimation error, it measures how well your learner can estimate the best h ($\in H$) using ERM

approx error has nothing to do with how you train with the dataset. if your hypothesis space is well enough (or large enough), it will be small

est error is on the opposite. It really depends on the learner, sample size so on. It's similar to the $\epsilon$ in the definition of agnostic PAC learnability. If your h space is too large (too complicated), then it will need more samples to decrease the est error.

so, large $|H|$ will reduce approx error, but it may be hard to have a small est error (overfitting), with a high probability a tr set will cause a bad generalization ability

on the contrary, small $|H|$ indeed will give us a small est error but will cause a large approximate error (underfitting)
