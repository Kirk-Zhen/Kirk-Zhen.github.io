---
layout: post
title: Jensen Shannon Divergence
category: tech
---




We have:

$$
\begin{align*}
    I(B;X) 
    &= H(X)-H(X\mid B)\\
    & = -\sum_x\frac {P(x)+Q(x)} {2} \log \frac {P(x)+Q(x)} {2} + \frac 1 2\left(\sum_x P(X)\log P(X)+\sum_x Q(X)\log Q(X)\right)\\
    &= \frac 1 2 \sum_x P(x)\log\frac {P(x)} {{(P(x)+Q(x))}/ {2}}+\frac 1 2 \sum_x Q(x)\log\frac {Q(x)} {{(P(x)+Q(x))}/ {2}}\\
    &= \frac 1 2 \sum_x P(x)\log\frac {P(x)} {M(x)}+\frac 1 2 \sum_x Q(x)\log\frac {Q(x)} {M(x)}\\
    &= \frac 1 2 D_{\mathrm{KL}}(P\|M)+\frac 1 2 D_{\mathrm{KL}}(Q\|M)
    =D_{\mathrm{JS}}(P,Q)
\end{align*}
$$


Apply $$Z=f(X)$$, then by Data Processing Inequality:

$$D_{\mathrm{JS}}(f_\sharp P,f_\sharp Q) = I(B;Z) \leq I(B;X) = D_{\mathrm{JS}}(P,Q)$$ 