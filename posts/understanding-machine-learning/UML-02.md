---
title: Understanding Machine Learning 02
author: Shiguang Wu
date: 2022-03-08
categories: [understanding machine learning, note]
---

## def PAC

def *PAC learnability*

hypothesis H is PAC learnable if realizability assumption holds, and exists $m_H(\epsilon,\delta)\rightarrow\mathbb{N}$

where with #i.i.d. sample $\ge m_H$, we always have a probability approx correct h just using ERM.

## removing realizability assumption

the realizability assumption is too strong and unrealistic where we assume the existence of a perfect hypothesis from $\mathcal{H}$ that can reveal ground truth $f$ with $Pr=1$

so, change $f(x)$ into a joint distrib $D(x,y)$ as most researchers would do

def generalized risk as $L_D(h)\coloneqq \mathcal{P}_{(x,y)\sim D}[h(x)\neq y]\coloneqq D(\{(x,y):h(x)\neq y\})$, just changed $D$ into a joint distrib

empirical risk is the same

still, when take $D$ to be a uniform distrib on $S$ they are eq

ideally, func "$f_D(x)=1\text{ if }Pr[y=1|x]\ge 0.5\text{ and 0 otherwise}$" is the optimal sol to the gen risk min problem when $\mathcal{Y}=\{0,1\}$, w.r.t 0-1 loss. Bayes optimal sol.

---

*note*: my stupid short proof about the above

we need to proof

$$
[f(x)\neq 0]Pr(y=0|x)+[f(x)\neq 1]Pr(y=1|x)\\
\le [g(x)\neq 0]Pr(y=0|x)\dots
$$

just consider cond on $Pr(y=0|x)\gt 0.5$, and it becomes

$$
LHS=Pr(y=1|x)\\
=\{[g(x)\neq 0]+[g(x)\neq 1]\}Pr(y=1|x)\\
\le RHS
$$

very ugly and not clever proof  :/ $\blacksquare$

---

here, we still have **the opt function** $f$, which minimizes the gen risk but does not minimize it to zero

## def Agnostic(不可知论的) PAC learnability

before: $L_{D,f}(h)\le \epsilon$, now: $L_{D}(h)\le min_{g\in\mathcal{H}}L_{D}(g)+\epsilon$

so $f$ above will not be used, but the joint distrib $D$ instead

and we see that if the realizability assumption holds, it's the same as the original PAC learnability

agnostic just means we can't obtain an h with arbitrary small gen risk

relative best instead of abs best

## other loss functions

we can actually use other measures in place of the 0-1 loss, especially in regression problems

naturally, we can extend the loss function into a more formal definition, $l:\mathcal{H}\times\mathcal{Z}\rightarrow \mathbb{R}_+$, where $\mathcal{Z}$ is the set of instances, in prediction tasks, it could be $\mathcal{X}\times\mathcal{Y}$

### Agnostic PAC learnability for General Loss Functions

$L_D(h)\le\min_{h'\in\mathcal{H}}L_D(h')+\epsilon$

where $L_D(h)=\mathcal{E}_{z\sim D}[l(h,z)]$

*note*: $l(h,\dot)$ is a rand var, it should be measurable..

## some exercises

- 3.1. about monotonicity of $m_\mathcal{H}$ on $\epsilon$ and $\delta$ respectively: seems trivial

- 3.2. about $\mathcal{H}_{singleton}$: first, you need to come up with a simple learning alg. if no pos samples appear, just choose $h^-$. this is enough to prove the PAC learnability, as m is large enough, the cost will be small enough

- 3.3. about $\mathcal{H}$ consist of disks. similar to the rect situation from this chap, but simpler, 'cause it's convenient to construct how the bad samples look like

- 3.4. about $\mathcal{H}$ consist of boolean conjunctions. similar to 3.2. it's easy to determine f if $S$ contains a positive sample. Otherwise, we just return an all-negative hypothesis

- 3.5. about samples from i. but not i.d. from the derivation of the finite hypo corollary, the key is to deal with $\prod_i \mathcal{P}_{x\in D_i}(x_i|h(x_i)=f(x_i))$, where the GA mean ineq could be used to make it as $(1-\epsilon)^m$

- 3.6. ??, since you added the realizability assumption, it's naive to have the PAC learnability, maybe?

- 3.7. about ideally opt sol of binary classification (w.r.t 0-1 loss). see [this](#removing-realizability-assumption)

- 3.8.1. same question as above, but use abs loss, and consider probabilistic hypothesis (outputs a distribution instead of the deterministic answer), method from [here](#removing-realizability-assumption) seems enough?

- 3.8.2. prove the existence of a learning algo that is better than any others provided $D$. ??? an algo from God that can directly output $f$ from the previous question -_-

- 3.8.3. for every learning algo A from $D$, there always exists a B and $D'$ that A is not better than B on $D$. shit, this is also naive from 3.8.2

- 3.9. about a variant of PAC learnability, which uses a two-oracle model that allows learner access to $D^+$ and $D^-$ on its preference. the learner can actually change the popularity of the observances, e.g. a learner can take samples from both pos and neg with equal probability (I don't understand here so much, does )

  - 1. proof PAC in the standard model $\implies$ PAC in the two-oracle model. we need to construct a learner from the one in the standard model. suppose the learner puts equal weights on pos and neg samples. It learns a population with equal pos and neg using a standard model. denote the new distribution as $D'$, so

  $$\begin{aligned}
  L_{D'}(h)&=P_{D'}[h\neq f,f=0]+P_{D'}[h\neq f, f=1]\\
  &=P_{D'}[f=0]P_D[h\neq f|f=0]+P_{D'}[f=1]P_D[h\neq f|f=1]\\
  &=\frac{1}{2} L_{D^+}(h)+\frac{1}{2}L_{D^-}(h)
  \end{aligned}
  $$

  - 2. proof PAC in the two-oracle model $\implies$ PAC in the standard model if $h^+,h^-\in\mathcal{H}$. if we have enough samples ($m_H$), it will contain $m^+$ pos, and $m^-$ neg with high prob, and even it fails to have enough pos (or neg), we can just return $h^-$ (($h^+$) and won't cost much risk.
