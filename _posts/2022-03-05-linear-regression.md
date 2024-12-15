---
layout: post
title: linear regression notes (advanced version)
category: example
---

Given a set of data 

$$\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{n}$$


Assume each input data point contains $d$ attributes, i.e., $$x\in \mathbb{R}^d$$, the linear regression looks like:

$$f(x) = \sum^{d}_{i=1}w_ix_i+b= \langle w, x\rangle + b $$

where 

- $$\langle w, x \rangle$$ 
is the inner product of two vectors

- this equation holds: $$\langle w, x\rangle:=\sum_{i=1}^{d} w_{i} x_{i}=: w \cdot x $$

- $$w_i$$ is the linear coefficient for the i-th attribute in $$x$$ 
- $$b$$ is the intercept


Measure the overall loss over the dataset, using mean squared error (MSE):

$$\mathcal{L}=\frac{1}{2 n} \sum_{i=1}^{n}\left(\hat{y}_{i}-y_{i}\right)^{2}$$

In our case, our goal is to minimize the overall loss over the dataset, which means the objective is to minimize the MSE. Combine everything, we have:

$$\min _{w} \quad \frac{1}{2 n} \sum_{i=1}^{n}\left(w \cdot x_{i}-y_{i}\right)^{2}$$

(note that we have to assume $$n \geq d$$, why?)


We want to find the optimal model parameter $w$, and here are some observations:

- The objective function is a quadratic function of $w$
- It is thus a paraboloid
- Global optimal solution exists and is unique (under certain conditions)

The objective function could be written as:

$$\mathcal{L}(w):=\frac{1}{2 n} \sum_{i=1}^{n}\left(w \cdot x_{i}-y_{i}\right)^{2}=\frac{1}{2 n}\|X w-y\|_{2}^{2} $$


Further expand the objective function a bit, we get the quadratic form of the objective function:

$$\mathcal{L}(w):= \frac{1}{2 n} w^{T}\left(X^{T} X\right) w-\frac{1}{n} y^{T} X w+\frac{1}{2 n} y^{T} y$$


Let's take a closer look at the leading quadratic term $$ w^{T}\left(X^{T} X\right) w $$.


Recall that what we have in Linear Algebra, A symmetric matrix $$A$$ is called positive-semidefinite if $$z^TAz\geq 0$$ for every vector $z$ of proper dimension. We claim that $$X^{T} X$$ is a positive-semidefinite matrix. easy proof: $$z_T(X^TX)z = (Xz)^T (Xz) = \text{Forbenius-Norm}(Xz) \geq 0$$

If a quadratic form is positive-semidefinite, then it is convex.

The reason why we care about convexity, is because, for a differentiable convex function, if a global minimum exists, then the gradient is 0 if and only if the point is the global minimizer.


The gradient:

$$\nabla \mathcal{L}(w)=\frac{1}{n}\left(X^T X\right) w-\frac{1}{n} X^T y$$


By setting $$\nabla \mathcal{L}(w)=0 $$, we obtain the normal equation:

$$\left(X^T X\right) w=X^T y$$


Why it called the `Normal' equation, that's because the residual $$(y-Xw)$$ is normal to the data points $$X$$, we can see that the following equation holds:

$$ X^T(y-X w)=0 $$

The equation holds, means that the residual vector is in the null space of $$X^T$$, and each row in $$X^T$$ is perpendicular to the residual vector.


To sum up all we have above, if the normal equation has a solution, the its solution must minimize the objective function.


When does the normal equation $$\left(X^T X\right) w=X^T y$$ have a solution?
Case 1: if $$\text{rank}(X^TX)= d$$, then $$X^TX$$ is invertible, so we have unique solution:

$$ w=\left(X^T X\right)^{-1} X^T y$$

Case 2: if $$\text{rank}(X^TX) < d$$, then $$X^TX$$ is not invertible, there will be infinitely many solutions. Why?

First, observe that 

$$
X^T y \in \operatorname{Col}\left(X^T\right)=\operatorname{Row}(X)=\operatorname{span}\left\{x_1, \ldots, x_n\right\}
$$

Next, we want to show that

$$
\operatorname{Col}\left(X^T X\right)=\operatorname{Col}\left(X^T\right)=\operatorname{Row}(X)=\operatorname{span}\left\{x_1, \ldots, x_n\right\}
$$

let $$X^TX$$ be $$A$$, $$X^Ty$$ be $$v$$, we have $$Aw=v$$. If this equation holds, then $$v$$ is in the col space of $$A$$, so $$w$$ must exist.

The last two equations are by definition, so we only need to show
the first. Equivalently, we only need to show:

$$
\operatorname{Row}\left(X^T X\right)=\operatorname{Row}(X)
$$

By the fundamental theorem of linear algebra, this is equivalent to:

$$
\operatorname{Ker}\left(X^T X\right)=\operatorname{Ker}(X)
$$

which means:

$$
\forall v \neq 0, X^T X v=0 \Longleftrightarrow X v=0
$$


From right to left is obvious, so we only need to show left to right:

$$
X^T X v \Longrightarrow v^T X^T X v=0 \Longrightarrow\|X v\|_2^2=0 \Longrightarrow X v=0
$$

Hence,

$$
X^Ty \in \operatorname{Col}(X^TX)
$$

which means that there exists $$w\in \mathbb{R}^d$$, such that 

$$\left(X^T X\right) w=X^T y$$



So, now, let's show the general form of the solutions for the normal equation.

If $$w_0$$ is a solution for the normal equation, then for any $$v\in \operatorname{Ker}(X)$$, $$w_0 + v$$ is also a solution.  (That's because $$v \in\operatorname{Ker}(X)$$ means $$v \in\operatorname{Ker}(X^TX)$$, it means that $$X^TXv=0$$, adding any 0 to LHS of the normal equation will not alter the value). So, the general form is:

$$w_0 + \operatorname{Ker}(X)$$

As a special case, if $$\operatorname{Rank}(X)=d$$, then $$\operatorname{Ker}(X)=\{0\}$$, and we recover the unique solution case.


Usually, a minimum norm solution is preferred for $$w_0$$ (try to prove it, it's fascinating!) :

$$ w_0 = (X^TX)^+(X^Ty) $$

where $$A^+$$ denotes the Moore-Penrose pseudo-inverse of $$A$$. 

Suppose the linear regression solution is unique, the minimum norm solution will be:

$$
w_* = (X^TX)^{-1}(X^Ty)
$$


For future observation $$x\in \mathbb{R}^d$$, we use the following expression as prediction:

$$
f(x)=\left\langle w^{*}, x\right\rangle=x^{T}\left(X^{T} X\right)^{-1} X^{T} y
$$


For time complexity, linear regression is fast to test, and fast to train:
- $$O(d)$$ in test time.
- $$O(d^3 + nd^2)$$ for solving the normal equation.