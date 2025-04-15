---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 3
Module:
  - "[[3 - Discrete Random Variables]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 2024-09-24
Date created: Wed., Oct. 16, 2024, 6:24:56 pm
Date modified: Thu., Dec. 5, 2024, 5:56:15 pm
---

- A random variable assigns *numerical values* to the outcomes of a random experiment ([[Random Variables]])

> [!def]+ Discrete random variable
> Let $\Omega$ be a sample space.
> - A **discrete random variable** is
>     - A *function* $X : \Omega \to \mathbb{R}$ that takes on a *finite* number of values $a_{1}, a_{2}, \dots, a_{n}$, or
>     - A *countably infinite* number of values $a_{1}, a_{2}, \dots$

# Probability Distribution

> [!def]+ Probability distribution
> - The **probability distribution** of a random variable $X$ (based on a random experiment and associated sample space) tells us
>     - What values $X$ can take on and how to assign probabilities to those values
> - For a discrete random variable $X$ that takes values in a set $S$, the **probability mass function (pmf)** is the probability function:
>     - $$p(x) = P(X = x), x \in S$$

| $x$               | 2    | 3    | 5   | 8    | 10   |
| ----------------- | ---- | ---- | --- | ---- | ---- |
| $p(x) = P(X = x)$ | 0.15 | 0.10 | ?   | 0.25 | 0.25 |

1. What is $P(X = 5)$
    - $P(X = 5) = 1 - P(P \neq 5) = 1 - 0.15 - 0.10 - 0.25 - 0.25 = 0.25$
- What is the probability that $X = 2$ or $X = 10$?
    - $P(X = 2 \cup X = 10) = P(X = 2) + P(X = 10) = 0.15 + 0.25 = 0.4$
- $P(X < 8)$
    - $P(X < 8) = P(X = 2) + P(X = 3) + P(X = 5) = 0.5$
- $P(X < 8)$
    - $0.5$

# Cumulative Distribution Function (CDF)

> [!def]+ Cumulative distribution function
> $$F(x) = P(X \leq x)$$
> - Represents the cumulative probability, less than or equal to $x$
> - For a discrete random variable, the CDF is a step function

## Properties of the CDF

1. $F(x)$ is a non-decreasing function of $x$
2. $0 \leq F(x) \leq 1$ for all $x$
3. $\lim_{ x \to -\infty } F(x) = 0$ and $\lim_{ x \to \infty } F(x) = 1$
    - $\lim_{ x \to -\infty } P(X \leq x) = 0$
    - $\lim_{ x \to \infty } P(X \leq x) = 1$
4. $F(x)$ is right-continuous at all $x$
    - $\lim_{ \epsilon \to 0^{+} } F(x + \epsilon) = F(x)$ for all $x$

![](https://i.imgur.com/PnsfeAl.png)

## Going from PMFs to CDFs

![](https://i.imgur.com/kijZym0.png)
