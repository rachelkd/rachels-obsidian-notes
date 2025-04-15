---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 6
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "11"
Chapter: "6.1"
Slides/Notes:
  - https://share.goodnotes.com/s/9hdjqKlwwy2TsFn48fxMua
Date: 2024-10-10
Date created: Thu., Oct. 17, 2024, 2:19:50 am
Date modified: Fri., Dec. 6, 2024, 1:19:18 am
---

# Discrete vs. Continuous Random Variables

![](https://i.imgur.com/CG7LZKg.png)

# What is a Continuous Random Variable?

> [!def]+ Continuous random variable
> - [[Random Variables|Random variable]] that can take on an **uncountably infinite** number of possible values

- e.g., $X$ = Time taken in hours to complete a particular task
    - **Support** of $X$ is $x > 0$
- e.g., $X$ = Time in minutes you wait for a bus to arrive, where buses arrive every 15 minutes and are always on time
    - **Support** of $X$ is $x \in [0, 15]$

# Probability Density Functions (pdf)

> [!def]+ Probability density function (pdf)
> Let $X$ be a continuous random variable.
> A function $f$ is a probability density function (pdf) of $X$ if
> $$\begin{align*} f(x) \geq 0 \text{ for all } x \in \mathbb{R} \tag{1} \\ \int_{-\infty}^{\infty} f(x)dx = 1 \tag{2} \\ \text{For } S \subseteq \mathbb{R}, P(X \in S) = \int_{S} f(x)dx \tag{3} \end{align*}$$
> - The **support** of a random variable is (informally):
>     - The region on where the PDF is non-zero
>     - Support of the RV are the values where it makes sense
>         - e.g., Cannot be negative height, task can’t take negative time, etc.
>     - Many continuous RVs have support $x \in (-\infty, \infty)$
>     - Often, will only define PDF on support of $X$
>         - Assumed to be 0 elsewhere

^6677b0

| Discrete RV                          | Continuous RV                                                                        |
| ------------------------------------ | ------------------------------------------------------------------------------------ |
| $$\sum_{\text{all }X} p(x) = 1$$<br> | $$\int_{-\infty}^{\infty} f(x)dx = 1$$                                               |
| $0 \leq p(x) \leq 1$ for all $x$     | $f(x) \geq 0$ for all $x$                                                            |
| $P(X \in S) = \sum_{x \in S} p(x)$   | $P(X \in S) = \int_{S} f(x)dx$                                                       |
| $p(x)$ represents $P(X = x)$         | $f(x)$ represents the *height* of the density function, and ==is not a probability== |

- Whenever we had a sum for discrete RVs, we now have an *integral* for continuous RVs

![](https://i.imgur.com/fx3szMA.png)

# Probabilities as Areas

- $X$ is a random variable with PDF $f(x) = \frac{3x^{2}}{16}$, for $-2 < x < 2$ (support)
    - The support of $X$ is $(-2, 2)$, and $f(x) = 0$ everywhere else
- Total area under $f(x)$ is 1
    - $$\int_{-\infty}^{\infty} f(x)dx = \int_{-2}^{2} \frac{3x^{2}}{16} dx = 1$$
- If we are interested in the probability of $X > 1$, we look at the ==area under the curve corresponding with $X > 1$==
    - $P(X > 1) = \int_{1}^{\infty} f(x)dx = \int_{1}^{2} \frac{3x^{2}}{16} dx = 0.4375$
    - Area of the shaded area in the plot of CDF

![](https://i.imgur.com/SImYM22.png)

- What if we are interested in the probability of $X = 1$?
    - $P(X = 1) = \int_{1}^{1} f(x)dx = \int_{1}^{1} \frac{3x^{2}}{16} dx = 0$

> [!important] For continuous random variables, $P(X = x) = 0$ for all $X$

- We *must* work with a range for $X$ to get meaningful probabilities

# Inequalities with Continuous RVs

For discrete random variables, we must be ==careful about writing ≤ vs. < or ≥ vs. ≥==.
- There are less than 4 customers $\neq$ There are 4 or less customers
- There are less than 4 customers $=$ There are 3 or less customers

> [!important]+ For continuous random variables, since $P(X = x) = 0$ for all $x$:
> - $P(a \leq X \leq b) = P(X \in [a, b])$
> - $P(a < X \leq b) = P(X \in (a, b])$
> - $P(a \leq X < b) = P(X \in [a, b))$
> - $P(a < X < b) = P(X \in (a, b))$
>
> ==are all *equivalent*==!
