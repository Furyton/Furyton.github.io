---
title: calculus review 01
author: Furyton
date: 2022-03-28
categories: [calculus, review]
---

## basic

linear transformations

measure of matrices: $\left \rVert A\right \lVert_F^2$

triangle inq in matrices:  $\rVert AB \lVert\le \rVert A\lVert\rVert B\lVert$

the neighborhood of $x$, exist an open ball init

closure  $\bar{A}$, the smallest close-set that contains A

interior  $\mathring{A}$, the largest open set that A contains

the boundary of subset, $\partial A$

---

convergence of a sequence, in terms of coordinates

limits of multivariable functions: continuity is preserved under dot product operation

continuity: the preimage of a neighborhood of $f(x)$ is also a neighborhood of x

uniform continuity: linear transformations are uniform continuity

convergence of the sum of series (vectors): absolute(*norm* in vector cases) convergence implies convergence

complex exponentials

- complex exponential series converges: $e^z=1+z+\frac{z^2}{2!}+\dots=\sum_{k=0}^\infty \frac{z^k}{k!}$, since the absolute series converges

euler formular:  $e^{it}=cost+isint$

geometric series of matrices:

- $S=I+A+A^2+\dots$ converges to $(1-A)^{-1}$ if $\lVert A \rVert \lt 1$

- the set of invertible n by n matrices is open

bounded: subset $X\subset \mathbb{R}^n$ is **bounded** if it is contained in some ball centered at the origin

compact: nonempty subset  $C\subset \mathbb{R}^n$ is compact if it is **closed** and **bounded**

## important theorem

- Bolzano-Weierstrass theorem: a compact set C contains a seq, then that seq has a convergent sub seq whose limit is in C

- a continuous function on a compact set achieves its minimum and maximum

- the mean value theorem

- continuity on a compact set is uniform continuity

- the fundamental theorem of algebra

## differentiable

### def

differentiable means $f$ can be well approximated at $x=a$ by a linear transformation

let $f:\mathcal{U}\to\mathbb{R}^m$ where $\mathcal{U}$ is a open subset of $\mathbb{R}^n$

$D_if(a)$ means the partial derivative on the i-th value. It is an **m by 1** vertical vector

we can treat it like the mapped element from $e_i$

$Df(a)$ is an **m by n** matrix where we stack $D_i$'s in a row

the precise def of the derivative is as below:

$Df(a)$ is a linear transformation from $\mathbb{R}^n$ to $\mathbb{R}^m$, s.t.

$$
\lim_{h\to 0}\frac{1}{|h|}\left( (f(a+h)-f(a)) - [Df(a)]h\right )=0
$$

### Jacobian matrix

Jacobian matrix now fits this definition and don't need for any transpose.

note the existence of Jacobian matrix does not mean differentiable, except that we let $f$ to be **continuously** differentiable. I won't present any examples of why and how. We just need to focus on the $C^1$ functions from now on :\

---

for some cases where the domain is not the subspace of Euclidiean space, we may need to use the definition of derivative instead of the Jacobian matrix.

e.g. 1

$\mathcal{S}:Mat(n,n)\to Mat(n,n)$ given by $S(A)=A^2$

$$[\mathbf{D}S(A)]H=AH+HA$$

e.g. 2

$f(A)=A^{-1}$ defined on invertible n by n matrices

$$[\mathbf{D}f(A)]H=-A^{-1}HA^{-1}$$

---

### about gradient

$$
grad\ f(a)=\vec{\nabla} f(a)=[D_1f(a),\dots, D_nf(a)]^\intercal
$$

where $f:\mathbb{R}^n\to \mathbb{R}$

From my POV, gradient is the **solution** of the fastest increasing direction at some point $a$ and it **happens** to be the (transpose of) derivative. note the transpose has its meaning, it makes the grad lying in the domain.

furthermore, from how we get the **solution**, we should have an inner-product defined on the domain space, which is not the case in the def of derivative.


### directional derivative

directional derivative of $f$ at $a$ in the derection $\vec{v}$

$$
[Df(a)]\vec{v}
$$

this is how linear transformation works.

### rules

1. constant function: 0

2. linear function: itself

3. $\mathbf{f}=(f_1,\dots,f_m)^\intercal$ then 

   $$[Df(a)]\vec{v}=\left[[Df_1(a)]\vec{v},\cdots,[Df_m(a)]\vec{v}\right]^\intercal$$

4. $[D(f+g)(a)]=[Df(a)]+[Dg(a)]$

5. $f:U\to \mathbb{R}$ and $\mathbf{g}:U\to \mathbb{R}^m$

   $$
   [D(f\mathbf{g})]\vec{v}=f(a) [D\mathbf{g}(a)]\vec{v}+([Df(a)]\vec{v})\mathbf{g}(a)
   $$

6. $$\left[D\left(\frac{\mathbf{g}}{f}\right)(a)\right]\vec{v}=\frac{[D\mathbf{g}(a)]\vec{v}}{f(a)}-\frac{([Df(a)]\vec{v})g(a)}{f^2(a)}$$

7. dot product $\mathbf{f}:U\to \mathbb{R}^m$

   $$
   [D(\mathbf{f}\cdot\mathbf{g})]\vec{v}=\mathbf{f}(a)\cdot[D\mathbf{g}(a)]\vec{v}+([D\mathbf{f}(a)]\vec{v})\cdot\mathbf{g}(a)
   $$

8. chain rule

   $$[D(f\circ g)(a)]=[Df(g(a))][Dg(a)]$$

---

ref

- [*Vector Calculus, Linear Algebra, and Differential Forms: A Unified Approach*](https://pi.math.cornell.edu/~hubbard/vectorcalculus.html)
