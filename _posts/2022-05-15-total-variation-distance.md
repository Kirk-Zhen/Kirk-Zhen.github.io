---
layout: post
title: Total Variation Distance
category: tech
---


The total variation distance is a distance measure for probability distributions. Can be regarded as the largest possible difference between the probabilities that the two probability distributions can assign to the same event.


$$d_{TV} (P,Q) := \sup_{A\subseteq \mathbb{R}^d } |P(A)-Q(A)|$$


Total variation distance is half the absolute area between the two curves: Half the shaded area below.

$$d_{\mathrm{TV}} (P,Q) := \frac 1 2 \sum_{x\in \mathrm{X}}|P(x)-Q(x)|=:\frac 1 2 \|P-Q\|_1 $$


Proof: 

Let $A^*$$ be the event such that $$d_{\mathrm{TV}}(P,Q)=P(A^*)-Q(A^*)$$. Then, $$\forall x\in \mathrm{X}\backslash A^*,\, P(x)<Q(x) $$, hence,


$$
\begin{align*}
    \|P-Q\|_{1} &=\sum_{x \in \mathrm{X}}|P(x)-Q(x)| =\sum_{x \in A^{*}}(P(x)-Q(x))+\sum_{x \in \mathrm{X} \backslash A^{*}}(Q(x)-P(x)) \\
    &=\left(P\left(A^{*}\right)-Q(A^*)\right)+\left(1-Q\left(A^{*}\right)-\left(1-P\left(A^{*}\right)\right)\right) \\
    &= 2 \left(P\left(A^{*}\right)-Q(A^*)\right) = 2 d_{\mathrm{TV}}(P, Q)
\end{align*}

$$

Some properties:

- Always non-negative, symmetric and bounded between $$[0,1]$$.
- TV-distance does not use the underlying geometry of $$\mathbb{R}^d$$.
- TV-distance is a strong one, e.g., if $\mu$ is continuous, then $$d_{\mathrm{TV}}(\hat{\mu}_n, \mu) = 1, \forall n$$ (each point in a continuous distribution has probability 0).

The total variation distance is related to the KL divergence by  Pinsker's inequality

$$d_{\mathrm{TV}}\leq \sqrt{\frac 1 2 D_{\mathrm{KL}}(\cdot\| \cdot)}$$

One also has the following inequality, which has the advantage of providing a non-vacuous bound even when $$D_{\mathrm {KL} }(P\parallel Q)>2 $$:

$$d_{\mathrm{TV}}(\cdot, \cdot) \leq \sqrt{1-\exp \left(-D_{\mathrm{KL}}(\cdot \| \cdot)\right)}$$