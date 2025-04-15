---
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 4
Module:
  - "[[3 - Discrete Random Variables]]"
Date: 2024-09-24
Date created: Thu., Oct. 3, 2024, 1:08:00 pm
Date modified: Thu., Dec. 5, 2024, 6:15:46 pm
---

# Expectation

- Expectation:
    - Numerical measure
    - Summarizes typical or average behaviour of a [[Random Variables|RV]]
    - & The *weighted average* of the values of $X$
        - Weights = corresponding probabilities of those values
        - More weight on values with > probability

> [!def]- Expectation
> If $X$ is a discrete random variable that takes values in a set $S$, its **expectation**, $E[X]$, is defined as $$E[X] = \sum\limits_{ x \in  S } xP(X = x)$$

- Expectation is finite $\iff$ Series is absolutely convergent
- Series is *not* absolutely convergent $\implies$ $X$ has no finite expectation
- When $X$ is [[Random Variables#Discrete Uniform Distribution|uniformly distributed]] on a finite set $\{x_{1}, \dots, x_{n}\}$, all outcomes are equally likely:
    - $$E[X] = \sum_{i=1}^{n} x_{i} P(X = x_{i}) = \sum_{i=1}^{n} x_{i} \left(\frac{1}{n}\right) = \frac{x_{1} + x_{2} + \dots + x_{n}}{n}$$
    - Expectation = regular average of the values
- Other names for expectation:
    - Mean, expected value

## Expectation of a Function of a Random Variable

> [!def]- Expectation of a function of a random variable
> Let $X$ be a random variable that takes values in a set $S$.
> Let $g$ be a function. Then,
> $$E[g(X)] = \sum\limits_{x \in S} g(x)P(X=x)$$

## Expectation of a Linear Function of $X$

> [!def]- Expectation of a linear function of $X$
> For constants $a$ and $b$, and a random variable $X$ with expectation $E[X]$,
> $$E[aX + b] = aE[X] + b$$

- ! In general, $E[g(X)] \neq g(E[X])$

## Properties of Expectation

If I have random variables $X, Y$ and constants $a$, then:

1. $E(a) = a$
2. $E(X + Y) = E[X] + E[Y]$
3. $E(a + bX) = a + bE[X]$
    1. $E[bX] = bE[X]$
    2. $E[a + X] = a + E[X]$
4. $X, Y$ are independent $\implies E[XY] = E[X]E[Y]$ and $E[f(X)g(Y)] = E[f(X)]E[g(Y)]$

## Properties of Variance

If I have random variables $X, Y$ and constants $a$, then

1. $\text{Var}(a + bX) = b^{2} Var(X)$, $SD(a + bX) = bSD(X)$
2. $\text{Var}(aX + bY) = a^{2} \text{Var}(X) + b^{2} \text{Var}(Y) + 2ab \text{Cov}(X, Y)$
    - $\text{Cov}(X,Y)$ is the *covariance* between $X$ and $Y$
    - Measures the linear relationship between $X$ and $Y$
    - $X, Y$ and independent $\implies \text{Cov}(X, Y) = 0$
    - $\implies \text{Var}(aX + bY) = a^{2} \text{Var}(X) + b^{2} \text{Var}(Y)$

# Variances of Random Variables

> [!def] ==Variance of a random variable== with mean $E[X]$ is
> $$\begin{align} \text{Var}[X], V[X] &= E[(X - E[X])^{2}] = \sum\limits_{x \in S} (x-E[X])^{2} P(X = x) \\ &= E[X^{2}] - E[X]^{2} \end{align}$$

## Deriving Variance Formula

$$
\begin{align}
Var[X]  & = E\left[ (X - E[X])^{2} \right] && \text{by definition} \\
 & = E\left[ X^{2} - 2XE[X] + E[X]^{2} \right] && \text{by expansion} \\
 & = E[X^{2}] - E\left[2XE\left[ X \right] \right] + E\left[ E[X]^{2} \right] && \text{by linearity of expectations} \\
 & = E[X^{2}] - 2E[X]E[X] + E[X]^{2} && \text{since } E[X] \text{ is a constant} \\
 & = E[X^{2}] - E[X]^{2}
\end{align}
$$

> [!def]- ==Standard deviation== of $X$ is
> $$SD[X] = \sqrt{ \text{Var}[X] }$$
