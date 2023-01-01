---
title: Understanding Machine Learning 05
author: Shiguang Wu
date: 2022-03-21
categories: [understanding machine learning, note]
---

## VCdim

here we go to one of the famous theories, the VC dimension

before going a little deeper, we will have a look at the motivation

### motivation

first, finite is not a sufficient and necessary condition of PAC learnability. [exercise 3.3](./understanding-machine-learning-02.html) is a simple example. Moreover, in the [last post](./understanding-machine-learning-04.html) (No-Free-Lunch theorem), we have seen H that contains all possible functions is not PAC learnable. When we rethink the proof, we may notice that the construction of set $C$ is the key point. Since we are considering **all** possible functions, the error of different f's can cancel, resulting in a large error.

borrow the idea, if we can find a subset $C$ of domain $\mathcal{X}$, and if H contains all the functions when taking $C$ as the domain, then it will cause a considerable risk using the same proof

further, if such kind of $C$ is infinitely large, then $H$ is not learnable

the thoughts above give us the basic idea of how the VC dimension comes

### VC dimension

---

Def. restriction of H to C

here, $\mathcal{C}=\{c_1,c_2,\dots,c_m\}\subseteq\mathcal{X}$

and $\mathcal{H}_C=\{h(c_1),\dots,h(c_m)\}$

---

Def. Shattering

H shatters C $\iff$ $|H_C|=2^{|C|}$

---

Def. VC-dimension

the VC-dimension of H $\coloneq\max_C\{|H_C|:\text{H shatters C}\}$

---

a simple method to show VCdim=d we need to show that

1. $\exists C$ of size d that is shattered by H

2. **every** C of size d+1 is not shattered by H

---

since $2^{|VCdim(H)|}\le |H|$, we have a loose upper bound of VC dim which is $log_2(|H|)$

## fundamental theorem of PAC learning

the followings are eq

1. uniform convergence

2. PAC learnable on every ERM learner

3. agnostic PAC learnable on every ERM learner

4. finite VC dimension

---

from 4. to 1. is non-trivial. here we will go deeper into that

we need to find $m_H^U$ s.t. for any S that $|S|\ge m_H^U$ is $\frac{\epsilon}{2}$-representative (i.e. $L_D(h)-L_S(h)|\le \frac{\epsilon}{2}$)

intuitively, if $m$ is larger than $d$, where $d$ is the VC dimension, $H_C$ will only take small part of $2^C$ (in fact that is polynomial large w.r.t. $|C|$), the estimation error could be bounded by $o(m)$

### Sauer's Lemma

---

Def. Growth Function

$$\tau_H(m)=\max_{C\subset \mathcal{X}:|C|=m}|H_C|.$$

---

according to Sauer's lemma, $\tau_H(m)\le \sum_{i=0}^d\tbinom{n}{i}$, if $m\gt d+1$, we have a looser but neat form $\tau_H(m)\ge (em/d)^d$

#### proof

basic idea

we prove a stronger claim, $\forall C=\{c_1,\dots,c_m\}$

we have $\forall H,\ |H_C|\le |\{B\subseteq C: \text{H shatters B}\}|$

induction on m

when m=1, both sides are equal

when m=k+1, suppose the claim holds for $m\le k$.

to use the claim, we need to split $C$ as $\{c_1\}$ and $\{c_1,\dots, c_m\} which is denoted as $C'$

we need to find such $H'$ that can naturally convert from $C'$ to $C$ but still holds the shattering property

note:

