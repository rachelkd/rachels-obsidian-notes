---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 9
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "16"
Chapter: 
Slides/Notes:
  - https://q.utoronto.ca/courses/354355/files/34507837?wrap=1
Date: 2024-11-14
Date created: Sun., Nov. 17, 2024, 11:38:40 pm
Date modified: Mon., Nov. 18, 2024, 12:51:02 am
---

# Pareto Distribution

> [!def]+ Pareto Distribution
> A continuous random variable $X$ has a Pareto distribution with location parameter $m$ and shape parameter $a$ if its probability density function (PDF) is:
> $$
> f(x) = \frac{am^a}{x^{a+1}}, \quad \text{for } x \geq m
> $$
> and 0 if $x < m$.
> We denote this $X \sim \text{Par}(m ,a)$, where $m, a > 0$.

- **Origin:**
    - Vilfredo Pareto used it to describe wealth distribution.
- **80-20 Rule:**
    - 20% of the population controls 80% of the wealth.
- **Applications:**
    - Models catastrophic events, where
    - Claim might have a large value
        - (e.g., extreme weather, war)
- **Characteristics:**
    - Right-skewed
    - Heavy long tail

![](https://i.imgur.com/IOwWl4B.png)

## R Implementation

- No built-in Pareto functions in base R.
- Use `EnvStats` package for Pareto functions.

### R Code Example

1. **Install and Load Package**:

   ```r
   install.packages("EnvStats")
   library(EnvStats)
   ```

2. **Plotting Pareto Distribution**:

   ```r
   n <- 100
   m <- 1  # location = 1
   a <- 0.5  # shape = 0.5
   x <- seq(1, 20, length = n)  # equally-spaced sequence from 1 to 20
   y <- dpareto(x, m, a)
   plot(x, y, type = "l")
   abline(h = 0, lty = 2)  # adds dotted line at y = 0
   ```

> [!tip]+ Location `m` and shape `a` parameters should be entered as the second and third arguments
> Define `location=` and `shape=` in functions to avoid errors.

## Properties of Pareto Distribution

> [!thm]+ Mean
> $$ \mu = E(X) = \frac{am}{a-1} \text{ if } a > 1 $$

> [!thm]+ Variance
> $$ \sigma^2 = Var(X) = \frac{am^2}{(a-1)^2(a-2)} \text{ if } a > 2 $$

> [!warning]+ Divergence Conditions
> - If $a \leq 1$:
>     - Mean diverges to infinity
> - If $a \leq 2$:
>     - Variance diverges to infinity
> - $\implies$ Moment generating function does not exist

## CDF, Quantile

> [!thm]+ Cumulative Distribution Function (CDF)
> $$ F(x) = 1 - \left(\frac{m}{x}\right)^a \text{ for } x \geq m $$
> and is 0 otherwise

> [!thm]+ Quantile Function
> The $p^{th}$ quantile (or $100p^{th}$ percentile), denoted as $x_p$ is:
> $$ x_p = m(1-p)^{-\frac{1}{a}} $$

**Derivation:**
1. CDF is the integral of PDF from $-\infty$ to $x$:
$$ F(x) = \int_{m}^{x} \frac{am^a}{t^{a+1}} dt $$
2. Solve using u-substitution:
$$ F(x) = \left[-\frac{m^a}{t^a}\right]_{m}^{x} $$
$$ = -\frac{m^a}{x^a} - (-1) $$
$$ = 1 - \left(\frac{m}{x}\right)^a $$

**Derivation:**
1. By definition, $F(x_p) = p$
2. Substitute CDF:
$$ 1 - \left(\frac{m}{x_p}\right)^a = p $$
3. Solve for $x_p$:
$$ \left(\frac{m}{x_p}\right)^a = 1-p $$
$$ \frac{m}{x_p} = (1-p)^{\frac{1}{a}} $$
$$ x_p = m(1-p)^{-\frac{1}{a}} $$

# Pareto Distribution Exercises

## Exercise 1

> [!question]+ Prove that when $a = m = 1$ then $E(X)$ does not exist (diverges to infinity)
> **Solution:**
> Using the definition of expectation for continuous random variables:
> $$ E(X) = \int_{-\infty}^{\infty} xf(x)dx $$
>
> For Pareto distribution with $a = m = 1$:
> $$ E(X) = \int_{1}^{\infty} x \cdot \frac{1}{x^2}dx $$
> $$ = \int_{1}^{\infty} \frac{1}{x}dx $$
> $$ = \lim_{t \to \infty} [\ln|x|]_{1}^{t} $$
> $$ = \lim_{t \to \infty} [\ln(t) - \ln(1)] $$
> $$ = \lim_{t \to \infty} \ln(t) $$
> $$ = \infty $$
>
> Therefore, $E(X)$ diverges to infinity.

## Exercise 2

> [!question]+ For $a = m = 1$, obtain the $p^{th}$ quantiles where $p = 0.5, 0.75, 0.9, 0.95, 0.99$
>
> **Solution:**
> Using the quantile formula: $x_p = m(1-p)^{-\frac{1}{a}}$
>
> For $a = m = 1$:
> - $p = 0.5$: $x_{0.5} = 1(1-0.5)^{-1} = 2$
> - $p = 0.75$: $x_{0.75} = 1(1-0.75)^{-1} = 4$
> - $p = 0.9$: $x_{0.9} = 1(1-0.9)^{-1} = 10$
> - $p = 0.95$: $x_{0.95} = 1(1-0.95)^{-1} = 20$
> - $p = 0.99$: $x_{0.99} = 1(1-0.99)^{-1} = 100$

## Exercise 3

> [!question]+ Sample $n = 100$ random values from Pareto($m = 1, a = 0.5$)
>
> **Solution:**
> ```r
> library(EnvStats)
> set.seed(123)  # for reproducibility
> sample <- rpareto(100, location = 1, shape = 0.5)
> 
> # Plot histogram
> hist(sample, breaks = 30, main = "Histogram of Pareto Sample")
> 
> # Calculate sample statistics
> sample_mean <- mean(sample)
> sample_var <- var(sample)
> ```
>
> **Analysis:**
> - Theoretical mean and variance don’t exist when $a = 0.5$ (since $a \leq 1$)
> - Sample statistics will be unreliable
> - Improvement: Use larger $a$ values ($a > 2$) for meaningful statistics

## Exercise 4

> [!question]+ Income above \$22,000 has Pareto($a = 2.8, m = 2.2$). Find $P(X > μ + 4σ)$
> Units are $10,000
> **Solution:**
> 1. Calculate $μ$ and $σ$:
> $$ \mu = \frac{2.8 \cdot 2.2}{2.8-1} = 3.44 $$
> $$ \sigma = \sqrt{\frac{2.8 \cdot (2.2)^2}{(2.8-1)^2(2.8-2)}} = 2.89 $$
>
> 2. Find $\mu + 4\sigma = 3.44 + 4(2.89) = 15.00$
>
> 3. Use CDF:
> $$ P(X > 15) = 1 - P(X \leq 15) = \left(\frac{2.2}{15}\right)^{2.8} = 0.0016 $$

## Exercise 5

> [!question]+ Auto insurance losses follow Pareto($a = 4$, $m = 800$). Find P(X > 1000)
>
> **Solution:**
> Using the survival function:
> $$ P(X > 1000) = 1 - P(X \leq 1000) = \left(\frac{800}{1000}\right)^4 = (0.8)^4 = 0.4096 $$
> Therefore, about 41% of policies exceed $1000 in losses.

## Exercise 6

> [!question]+ Income follows Pareto($a = 3$, $m = 1000$). Find P(2000 < X < 4000)
>
> **Solution:**
> $$ P(2000 < X < 4000) = F(4000) - F(2000) $$
> $$ = \left[1-\left(\frac{1000}{4000}\right)^3\right] - \left[1-\left(\frac{1000}{2000}\right)^3\right] $$
> $$ = \left[1-\left(\frac{1}{4}\right)^3\right] - \left[1-\left(\frac{1}{2}\right)^3\right] $$
> $$ = (1-0.0156) - (1-0.125) = 0.1094 $$
> Therefore, about 10.94% of the population has income between $2000 and $4000.
