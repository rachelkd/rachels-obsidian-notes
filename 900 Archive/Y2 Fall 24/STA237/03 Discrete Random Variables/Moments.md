---
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 4
Module:
  - "[[3 - Discrete Random Variables]]"
Date: 2024-09-24
Date created: Thu., Oct. 3, 2024, 2:08:44 pm
Date modified: Thu., Dec. 5, 2024, 6:19:14 pm
---

- Some expectations have special names:
    - For $k = 1, 2, \dots,$ the ==$k$-th moment of a random variable $X$ is $E[X^{k}]$==
- Moment-generating function (mgf) → generates the moments of a random variable
    - Also useful for demonstrating some relationships between random variables

> [!def]- Moment-generating functions
> Let $X$ be a random variable.
> The **mgf** of $X$ is the real-valued function
> $$m_{X}(t) = E[e^{tX}] = \sum\limits_{\forall x} e^{tx}p(x)$$
> defined for all real $t$ when this expectation exists

- If mgf exists, then we can calculate the $k$-th moment of $X$ as:
    - $$E(X^{k}) = \left.\frac{d^{k}}{dt^{k}} m_{X}(t)\right|_{t=0} = m_{X}^{(k)}(0)$$
- ! For some distributions, $E(e^{tX})$ is not finite (e.g., Cauchy Distribution)

# Properties of Moment-Generating Functions

1. If $X$ and $Y$ are independent random variables, then the mgf of their sum is the product of their mgfs
    - $$\begin{align} m_{X+Y}(t) &= E[e^{t(X+Y)}] \\ &= E[e^{tX} e^{tY}] \\ &= E[e^{tX}] E[e^{tY}] \\ &= m_{X}(t)m_{Y}(t) \end{align}$$
2. Let $X$ be a random variable with mgf $m_{X}(t)$ and constants $a \neq 0, b$
    - $$m_{aX+b} = E[e^{t(aX+b)}] = e^{bt} E[e^{(ta)X}] = e^{bt}m_{X}(at)$$
3. & Mgfs uniquely determine the underlying probability distribution
    - i.e., If two random variables have the same mgf, they have the same probability distribution
