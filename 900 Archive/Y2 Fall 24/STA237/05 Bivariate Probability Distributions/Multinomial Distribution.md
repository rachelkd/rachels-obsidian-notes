---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 12
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 2024-11-28
Date created: Thu., Dec. 5, 2024, 2:09:25 pm
Date modified: Thu., Dec. 5, 2024, 3:16:18 pm
aliases: []
---

# Multinomial Distribution

> [!def]+ Multinomial Distribution
> - Multivariate *discrete* probability distribution
> - Generalization of the [[The Bernoulli and Binomial Distributions|Binomial]] distribution for more than one possible outcome for success/fail
>
> Consider an experiment that consists of $n$ i.i.d. trials, where
>
> - Each trial can result in one of $r$ possible outcomes with probabilities $p_{1}, p_{2}, \dots, p_{r}$
>     - These probabilities must add to 1
>
> The joint PMF for $X_{i}$ for $i = 1, 2, \dots, r$, the frequencies (counts) of each of the $r$ outcomes in $n$ trials is:
>
> $$
> p_{X_{1}, X_{2}, \dots, X_{r}}(x_{1}, x_{2}, \dots, x_{r}) = \frac{n!}{x_{1}! \cdot x_{2}! \cdot \dots \cdot x_{r}!} p_{1}^{x_{1}} p_{2}^{x_{2}} \dots p_{r}^{x_{r}}
> $$
> for $x_{i} \in \{ 0, 1, \dots, n \}, (i = 1, 2, \dots, r), x_{1} + x_{2} + \dots + x_{r} = n$

- $r$ possible outcomes need *not* to be ordered
    - i.e., They can be unordered categories
- Order of $x$ values *must* be consistent with the ==order of the probabilities==
    - Index $i$ represents a specific category
- Marginal distributions for the *frequency* of each category are [[The Bernoulli and Binomial Distributions|Binomial]]
    - $X_{i} \sim Bin(n, p_{i})$ for $i = 1, \dots, r$

## Multinomial Probabilities and Realizations in R

> [!important]+ `dmultinom(x, prob)`
> - Computes the multinomial PMF for a ==specified vector== of $x$ values and the associated vector of probabilities
>     - i.e., Height of the pmf at $(x_{1}, \dots, x_{r})$
>     - Not the CDF
> - Both vectors are of length $r$
> - e.g., `dmultinom(x=c(1, 2, 1), prob=c(0.2, 0.5, 0.3))` computes:
>     - $p_{X_{1}, X_{2}, X_{3}}$ where $r = 3, n = 4, p_{1} = 0.2, p_{2} = 0.5, p_{3} = 0.5$

> [!important]+ `rmultinom(k, size, prob)`
> - Generates $k$ vectors of $X$ values (each of length $r$) from a multinomial distribution, with
> - Size $= n$
>     - Number of items which are organized into $r$ categories
> - Probabilities specified as a *vector* of length $r$
> - e.g., `rmultinom(5, size=4, prob=c(0.2, 0.5, 0.3))`
>     - Randomly generates 5 realizations of the $r = 3$ counts (i.e., $x_{1}, x_{2}, x_{3}$) when $n = 4$ items are distributed across the $r = 3$ categories according to probabilities $p_{1} = 0.2, p_{2}=0.5, p_{3} = 0.3$
>     - Output is a $3 \times 5$ matrix
>         - Columns represent 5 generated realizations of $X_{1}, X_{2}, X_{3}$

## Example. Blood Types

- 46% of Canadians have Type O blood
- 42% have Type A
- 9% have Type B
- 3% have Type AB

Suppose 40 people visit a particular blood donation clinic per day to donate blood. Assume it is reasonable to consider these 40 people as a representative sample of Canadians.

Let $X_{1}$ be type O, $X_{2}$ be type A, $X_{3}$ be type B, $X_{4}$ be type AB.

> [!question]+ What is the probability that among the 40 people, 15 have type O blood, 15 have type A blood, 5 have type B blood, and 5 have type AB blood? Calculate this by hand and confirm your answer in R
> $$
> p_{X_{1}, X_{2}, X_{3}, X_{4}}(15, 15, 5, 5) = \frac{40!}{15! \cdot 15! \cdot 5! \cdot 5!} (0.46)^{15} \cdot (0.42)^{15} \cdot (0.09)^{5} \cdot (0.03)^{5}
> $$
> ```r
> dmultinom(x=c(15, 15, 5, 5), prob=c(0.46, 0.42, 0.09, 0.03))
> ```
>
> - Returns `[1] 9.27276e-05`

> [!question]+ What is the probability that there will be 20 type O and 20 type A donors on a randomly selected day?
> $$
> p_{X_{1}, X_{2}, X_{3}, X_{4}}(20, 20, 0, 0) = \frac{40!}{20! \cdot 20! \cdot 0! \cdot 0!} (0.46)^{20} \cdot (0.42)^{20} \cdot (0.09)^{0} \cdot (0.03)^{0}
> $$
>
> ```r
> dmultinom(x=c(20, 20, 0, 0), prob=c(0.46, 0.42, 0.09, 0.03))
> ```
>
> - Returns `0.0007236662`

> [!question]+ What is the probability that there will be at least one type AB donor on a randomly selected day?
>
> - Consider the complement of the event where there are no type AB donors.
> - Let $X$ be the random variable representing the number of type AB donors.
> - We want to find $P(X \geq 1)$, which is equivalent to $1 - P(X = 0)$.
> - Probability of type AB donor is $p_{4} = 0.03$
> - Then:
>   - $P(X = 0) = (1 - p_{4})^n = (1 - 0.03)^{40}$
>   - Therefore, $P(X \geq 1) = 1 - (0.97)^{40} = 0.704$
