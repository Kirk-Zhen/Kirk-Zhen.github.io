---
layout: post
title: logistic regression notes (advanced version)
category: tech
---

# Linear Classifier

Given a set of data  $$\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{n}$$ if we have a classifier $$ \hat{y_t} = f(x_i)$$, how often does my classifier make correct predictions?

The simple answer would be an indicator function:

$$\ell(\hat{y}, y) = \mathbb{I}(\hat{y} \neq y) $$

Note: $$\mathbb{I}(\hat{y} \neq y)$$ return 1 if $$\hat{y} \neq y$$, otherwise return 0.


Measure the overall loss over the dataset, using binary classification error:

$$\mathcal{L}=\frac{1}{ n} \sum_{i=1}^{n} \ell(\hat{y_i}, y_i) = \sum_{i=1}^{n} \mathbb{I}(\hat{y_i} \neq y_i)$$

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

$$\min _{w} \quad \sum_{i=1}^{n} \mathbb{I}( \text{sgn} (w\cdot x_i) \neq y_i)$$

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
    \mathcal{L}\left(w \mid\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{n}\right) 
    &:=\sum_{i=1}^{n} \log \operatorname{Pr}\left(Y=y_{i} \mid X=x_{i}, w\right) \\
    & =\sum_{i=1}^{n} y_{i} \log \sigma\left(w \cdot x_{i}\right)+\left(1-y_{i}\right) \log \sigma\left(-w \cdot x_{i}\right)
    \end{align*}
    $$

    - Conditional loglikelihood function is a function of the model parameter $$w$$.
    - Conditional likelihood function is also a conditional probability, measuring “given the current model parameter, how likely is the data generated from the current model?”
    -  conditional probability is always below $$1$$, for a perfect predictor, we have $$\Pr = 1$$, with a high probability to observe the correct label, so, we want to max the log-likelihood
    - the sum here is the product of $$\Pr$$, which means we want to maximize the likelihood of all samples. But the product of lots of 0 to 1 float is super small,  which will cause problems for numerical optimization. So, we take the logarithm and transfer the product to summation.

Verify yourself: $$\ell(t) = -\log \sigma(t)$$ is convex in $$t$$, so the negative-log-likelihood function in convex in $$w$$. 


