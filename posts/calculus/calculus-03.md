---
title: calculus review 03
author: Furyton
date: 2022-09-11
categories: [calculus, review]
---

## manifolds

New stuff (to me)

### definition

::: {.callout-note}
**manifold**: a subset $M\subset \mathbb{R}^n$ is a smooth $k$-dimensional manifold

if locally it is the **graph** of a $C^1$ mapping $f$ expressing $n-k$ variables as functions of the other $k$ variables
:::

The definition seems highly related to the implicit function theorem

Therefore, we can quickly catch the spirit below

let $\mathbf{F}:U\rightarrow \mathbb{R}^{n-k}$ be a $C^1$ mapping, where $U\subset\mathbb{R}^n$.

::: {.callout-note}
- Some subset $M$ of the domain $U$ is a $k$-dimensional manifold if $\mathbf{F}(z)=0$ and $[D\mathbf{F}(z)]$ is onto for every $z\in M$.

- (converse) if $M$ is a smooth $k$-dimensional manifold, then for every $z\in M$, there exists $\mathbf{F}$ s.t., $[D\mathbf{F}(z)]$ is onto and $\mathbf{F}(y)=0$ with a neighborhood of $z$ as the domain
:::

$\star$ it says we can virtually claim a manifold by $\mathbf{F}(z)=0$

intuitively, the definition of manifold should not depend on the coordinates

::: {.callout-note}
$k$-dimensional manifold $M\subset\mathbb{R}^m$, $f$ is a mapping with some properties ^[ $f:U\rightarrow\mathbb{R}^m$ where $U$ is an open subset of $\mathbb{R}^n$ and $[Df(x)]$ is surjective at $\forall x\in f^{-1}(M)$]

then $f^{-1}(M)$ is a submanifold of $\mathbb{R}^n$ of dimension $k+n-m$
:::

as a corollary, manifolds are independent of coordinates if $f$ is an affine transformation

### parametrization of manifolds

parametrization is useful for analysing manifolds since taking a manifolds as domain directly is rather cumbersome

::: {.callout-note}
a *parametrization* of a $k$-dimensional manifold $M\subset\mathbb{R}^n$ is a mapping $\gamma:U\subset\mathbb{R}^k\rightarrow M$, s.t.,

- $U$ is open

- $\gamma$ is $C^1$, one to one, and onto $M$

- $[D\gamma(u)]$ is one to one for every $u\in U$
:::

---

as a comparison, linear algebra and differential calculus have mucn in common

- row reduction $\leftrightarrow$ newton's method

- kernels $\leftrightarrow$ defining manifolds by equations ( e.g., $f(x)=0$ )

- images $\leftrightarrow$ defining manifolds by a parametrization

---

### tangent space

tangent space is similar to the linear approximation of a function $f$, but it is used on a manifold.

- (**derivative**) replace a function $f$ locally by a linear map 
- (**tangent space**) replace a manifold locally by a linear space

for a manifold $M=\left\{\left(\begin{matrix} x\\y \end{matrix}\right)\in\mathbb{R}^n\mid f(x)=y\right\}$

fix $z_0\in M$.

the change of $z$ should be approximated by a linear mapping

$$
y-y_0=[Df(x_0)](x-x_0)
$$

in short $\Delta y=[Df(x_0)]\Delta x$

::: {.callout-note}
the graph of $[Df(x_0)]$ is the **tangent space**, which is denoted as $T_{z_0}M$

the linear approximation to the graph is the graph of the linear approximation
:::

### how to find tangent space

if you have an equation form $F(x)=0$ of the manifold, then you are lucky. 

::: {.callout-note}
the kernel space of the derivative of $F$ **is the tangent space**.

$$
T_{z_0}M=\ker [DF(z_0)]
$$
:::

again, this will certainly remind you the implicit function theorem, where we also claim the derivative of the implicit function

here is another approach to get the tangent space

::: {.callout-note}
Let $U\subset\mathbb{R}^k$ be open, and let $\gamma:U\rightarrow\mathbb{R}^n$ be a parametrization of manifold $M$, then

$$
T_{\gamma(u)}M=\textrm{img}[D\gamma(u)].
$$
:::

it might look strange at first --- why taking the image instead of the kernel like above.

quick calculation: 

$$
[D(F\circ\gamma)(u)]=[DF(\gamma(u))]\circ[D\gamma(u)]=[0]
$$

derivative is not quite like what we think as the origin function

here $\textrm{img}[D\gamma(u)]$ and $\textrm{img}[DF(\gamma(u))]$ are orthogonal complementary subspaces

### maps defined on manifolds

TBD :)