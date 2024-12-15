---
layout: post
title: logistic regression notes (advanced version)
category: tech
---

# Linear Classifier

Given a set of data  $$\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{n}$$ if we have a classifier $$ \hat{y_t} = f(x_i)$$, how often does my classifier make correct predictions?

The simple answer would be an indicator function:

$$\ell(\hat{y}, y) = \I(\hat{y} \neq y) $$

Note: $$\I(\hat{y} \neq y)$$ return 1 if $$\hat{y} \neq y$$, otherwise return 0.


Measure the overall loss over the dataset, using binary classification error:

$$\L=\frac{1}{ n} \sum_{i=1}^{n} \ell(\hat{y_i}, y_i) = \sum_{i=1}^{n} \I(\hat{y_i} \neq y_i)$$

But, we haven’t specified how to obtain the prediction. If the linear model is applied, ideally, our model should look like this:

$$\hat{y} = f(x) = \text{sgn} \left( \sum_{i=1}^{n} w_ix_i\right) = \text{sgn}(w\cdot x)$$


where $\text{sgn}(\cdot)$ is the sign function:
$$
\begin{equation*}
    \text{sgn}(t) = 
    \begin{cases}
    1 \quad \text{if } t\geq 0\\
    0 \quad \text{otherwise}
    \end{cases}
\end{equation*}
$$


The classification problem becomes the 0-1 loss minimization problem:

$$\min _{w} \quad \sum_{i=1}^{n} \I( \text{sgn} (w\cdot x_i) \neq y_i)$$

Pros:
- Robust to outliers, even if a prediction is away from the decision boundary, the penalty is still 1

Cons:
- Loss function is discontinuous, non-convex
- Gradient is 0 a.e., not informative for local improvement
- Intractable to solve the opt. (NP-hard, even approximately)

# Logistic Regression
How can we rescue the cons of a simple linear classifier? Key idea: use a convex surrogate loss  function.

1. Smooth the output, use the Sigmoid function to replace the sign function:

$$f(x) = \sigma(w\cdot x), \quad \sigma(t) = \frac {1} {1+e^{-t}}$$

2. interpret the output as a conditional probability

    Since:

    $$f(x)=\sigma(w\cdot x \in  (0,1))$$

    Define:

    $$\Pr (Y=1|X=x,w)=f(x) = \sigma(w\cdot x)$$

    Hence:

    $$\Pr (Y=0|X=x,w)=f(x) = 1- \sigma(w\cdot x) = \sigma(-w\cdot x)$$
    
    Realize that $$y\in \{0,1\}$$, so:

    $$\Pr (Y=y|X=x,w)=f(x) = \sigma(w\cdot x)^y \cdot  \sigma(-w\cdot x)^{(1-y)}$$

3. maximizing conditional log-likelihood function
    $$
    \begin{align*}
    \L\left(w \mid\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{n}\right) 
    &:=\sum_{i=1}^{n} \log \operatorname{Pr}\left(Y=y_{i} \mid X=x_{i}, w\right) \\
    & =\sum_{i=1}^{n} y_{i} \log \sigma\left(w \cdot x_{i}\right)+\left(1-y_{i}\right) \log \sigma\left(-w \cdot x_{i}\right)
    \end{align*}
    $$

    - Conditional loglikelihood function is a function of the model parameter $$w$$.
    - Conditional likelihood function is also a conditional probability, measuring “given the current model parameter, how likely is the data generated from the current model?”
    -  conditional probability is always below $$1$$, for a perfect predictor, we have $$\Pr = 1$$, with a high probability to observe the correct label, so, we want to max the log-likelihood
    - the sum here is the product of $$\Pr$$, which means we want to maximize the likelihood of all samples. But the product of lots of 0 to 1 float is super small,  which will cause problems for numerical optimization. So, we take the logarithm and transfer the product to summation.

Verify yourself: $$\ell(t) = -\log \sigma(t)$$ is convex in $$t$$, so the negative-log-likelihood function in convex in $$w$$. 


![algo](https://github.com/Kirk-Zhen/Kirk-Zhen.github.io/blob/main/figures/lr_algo.jpg?raw=true)

Do it yourself: try to derive the gradient of the above loss function.




Suppose we are given the optimal model parameter $$w^*$$, how to perform prediction?

Recall our probability model:

$$\Pr (Y=1|X=x,w^*)=f(x) = \sigma(w^* \cdot x)$$

We can do this:

- if $$\sigma(w^* \cdot x) \geq 0.5 $$, then predict 1.
- Otherwise predict 0.

Equivalently, this means

- if $$w^* \cdot x \geq 0 $$, then predict 1.
- Otherwise predict 0.

So, logistic regression is a linear classifier.



# Generalization of Logistic Regression to Multiple Class

For a two-class classification problem, we need only one $$w$$ for the decision boundary which helps to separate the two classes in the feature space. But, what's the nature of such $$w$$? 


Try to view the conditional probability of logistic regression in another way :

- For class 0: define a normal vector $$w_0$4 such that 

    $$\Pr (Y=0|X=x)\propto e^{w_0 \cdot x}$$

- For class 1: define a normal vector $$w_1$$ such that 

    $$\Pr (Y=1|X=x)\propto e^{w_1 \cdot x}$$    


By normalization (the two prob sums to 1), we have:

$$
\begin{align*}
    \Pr (Y=1|X=x) 
    &= \frac  {e^{w_1 \cdot x}} {e^{w_0 \cdot x} + e^{w_1 \cdot x}}\\
    &= \frac  {1} {1 + e^{ -  (w_1-w_0 ) \cdot x}}
\end{align*}
$$

So, it suffices to learn: $$w^*:= w_1 - w_0 $$.


Suppose we have $$K$$ classes, then 
-  For class 1: define a normal vector $$w_1$$ such that 

$$\Pr (Y=1|X=x)\propto e^{w_1 \cdot x}$$

    ...

-  For class $$K$$: define a normal vector $$w_K$$ such that 

$$\Pr (Y=K|X=x)\propto e^{w_K \cdot x}$$

Again, by normalization, we obtain:


$$
\begin{align*}
    \Pr (Y=i|X=x) 
    &= \frac  {e^{w_i \cdot x}} {\sum_{j\in [K]} e^{w_j \cdot x}}
\end{align*}
$$


- This is called the “softmax” function in the literature
- For the loss function, apply the principle of maximizing conditional log-likelihood
- Clearly, the solution is not unique, because of redundancy