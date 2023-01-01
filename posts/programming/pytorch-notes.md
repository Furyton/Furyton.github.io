---
title: PyTorch Notes
author: Furyton
date: 2022-08-12
categories: [PyTorch, programming]
---

## gadget functions (torch)

+ `x.numel()` number of elements in `x`

+ broadcast mechanism

+ `Y = Y + X` would allocate a new space for the result of `Y + X`. `Y` then is the reference to this new memory. `Y += X` or `Y[:] = Y + X` is better. (`id(Y)` will be unchanged.)

+ `float()` or `int()` converts a scalar tensor to a **standard number**, similar to `x.item()`

+ `A * B` element-wise product.

+ `A_sum = A.sum(axis=1, keepdims=True)` will keep the number of axis. `A / A_sum` will enable broadcast mechanism.

+ `A.cumsum(axis=0)` accumulated sum.

+ linear algebra (matrix, vector) multiplication

  + dot product: `torch.dot(x, y)` only accept 1-D tensor.

  + matrix-vector multiplication: `torch.mv(A, x)` matrix and vector. x is 1-D vector. All in **column** vectors.

  + matrix-matrix multiplication: `torch.mm(A, B)`.

+ `torch.distributions`

  + `multinomial.Multinomial(n, probs).sample(N, )`

## gadget functions (pandas)

+ `data.fillna(data.mean())`

+ `pd.get_dummies(data, dummy_na=True)` Convert categorical variable into dummy/indicator variables. Works for `str`.

## gadget functions (d2l)

+ `d2l.plot` example

  + `x = np.arange(0, 3, 0.1)
d2l.plot(x, [x ** 2, 2 * x - 1], 'x', 'f(x)', legend=['f(x)', 'Tangent'])`

## plot

+ example

  + `plt.plot(y, label=("label"))`

  + `plt.axhline(y=0, color='black', linestyle='dashed')`

  + `plt.gca().set_xlabel('x label')`

  + `plt.gca().set_ylabel('y label')`

  + `plt.legend()`
