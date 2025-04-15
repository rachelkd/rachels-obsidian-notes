---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 8
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "13"
  - "14"
Chapter: 6.2-4
Slides/Notes:
  - https://share.goodnotes.com/s/5O07xen8EixNbSCGgzvIwZ
Date: 2024-10-22
Date created: Wed., Oct. 30, 2024, 1:19:43 am
Date modified: Wed., Dec. 11, 2024, 5:26:53 pm
---

# Normal Distribution

- Normal distribution has **support** on $(-\infty, \infty)$
- Has a PDF that is *bell-shaped*, *symmetric*, *unimodal*
- Defined by its mean and variance
    - & We write $X \sim N(\mu, \sigma^2)$
        - $\mu =$ **mean**
        - $\sigma^{2} =$ **variance**
    - Some sources write $X \sim N(\mu, \sigma)$ where $\sigma$ is the **standard deviation**
- Many applications in statistics and probability theory
    - “Bell curve,” “curving grades,” “standard curve,” and similar terms all refer to the normal distribution

> [!def]+ Normal Distribution
> A random variable $X$ has the **normal distribution with parameters $\mu$ and $\sigma^{2}$**, if the density function of $X$ is
> $$f(x) = \frac{1}{\sigma \sqrt{ 2\pi }} e ^{-\frac{(x-\mu)^{2}}{2\sigma^{2}}}, \quad -\infty<x<\infty$$
> We write $X \sim \text{Norm}(\mu, \sigma^{2})$ or $N(\mu, \sigma^{2})$