$$
H_C=H_{C'}\oplus H'_{C'}
$$

where $H'_{C'}=\{\exists f(c_1)=1\land g(c_1)=0\land f_{C'}=g_{C'}, \text{ where }f,g\in H\}$, $\oplus$ means direct sum ($H_{C'}\cap H'_{C'}=\emptyset$)

---

if $f_{C'}$ and $g_{C'}$ are exactly the same, they represent the **same** function in $H_{C'}$, and hence only count once in $H_{C'}$ which should be treated separatedly in $H_C$.

---

so we have that

$$
\begin{aligned}
    |H_C|&=|H_{C'}|+|H'_{C'}|\\
    &\le |\{B\subseteq C':\text{H shatters B}\}|+|\{B\subseteq C':\text{H' shatters B}\}|
\end{aligned}
$$

note that $H'$ shatters B $\iff$ H shatters $\{c_1\}+B$

so RHS $\le |\{B\subseteq C:\text{H shatters B}\}|$  $\blacksquare$

### uniform convergence

we will give the upper bound without proof :(

for every $\delta$, with prob $1-\delta$ over the choice of $S\sim D^m$

$$
|L_D(h)-L_S(h)|\le \frac{4+\sqrt{log(\tau_H(2m))}}{\delta \sqrt{2m}}
$$

RHS $\in o(m)$

---

I'll add the proof when I master this math skill :(

## exercise

- 6.1. monotonicity of VC dim

- 6.2. finite X, $k\lt |\mathcal{X}|$, compute the VC dim

  - $H_{=k}^\mathcal{X}=\{h\in \{0,1\}^\mathcal{X}:|x:h(x)=1|=k\}$. VC dim=$\min\{k,|\mathcal{X}|-k\}$, otherwise you can't assign all one (zero) to these instances

  - $H_{\text{at most k}}^\mathcal{X}=\{|x:h(x)=1|\le k \text{ or }|x:h(x)=0|\le k\}$. VC dim=k.

- 6.3. parity function $h_I$ (h computes the parity of bits at $I$ ). finte H, $d\le n$, and easy to construct such $C$ that $|C|=n$

- 6.4. skip

- 6.5. The VC dim of axis-aligned rectangles in d-dim is 2d. Since we need 2d points to construct a rectangle that assigns all points as one if there are 2d+1 points, one of them must stay inside the box (or lie on the border with no points in the box). With that point assigned as zero, others assigned as one, no function will satisfy

- 6.6. VC-dimension of Boolean conjuntions $H_{con}^d$: $f(x)=x_{i_1}\land \dots \land x_{i_k}$

  1. prove $|H_{con}^d|\le 3^d + 1$. each $x_i$ has three states, not chosen, origin $x_i$, inverse $\bar{x}_i$, so should be $= 3^d$ ??

  2. prove $VCdim(H)\le d log 3$. $VDdim\le log(|H|)=dlog3$

  3. show that $H_{con}$ shatters $\{e_i:i\le d\}$, seems easy

  4. show that $VCdim(H)\le d$. if $d+1$, consider hypothesis
    $h_i(c_j) = \begin{cases}
        0 &\text{if } i=j \\
        1 &\text{otherwise}
      \end{cases}$
    .that means each $c_i$ has at least one bit that is different from others, which is a contradiction

  5. $H_{mcond}^d$ which do not contain negations, empty conjunction is considered as all positive, the vc dim of $H_{mcond}^d$ with all negative h =d. first $VCdim(H_{mcond})\le d$. consider $x_i$ is all one but zero at index i, $i\le d$

- 6.7. skip

- 6.8. show vcdim of $H=\{x\mapsto \lceil sin(\theta x) \rceil: \theta \in \mathbb{R}\}$ is infinity.

  - *hint*: if $0.x_1x_2x_3\dots$ is the binary expansion of $x\in (0,1)$, then for any natual number m, $\lceil sin(2^m\pi x) \rceil =(1-x_m)$, provided that $\exist k\ge m$ s.t. $x_k=1$.

  - with the hint, that is easy. Just construct a huge matrix with each row representing a binary expansion of $x_i$ and the columns running through all possible combinations.

- 6.9. skip

- 6.10. skip

- 6.11. VC of union. $H_1,\dots,H_r$.

- 6.12. Dudley classes. 

  1. $VCdim(POS(\mathcal{F}+g))=VCdim(POS(\mathcal{F}))$. "$\mathbb{R}ightarrow$", $(f_1+g)-(f_2+g)$.

  2. $VCdim(POS(\mathcal{F}))=\dim(\mathcal{F})$. half space, full rank, bla bla bla

  3. examples of Dudley classes

btw, Dudley class seems to be a fascinating and important topic
