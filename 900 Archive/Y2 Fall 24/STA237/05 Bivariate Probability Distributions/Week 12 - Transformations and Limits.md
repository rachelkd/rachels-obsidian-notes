---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 12
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Date: 2024-11-26
Lecture:
  - "21"
  - "22"
Quercus Page: https://q.utoronto.ca/courses/354355/pages/week-12-nov-25-dec-1-transformations-and-limits
Slides:
  - https://share.goodnotes.com/s/XXMorlcfEcVaGCtFsKwHyp
Date created: Mon., Dec. 2, 2024, 6:26:08 pm
Date modified: Fri., Dec. 6, 2024, 7:19:56 am
---

# Transformations and Limits

> [!goal]- Learning Objectives
> - **12.1** Describe the limiting probability behaviour of averages of several independent random variables.
> - **12.2** Explore the law of large numbers and central limit theorem via simulation in R.
> - **12.3** Explain the difference between the law of large numbers and the central limit theorem.
> - **12.4** Apply the central limit theorem to solve practical problems.

## Review: Transformations

Recall **[[Functions of Two RVs#Transformation Method for Joint PDF]]**.

> [!thm]+ Transformation Method
> If $Y = g(X)$ is strictly increasing or strictly decreasing over the range of $X$, so that it is **invertible** with inverse function $X = g^{-1}(Y)$, then
> $$
> f_{Y}(y) = f_{X}\left( g^{-1}(y) \right) \left| \frac{d}{dy} g^{-1}(y) \right| 
> $$

> [!thm]+ Transformation Method in two (or more) dimensions
> $$
> f_{V, W}(v, w) = f_{X, Y}\left( h_{1}(v, w), h_{2}(v, w) \right) |J|
> $$
> where $v = g_{1}(x, y), u = g_{2}(x, y)$ is an *invertible* bivariate transformation with inverse $x = h_{1}(v, w), y = h_{2}(v, w)$, and
> $$J = \underbrace{\begin{vmatrix}
> \frac{\partial h_1}{\partial v} & \frac{\partial h_1}{\partial w} \\
> \frac{\partial h_2}{\partial v} & \frac{\partial h_2}{\partial w}
> \end{vmatrix}}_{\text{determinant}} =
> \frac{\partial h_1}{\partial v}\frac{\partial h_2}{\partial w} -
> \frac{\partial h_2}{\partial v}\frac{\partial h_1}{\partial w}$$

- [[Law of Large Numbers]]
- [[Central Limit Theorem]]

## Summary of LLN and CLT

- Let $\overline{X}_{n} = \frac{{X_{1} + \dots + X_{n}}}{n}$, where the $X_{i}$‘s are i.i.d. from any distribution with finite mean $\mu$ and variance $\sigma^{2}$
- ? What do we know about the distribution of $\overline{X_{n}}$ (the sampling distribution)

| Property                                                                                          | Sample size       | Why?                                                                                                  |
| ------------------------------------------------------------------------------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------- |
| $$E\left(\overline{X}_{n}\right) = \mu, V\left( \overline{X}_{n} \right) = \frac{\sigma^{2}}{n}$$ | Any $n$           | Properties of expectation and variance; $\overline{X}_{n}$ is a linear combination od independent RVs |
| $$\bar{X}_{n} \to \mu$$                                                                           | As $n \to \infty$ | LLN                                                                                                   |
| Distribution of $\bar{X}_{n}$ approaches the [[Normal Distribution]]                              | As $n \to \infty$ | CLT                                                                                                   |