![|center|400](https://i.imgur.com/lCfhjmz.png)

> [!thm]- Expectation
> $$E[X] = \mu$$

> [!thm]- Variance
> $$Var[X] = \sigma^{2}$$

> [!thm]- MGF
> $$M_{X}(t) = e^{\mu t + \frac{\sigma^{2}t^2}{2}}$$
> - If a RV has a MGF that looks like this, it is **normally distributed**

- No closed form for CDF
    - Use R or normal tables

# Normal Distribution on R

> [!note] R takes $\sigma$ as the argument, not $\sigma^{2}$

- $f(x)$
    - `dnorm(x, μ, σ)`
- $F(x)$
    - `pnorm(x, μ, σ)`
- Determine a quantile $x_{p}$ such that $F(x_{p}) = p$: $F^{-1}(p)$
    - `qnorm(p, μ, σ)
- Generate $k$ independent observations of $X$
    - `rnorm(k, μ, σ)

```r title:Examples
# PDF at x = 3
dnorm(3, mean = 5, sd = 2)
[1] 0.1209854

# CDF at x = 3
pnorm(3, mean = 5, sd = 2)
[1] 0.1586553

# 15th percentile
qnorm(0.15, mean = 5, sd = 2)
[1] 2.927133

# Generate 5 random values
rnorm(5, mean = 5, sd = 2)
[1] 7.236292 3.666825 5.494397 2.698559 2.699452
```

# Shape of the Normal Distribution PDF

- Shape of the normal distribution is determined by both its
    - **centre** (mean)
    - **spread** (variance or standard deviation)
- Distribution is always
    - ==bell-shaped==
    - ==symmetric about its mean== i.e., creates a mirror image around the mean
- Mean is a *location* parameter
    - → If only the mean changes, the distribution is shifted to the left or to the right
    - Shape stays the same
- Variance is a *scale* parameter
    - → As variance changes, shape of distribution changes
    - Larger variance $\iff$ Wider and flatter distribution

## Effect of the Mean ($\mu$)

![](https://i.imgur.com/eZtUMEK.png)

## Effect of Variance ($\sigma^{2}$)

![](https://i.imgur.com/ATJRq5K.png)

![](https://i.imgur.com/aUkxqYk.png)

# Example. Birth Weight

Suppose the birth weight of an infant follows a normal distribution with mean 120 ounces and standard deviation 18 ounces.

> [!note]+ For any normal distribution, the area under $f(x)$
> - Between $\mu - \sigma$ and $\mu + \sigma$ is about 0.68
> - Between $\mu \pm 2\sigma$ is about 0.95
> - Between $\mu \pm 3\sigma$ is about 0.997
> $$X \sim N(\mu, \sigma^{2}) \implies P(\mu - \sigma \leq X \leq \mu + \sigma) = 0.68$$
> ![|center|400](https://i.imgur.com/bEwq5Sb.png)

> [!question]+ Sketch the PDF that corresponds with this distribution
> ![|center|400](https://i.imgur.com/e5jWwQt.png)

> [!question]+ What R code do we need to solve the following?
>
> > [!question]- What percent of infants weight less than 96 ounces?
> > - Want $P(X < 96)$, $X \sim N(120, \sigma^{2} = 18^{2})$
> > ```r
> > pnorm(96, 120, 18)
> > [1] 0.09121122
> > ```
>
> > [!question]- What percent of infants weigh between 128 and 144 ounces?
> > - Want $P(128 < X < 144)$ = $P(X < 144) - P(X < 128)$
> > ```r
> > pnorm(144, 120, 18) - pnorm(128, 120, 18)
> > [1] 0.2371494
> > ```
>
> > [!question]- What is the first quartile (25th percentile) of birth weight?
> > - Want $P(X < q_{0.25}) = 0.25$, find $q_{0.25}$
> > ```r
> > qnorm(0.25, 120, 18)
> > [1] 107.8592
> > ```

# Standard Normal Distribution

> [!def]+ Standard Normal Distribution
> If a random variable follows a normal distribution with *mean 0* and *variance 1*, we say that it follows the **standard normal distribution**
> $$Z \sim N(0, 1)$$

- This is where the term **z-score** comes from
- If $X \sim N(\mu, \sigma^{2})$ and $Z = \frac{X-\mu}{\sigma}$, then $Z \sim N(0, 1)$
- For any normal random variable $X \sim N(\mu, \sigma^{2})$:
    $$P(a < X < b) = P\left(\frac{a-\mu}{\sigma} < \frac{X-\mu}{\sigma} < \frac{b-\mu}{\sigma}\right) = P\left(\frac{a-\mu}{\sigma} < Z < \frac{b-\mu}{\sigma}\right)$$
    where $Z \sim N(0,1)$
- This standardization is useful because:
    - Makes it easier to calculate probabilities when using normal tables
    - Particularly important when we don’t have access to R and need to use standard normal tables

## Example: Birth Weight (Using Standard Normal)

> [!question]- What percent of infants weigh less than 96 ounces?
> - Want $P(X < 96)$ = $P\left(\frac{X-120}{18} < \frac{96-120}{18}\right)$ = $P(Z < -1.33)$
> ```r
> pnorm(-1.33)
> [1] 0.0917914
> ```

> [!question]- What percent of infants weigh between 128 and 144 ounces?
> - Want $P(128 < X < 144)$ = $P\left(\frac{128-120}{18} < Z < \frac{144-120}{18}\right)$ = $P(0.44 < Z < 1.33)$
> ```r
> pnorm(1.33) - pnorm(0.44)
> [1] 0.2382094
> ```

> [!question]- What is the first quartile (25th percentile) of birth weight?
> - Want $q_{0.25}$ = $\mu + \sigma \cdot z_{0.25}$
> ```r
> qnorm(0.25)*18 + 120
> [1] 107.8592
> ```

Note: This method gives the same results as before, but uses the standard normal distribution ($Z \sim N(0,1)$) instead of working directly with $X \sim N(120, 18^2)$.

# Symmetry of the Normal Distribution

> [!thm]+ Symmetry Property
> If $Z \sim N(0,1)$, then $F(z) = 1 - F(-z)$ for any $z \in \mathbb{R}$

```r
# Left tail probability
pnorm(-1.65, mean=0, sd=1)
[1] 0.0494714

# Right tail probability
pnorm(1.65, mean=0, sd=1)
[1] 0.9505285

# Finding z-values for symmetric probabilities
qnorm(0.0495, mean=0, sd=1)
[1] -1.649721

qnorm(0.9505, mean=0, sd=1)
[1] 1.649721
```

![|center|400](https://i.imgur.com/vp8V7Gy.png)

*The symmetry property shows that approximately 5% of the distribution lies in each tail beyond ±1.65 standard deviations from the mean.*

# Properties of Standard Normal Distribution

> [!note]+ Special Notation for Standard Normal Notation
> For the standard normal distribution ($Z \sim N(0,1)$),
> - $\phi(t)$ denotes the PDF of the standard normal
> - $\Phi(x)$ denotes the CDF of the standard normal, where:
> $$\Phi(x) = F(x) = \int_{-\infty}^x \phi(t)dt = \int_{-\infty}^x \frac{1}{\sqrt{2\pi}} e^{-\frac{t^2}{2}} dt$$

## Symmetry Properties

For $Z \sim N(0,1)$:

- $F(z) = 1 - F(-z)$ for any $z \in \mathbb{R}$
- $P(Z > z) = P(Z < -z)$ for any $z \in \mathbb{R}$
- $P(Z < z) = P(Z > -z)$ for any $z \in \mathbb{R}$

## Converting Between Normal and Standard Normal

- If $X \sim N(\mu, \sigma^2)$, we can convert to standard normal using:
    $$Z = \frac{X-\mu}{\sigma} \sim N(0,1)$$

- This allows us to convert probability statements:
    $$P(X < a) = P\left(\frac{X-\mu}{\sigma} < \frac{a-\mu}{\sigma}\right) = P\left(Z < \frac{a-\mu}{\sigma}\right)$$

## Finding Values from Percentiles

If we know that $q\%$ of the data lies below some value $b$:
1. First write the probability statement:
    $$q\% = P(X < b) = P\left(\frac{X-\mu}{\sigma} < \frac{b-\mu}{\sigma}\right) = P\left(Z < \frac{b-\mu}{\sigma}\right)$$
2. Then solve for $b$:
    $$b = \mu + \sigma \cdot z_{q}$$
    where $z_{q}$ is the $q$-th percentile of the standard normal distribution

# Standard Normal Tables

- Given a $z \in \mathbb{R}$, we can ==find $P(Z < z) = \Phi(z)$== by looking up $z$ on the margins of the table and getting the probability from the middle
    - ![|400](https://i.imgur.com/ofVArcP.png)

- Some tables only have positive values of $z$ → Use the **symmetry property**
    - If you need to find $P(Z< z)$ for $z < 0$
        - $P(Z < z) = P(Z > -z) = 1 - P(Z > z) = 1 - P(Z < -z) = 1 - \Phi(-z)$

# Example. Video Games

Suppose the time Gracia spends playing video games every week can be modelled by the random variable $X$ has a normal distribution with a mean of $\mu = 120$ minutes and a standard deviation of $\sigma = 20$ minutes.

Let $X$ be the number of minutes played. $X \sim N(120, \sigma^{2} = 400)$

> [!question]+ (a) What is the probability that Gracia plays for less than 105 minutes?
> $P(X < 105) =$ `pnorm(105, 120, 20)`
> $= 0.2266$
> ![|center|300](https://i.imgur.com/oPAWU7T.png)
> $P\left( Z < \frac{{105-120}}{20} \right) =$ `pnorm(-0.75)`
> $= 0.2266$
> ![|center|300](https://i.imgur.com/VzRbfeo.png)

> [!question]+ (b) What is the probability that Gracia plays between 92 and 108 minutes?
> $P(92 < X < 108) = P(X < 108) - P(X < 92)$
> $=$ `pnorm(108, 120, 20) - pnorm(92, 120, 20)`
> $= 0.2743 - 0.0808 = 0.1935$
> ![|center|300](https://i.imgur.com/GyC5n5s.png)
> $P\left( \frac{{92-120}}{20} < Z < \frac{{108-120}}{20} \right) = P(Z < -0.6) - P(Z < -1.4)$

> [!question]+ (c) What is the probability that Gracia plays for exactly 84 minutes?
> $P(X = 84) = 0$ because $X$ is continuous

> [!question]+ (d) Given that Gracia has already played for 92 minutes, what is the probability she plays for less than 108 minutes?
> $$\begin{align*}
> P(X < 108 \mid X > 92) &= \frac{P(92 < X < 108)}{P(X > 92)} \\
> &= \frac{0.1935}{1 - P(X < 92)} \\
> &= \frac{0.1935}{1 - 0.0808} \\
> &= \frac{0.1935}{0.9192} \\
> &\approx 0.21
> \end{align*}$$

> [!question]+ (e) To get an achievement in the game, Gracia must play for at least 105 minutes every week. If she plays for 5, independent weeks, what is the probability that she gets the achievement in all 5 weeks? What distributions does this question use?
> $$Y \sim \text{Binom}(n = 5, p = P(X > 105))$$
> $$p = 1 - P(X < 105) = 1 - 0.2266 = 0.7734 \quad \text{from (a)}$$
> $$\begin{align*} 
> P(Y = 5) &= \binom{5}{5} 0.7734^{5} (1-0.7734)^{0} \\
> &= 1 \cdot 0.7734^{5} \cdot 1 \\
> &\approx 0.2767
> \end{align*}$$
> ```r
> p <- 1 - pnorm(105, 120, 20)
> dbinom(5, size = 5, prob=p)
> [1] 0.2766585
> ```

> [!question]+ (f) To get an achievement in the game, Gracia must play for at least 105 minutes every week. Gracia wants to get an achievement 90% of the time and plans to do so by decreasing the standard deviation of her gaming time while keeping the mean the same. What standard deviation must she achieve?
> Let $X \sim N(120, \sigma^2)$ where $\sigma$ is unknown
>
> Want: $P(X > 105) = 0.9$
>
> This means: $P(X < 105) = 0.1$
>
> Converting to standard normal:
> $$P\left(Z < \frac{105-120}{\sigma}\right) = 0.1$$
>
> Using `qnorm()`:
> $$\frac{105-120}{\sigma} = \text{qnorm}(0.1)$$
>
> Solving for $\sigma$:
> $$\sigma = \frac{105-120}{\text{qnorm}(0.1)} = \frac{-15}{-1.28} \approx 11.7$$
>
> Therefore, Gracia needs to reduce her standard deviation to approximately 11.7 minutes to achieve her goal.

> [!question]+ (g) What is the 75th percentile of the amount of time Gracia plays?
> Method 1: Direct using `qnorm()`
> ```r
> qnorm(0.75, 120, 20)
> [1] 133.5102
> ```
>
> Method 2: Using standard normal and converting
> ```r
> qnorm(0.75) * 20 + 120  # μ + σz₀.₇₅
> [1] 133.5102
> ```
>
> Therefore, 75% of the time Gracia plays less than 133.5 minutes.
