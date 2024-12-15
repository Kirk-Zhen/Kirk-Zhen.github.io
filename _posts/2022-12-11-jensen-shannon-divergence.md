---
layout: post
title: Jensen Shannon Divergence
category: tech
---


Jensen–Shannon divergence is a method of measuring the similarity between two probability distributions. It is also known as information radius (IRad). It is a smoothed variant of KL divergence, with some notable (and useful) differences.

$$D_{\mathrm{JS}}(P,Q) := \frac 1 2 D_{\mathrm{KL}}(P\|M)+\frac 1 2 D_{\mathrm{KL}}(Q\|M)$$

where $$M=(P+Q)/2$$


Jensen-Shannon Divergence  is symmetric and it is bounded between $$[0,1]$$(under base 2 logarithm), i.e., if two distribution is disjoint, then $$D_{\mathrm{JS}}(P,Q)=\log 2$$. 

The square root of the Jensen–Shannon divergence is a metric often referred to as Jensen-Shannon distance, which is a distance metric, i.e., triangle inequality holds.

$$d_{\mathrm{JS}}(P,Q) := \sqrt{D_{\mathrm{JS}} (P,Q)}$$

$$\forall P_1, P_2, P_3, \quad d_{\mathrm{JS}}(P_1,P_2) \leq d_{\mathrm{JS}}(P_1,P_3) +d_{\mathrm{JS}}(P_3,P_2)  $$

Jensen-Shannon Divergence is upper bounded by the Total Variation Distance:

$$ 0\leq D_{\mathrm{JS}}(P,Q) \leq d_{\mathrm{TV}}(P,Q)\leq 1$$

#Data Processing Principle 

if $$Z=f(X)$$, and let $P,Q$ be two distribution over $\mathcal{X}$. We use $$f_\sharp P$$ and $$f_\sharp Q$$ to denote the induced distribution of $P,Q$ under $$f:\mathcal{X}\to \mathcal{Z}$$, respectively, then


$$d_{\mathrm{JS}}(f_\sharp P,f_\sharp Q)\leq d_{\mathrm{JS}}(P,Q)$$

Proof: Let $$B\in \{0,1\}$$ be a fair coin with $$1/2$$ on each event, define:

$$M=\begin{cases}
P,\quad B=0\\
Q, \quad B=1
\end{cases}$$

e.g., $$M$$ is a mixture of $$P,Q$$. Let $$X\sim M$$, then:

$$H(X)=-\sum_{x}M(x)\log M(x)=-\sum_x\frac {P(x)+Q(x)} {2} \log \frac {P(x)+Q(x)} {2}$$
$$H(X\mid B) = \frac 1 2 H(X\mid B=0) + \frac 1 2 H(X\mid B=1) =\frac 1 2 H_P(X)+\frac 1 2 H_Q(X) = -\frac 1 2\left(\sum_x P(X)\log P(X)+\sum_x Q(X)\log Q(X)\right)$$

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