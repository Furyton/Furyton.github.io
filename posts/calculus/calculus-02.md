---
title: calculus review 02
author: Furyton
date: 2022-06-27
categories: [calculus, review]
---

## the great *row reduction*

can be used for computing ranks, solving systems of linear equations, etc.

Given $\mathbf{A}_{m\times n}\mathbf{x}=\mathbf{b}$

$[\mathbf{A}\mid\mathbf{b}]$ row reduces to $[\tilde{\mathbf{A}}\mid\tilde{\mathbf{b}}]$, determine the case of the solutions.

::: {.callout-note}
## extend
square matrix can be approximated by invertible matrices (replace zeros in the diagonal entries of the row reduced matrix with $1/n$).
:::

### rank

the dim of the image

row rank = column rank

## Vector Space

### "contrete to abstract" function $\Phi_{v}$

A linear transformation from contrete $\mathbb{R}^n$ coordinate representation to abstract vector space $V$

$$
\Phi_{\{\mathbf{v}\}}(\mathbf{a})=\Phi_{\{\mathbf{v}\}}\left(\begin{matrix}a_1\\\vdots\\a_n\end{matrix}\right)=a_1\mathbf{v_1}+\cdots+a_n\mathbf{v_n},
$$

where $\{\mathbf{v}\}$ is the set of base vectors of $V$.

### Change of bases $P_{\{\mathbf{v}'\}\to \{\mathbf{v}\}}$

$$[P_{\{\mathbf{v}'\}\to\{\mathbf{v}\}}]\mathbf{a}=\mathbf{b},$$

where $\sum_ia_i\mathbf{v}_i'=\sum_ib_i\mathbf{v}_i$

In other words,

$$P_{\{\mathbf{v}'\}\to \{\mathbf{v}\}}=\Phi_{\{\mathbf{v}'\}}^{-1}\Phi_{\{\mathbf{v}\}}$$

## Eigenvectors and Eigenvalues

eigenvectors with **different** eigenvalues are linearly independent.

diagnolization

## Newton's Method

why talking about Newton's Method?

a convenient and practical way to introduce implicit function and inverse function

### def

**target**: solve for a solution $f(x)=0$

start at $a_0$

iterate with $a_i=a_{i-1}-[Df(a_{i-1})]^{-1}f(a_{i-1})$

::: {.callout-note}
notes: since $f(x)\approx f(a)+[Df(a)](x-a)$, letting $f(x)=0$ will give us the next guess.
:::

There is actually a sufficient condition to ensure the convergence.

### Kantorovich's theorem

let $a_0\in\mathbb{R}^n$, $U$: open neighborhood of $a_0$ in $\mathbb{R}^n$, $f:U\to\mathbb{R}^n$

def $h_0=-[Df(a_0)]^{-1}f(a_0)$, $a_1=a_0+h_0$, $U_1=B_{|h_0|}(a_1)$, $M$ as the Lipschitz ratio of $Df(x)$ in $U_1$

**theorem**: if $|f(a_0)||[Df(a_0)]^{-1}|^2M\le\frac{1}{2}$, then $f(x)=0$ has a **unique** solution in the closed ball $\overline{U_1}$ and Newton's Method converges to it with initial guess $a_0$.

::: {.callout-note}
notes 1: in short, $a_1$ is the next guess

notes 2: **a valid value of Lipschitz ratio**

If $|D_kD_jf_i(x)|\le c_{i,j,k}$

then

$$\left(\sum_{1\le i,j,k\le n}c_{i,j,k}^2\right)^{1/2}$$

is valid
:::

### proposition

def $U_0=\{x\mid |x-a_0|\lt 2|h_0|\}$

If $Df$ satisfy Lipschitz condition in $U_0$, then $f(x)=0$ has a unique solution in $\overline{U_0}$ and Newton's Method converges to it.

::: {.callout-note}
If the product is strictly less than $1/2$, then Newton's Method *superconverges*.

If $|h_n|\le\frac{1}{c}$, for $c$ some constant depend on $f$, then

$$|h_{n+m}|\le \frac{1}{c}\cdot\left(\frac{1}{2}\right)^{2m},$$

where $h_i=a_{i+1}-a_{i}$.
:::

### stronger version

replace all lengths of matrices with **norms**.

::: {.callout-note}
norm of a matrix $A$

$$\lVert A\rVert=\sup_{|x|=1} |A\mathbf{x}|$$

easy to check: $\lVert A\rVert\le |A|$
:::

## inverse function theorem

$f$ is continously differentiable

$Df$ is invertible at $x_0$

then $f$ is locally invertible, with differentiable inverse in some neighborhood of $f(x_0)$

::: {.callout-note}
the vigorous version is too lengthy, I am not gonna put it here.

As you might expect, the locality can actually be quantified by Kantorovich's theorem.
:::

## implicit function theorem

$U$: a open subset of $\mathbb{R}^{n+m}$

$\mathbf{F}:U\to\mathbb{R}^n$ a $C^1$ mapping s.t. $\mathbf{F}(\mathbf{c})=0$ and $[D\mathbf{F}(c)]$ is onto

then the system of linear equations $D\mathbf{F}(\mathbf{x})=0$ has n pivotal vars and m nonpivotal vars. there exists a neighborhood of $\mathbf{c}$ where $\mathbf{F}(\mathbf{c})=0$ implicitly defines the n passive vars as a function $\mathbf{g}$ of the m active vars.

::: {.callout-note}
notes 1: $\mathbf{F}$ behaves similar to $D\mathbf{F}$ locally.

in a normal system of linear equations $\mathbf{Ax}=0$, where $\mathbf{x}\in\mathbb{R}^{m+n}$,  $\textrm{rank }\mathbf{A}=n$, the dimension of the kernel space of $\mathbf{A}$ is m which corresponds to the m active vars (the base of the kernel).

notes 2: again, the rigorous version is a little lengthy.
:::

not like the existence theorem, which only claims the existence of the inverse function or the implicit function, using Newton's Method gives us a more practical way to find the function and a more quantified result.
