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
is the inner product of two vectors:

- $$\langle w, x\rangle:=\sum_{i=1}^{d} w_{i} x_{i}=: w \cdot x $$

- $$w_i$$ is the linear coefficient for the i-th attribute in $$x$$ 
$$b$$ is the intercept
