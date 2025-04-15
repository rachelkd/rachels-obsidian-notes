---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
aliases: []
Week: 11
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/JvMEYIPHcqWjrtPBX0XusL
Date: 2024-11-19
Date created: Sat., Nov. 23, 2024, 8:42:50 pm
Date modified: Tue., Jan. 28, 2025, 1:25:37 am
---

# Review

Recall **expected values**:

![[Probability Distributions of More than One RV#^7711bc]]

![[Probability Distributions of More than One RV#^57163f]]

![[Probability Distributions of More than One RV#^f90dcd]]

# Conditional Expectations Are Expectations

- Have same **properties** as “regular” expectations

> [!thm]+ Properties of Conditional Expectations
>
> 1. (**Linearity**) For constants $a, b$ and random variables $X, Y, W$,
> $$E[aW + bY|X] = aE[W|X] + bE[Y|X]$$
> 2. (**Law of the unconscious statistician**) If $g$ is a function, then
> $$E[g(Y) | X = x] = \begin{cases} \sum\limits_{y} g(y) P(Y = y | X = x), && \text{discrete} \\ \int_{y} g(y)f_{Y|X}(y | x)\, dy && \text{continuous} \end{cases}$$
> 3. (**Independence**) If $X$ and $Y$ are independent, then
> $$E[Y|X] = E[Y]$$which is a number
> 4. (**$Y$ function of $X$**) If $Y = g(X)$ is a function of $X$, then
> $$E[Y|X] = E[g(X)|X] = g(X) = Y$$

- Last property means: If $Y = g(X)$ (the only random component depends on $X$), then $E[g(X)|X] = g(X)$
  - e.g., $E(X^{2} | X) = X^{2}$

# Law of Total Expectation

- Recall: **[[Conditional Probability#Law of Total Probability|Law of Total Probability]]**
- We often collect data on different groups separately, rather than collecting data on the whole population at once
- We can use the **Law of Total Probability**
  - $$P(A) = P(A | B_{1})P(B_{1}) + \cdots + P(A|B_{k})P(B_{k}) $$
- We can do something similar with ==expectations==

> [!thm]+ Law of Total Expectation
> For a random variable $Y$ and any random variable $X$ defined jointly with $Y$, the expectation of $Y$ is equal to the expectation of the conditional expectation of $Y$ given $X$
> $$E[Y] = E[E[Y|X]]$$

- $E[Y|X]$ is a function of $X$
    - When we write $E[Y|X]$, we’re actually representing $E[Y|X = x]$ for any value $x$ that $X$ can take
    - For each specific value of $X = x$, we get a different expected value of $Y$
    - This creates a mapping from $x$ values to expected values, which is the definition of a function
$$E(Y) = E(E(Y|X)) = \int_{\text{all }x} \int_{\text{all }y} y f_{Y|X}(y|x)f_{X}(x) \,dy \,dx$$

$$
E(X) = \sum\limits_{i} E(X | B_{i})P(B_{i})
$$

## Discrete Example

- We know that 50% of the population we are interested in is male.
    - The rest are female.
- The expected proportion of males who are smokers is 40%.
- The expected proportion of females who are smokers is 70%.

> [!question]+ What is the expected proportion of the populartion that smokes?
>
> Let $X$ be the proportion of smokers in the whole population.
>
> $$
> \begin{align*}
> E[X] &= E[X | \text{gender = male}]P(\text{gender = male}) + E[X|\text{gender = female}]P(\text{gender = female}) \\
> &= 0.4 \times 0.5 + 0.7 \times 0.5 \\
> &= 0.55
> \end{align*}
> $$
>
> ![|center|400](https://i.imgur.com/XpRxhSu.jpeg)

## Proof

Recall the **[[Conditional Probability|General Multiplication Rule]]**

$$
P(X | Y) = \frac{P(X \cap Y)}{P(Y)}
$$

$$
\begin{align*}
E[E[X|Y]] &= E\left[ \sum\limits_{\text{all }x} x \cdot P(X = x | Y) \right] \\
&= \sum\limits_{\text{all }y}\left[ \sum\limits_{\text{all }x} x \cdot P(X = x | Y = y) \right] P(Y = y) \\
&= \sum\limits_{y} \sum\limits_{x} x P(X = x | Y = y)P(Y = y) \\
&= \sum\limits_{y} \sum\limits_{x} x P(X = x, Y = y) && \text{(General Multiplication Rule)} \\
&= E[X]
\end{align*}
$$

## Another Example

- Gracia makes $Q$ practice questions to post on Quercus, where $Q \sim Pois(\lambda)$
- Each question is independent, and has probability of $p$ of having an error

> [!question]+ What is the expected number of questions posted that has an error?
> Let $X$ be the # questions with an error.
> $X | Q \sim Binom(Q, p)$
>
> $$
> \begin{align*}
> E(X) &= E(E(X | Q)) \\
> &= E(Qp) && \text{from binomial distribution} \\
> &= pE(Q) && \text{since }p \text{ is constant} \\
> &= p \lambda && \text{from Poisson distribution}
> \end{align*}
> $$

## Example. Uniform Distribution

- You simulate two random numbers
- First, you simulate $X \sim Unif(0, 1)$
- Then, you simulate $Y \sim Unif(0, x)$, where $x$ is the first number simulated

> [!question]+ What is the expected value of $Y$?
> Given:
>
> - $X \sim Unif(0,1)$
> - $Y|X \sim Unif(0,x)$
>
> Using Law of Total Expectation:
> $$
> \begin{align*}
> E[Y] &= E_X[E_Y[Y|X]] \\
> &= E_X[\frac{x}{2}] && \text{(mean of Unif(0,x) is x/2)} \\
> &= \frac{1}{2}E_X[X] && \text{(pull out constant)} \\
> &= \frac{1}{2} \cdot \frac{1}{2} && \text{(mean of Unif(0,1) is 1/2)} \\
> &= \frac{1}{4}
> \end{align*}
> $$

## Example. Die Roll, Coin Flip

- You are playing a game where you roll a dice, and then flip the same number of coins as the number shown on the dice
- You win $1 for every heads that you get

> [!question]+ If the dice roll shows the number $k$, how much money do you expect to win?
> Let $X$ be the number of dollars won.
> $X | K \sim \text{Binom}(k, 0.5)$
> $E(X | K) = 0.5k$

> [!question]+ How much money do you expect to win in this game?

$$
\begin{align}
E(X)  & = E_{k} \left[ E_{X|k}[X|K] \right]  \\
 & = E_{k}[0.5k] \\
 & = 0.5E_{k}[k] \\
 & = 0.5\left( \frac{{1 + 6}}{2} \right) && (\text{since }k \sim \text{Discrete Uniform over }\{ 1,2,3,4,5,6 \}) \\
 & = 1.75
\end{align}
$$

# Conditional Variance

> [!thm]+ Conditional Variance of $Y$ given $X = x$
> $$
> V[Y|X = x] = \begin{cases}
> \sum\limits_{y} (y - E[Y|X = x])^{2} P(Y = y| X = x), \quad Y \text{ is discrete}  \\
> \int_{y} (y - E[Y|X =x])^{2} f_{Y|X}(y|x) \, dy, \quad Y \text{ is continuous}
> \end{cases}
> $$

## Properties of Conditional Variance

> [!thm]+ Properties of Conditional Variance
>
> 1. $$V[Y | X = x] = E[Y^{2}|X=x] - (E[Y|X = x])^{2}$$
> 2. For constants $a, b$,
>
> $$
> V[aY + b|X = x] = a^{2}V[Y|X = x]
> $$
>
> 3. If $W$ and $Y$ are independent, then
>
> $$
> V[W + Y | X = x] = V[W | X = x] + V[Y | X = x]
> $$

- Conditional variance has the same properties as the “regular” variance

## Law of Total Variance

> [!thm]+ Law of Total Variance
> $$
> V[Y] = E[V[Y|X]] + V[E[Y|X]]
> $$

- If $X$ is discrete, this can be interpreted as:
  - The variance of $Y$ is the sum of the “within-group” (or unexplained) variance and the “between-group” (or explained by group membership) variance

![|center|500](https://i.imgur.com/vzaxkbx.jpeg)

### Proof of Law of Total Variance

$$
\begin{align*}
E[V[Y|X]] &= E\left[E[E[Y^{2} | X] - (E[Y|X]^{2})]\right] \\
&= E\left[ E \left[ Y^{2}|X \right]  \right] - E \left[ (E\left[ Y|X \right])^{2}  \right] \\
&= E[Y^{2}] - \bbox[lavender]{E \left[ (E[Y|X])^{2} \right]} && \text{(Law of Total Expectation)}\\

V[E[Y|X]] &= E \left[ (E[Y|X])^{2} \right] - \left( E[E[Y|X]] \right) ^{2} \\
&= \bbox[lavender]{E\left[(E[Y|X])^{2}\right]} - \left[E[Y]\right]^{2} && \text{(Law of Total Expectation)}
\end{align*}
$$
So,
$$
E\left[ V \left[ Y|X \right]  \right] + V\left[ E \left[ Y | X \right]  \right] = E[Y^{2}] - \left(E[Y]\right)^{2} = V[Y]
$$

### Example

- Gracia makes $Q$ practice questions to post on Quercus, where $Q \sim Pois(\lambda)$
- Each question is independent, and has probability $p$ of having an error

> [!question]+ What is the variance of the number of questions posted that has an error?
> Let $X =$ # questions with an error.
> $X | Q \sim Binom(Q, p)$
> $$
> \begin{align*}
> Var(X) &= E \left[ \underbrace{V \left[ X | Q \right]}_{Qp(1-p)}  \right] + V \left[ \underbrace{E \left[ X | Q \right]}_{Qp}  \right] \\
> &= E \left[ Qp(1-p) \right] + V(Qp) && \text{(by binomial distribution)} \\
> &= p(1-p) E(Q) + p^{2} V(Q) \\
> &= p(1-p) \lambda + p^{2} \lambda \\
> &= p \lambda
> \end{align*}
> $$

> [!question]+ What is the covariance between the number of questions Gracia makes in total ($Q$) and the number of questions with errors ($X$)?
>
> Given:
>
> - $Q \sim Pois(λ)$ (total questions)
> - $X|Q \sim Bin(Q,p)$ (questions with errors)
>
> $$
> \begin{align*}
> Cov(Q,X) &= E(QX) - E(Q)E(X) \\
> &= E_Q[E_X(QX|Q)] - E(Q)E(X) && \text{(Law of Total Expectation)} \\
> &= E_Q[Q \cdot E_X(X|Q)] - E(Q)E(X) && \text{(Q constant given Q)} \\
> &= E_Q[Q \cdot Qp] - λ \cdot λp && \text{(mean of binomial, mean of Poisson, and earlier example)} \\
> &= pE_Q[Q^2] - λ^2p \\
> &= p(λ + λ^2) - λ^2p && \text{(using } E(Q^2) = Var(Q) + (E(Q))^2 \text{ for Poisson)} \\
> &= pλ + pλ^2 - λ^2p \\
> &= λp
> \end{align*}
> $$
>
> The positive covariance $λp$ indicates that as the total number of questions increases, the number of questions with errors tends to increase as well.

### Example. Uniform Distribution

- You simulate two random numbers
- First simulate $X \sim Unif(0, 1)$
- Then, simulate $Y \sim Unif(0, x)$, where $x$ is the first number simulated

> [!question]+ What is the variance of $Y$?
>
> $$
> \begin{align*}
> Y|X \sim Unif(0, x) &\implies Var(Y|X) = \frac{(x-0)^{2}}{12} = \frac{x^{2}}{12} \\
> X \sim Unif(0, 1) &\implies Var(X) = \frac{(1-0)^{2}}{12} = \frac{1}{12}
> \end{align*}
> $$
> Then, $E[X^2] = V[X] + (E[X])^2 = \frac{1}{12} + (\frac{1}{2})^2 = \frac{1}{3}$
>
> Using Law of Total Variance:
> $$
> \begin{align*}
> Var(Y) &= E_X\left[V_{Y|X}\left[Y|X\right]\right] + V_X\left[E_{Y|X}\left[Y|X\right]\right] \\
> &= E_X\left[\frac{x^2}{12}\right] + V_X\left[\frac{x}{2}\right] \\
> &= \frac{1}{12}E_X\left[X^2\right] + \frac{1}{4}V_X\left[X\right] \\
> &= \frac{1}{12}\left(\frac{1}{3}\right) + \frac{1}{4}\left(\frac{1}{12}\right) \\
> &= \frac{7}{144} \\
> &\approx 0.0486
> \end{align*}
> $$
