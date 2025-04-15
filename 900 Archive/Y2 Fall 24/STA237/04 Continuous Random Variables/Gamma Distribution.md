---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 9
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "15"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/IJgTcGqAZl4SKDLIfYpPM4
Date: 2024-11-05
Date created: Sun., Nov. 10, 2024, 3:01:53 am
Date modified: Sun., Nov. 10, 2024, 4:06:39 pm
---

# Gamma Distribution

> How long do I need to wait for $\alpha$ events to happen?

- Gamma distribution is an extension of the [[Exponential Distribution]] that models the ==time until multiple events occur==.

# Sums of I.I.D Random Variables

Several important distributions arise from sums of independent and identically distributed (i.i.d.) random variables:

> [!thm]+ Sum of i.i.d. Bernoulli
> If $X = X_{1} + \dots + X_{n}$ where the $X_{i}$‘s are i.i.d. $\text{Ber}(p)$, then $X \sim \text{Binom}(n, p)$
> - Represents the number of successes within $n$ trials

> [!thm]+ Sum of i.i.d. Geometric
> If $X = X_{1} + \dots + X_{r}$ where the $X_{i}$’s are i.i.d. $\text{Geo}(p)$, then $X \sim \text{NegBin}(r, p)$
> - Represents the number of trials needed to get $r$ successes

> [!thm]+ Sum of i.i.d. Exponential
> If $X = X_{1} + \dots + X_{\alpha}$ where the $X_{i}$‘s are i.i.d. $\text{Exp}(\lambda)$, then $X \sim \text{Gamma}(\alpha, \lambda)$
> - Represents the time until the $\alpha^{th}$ event occurs
> - However, $\alpha$ does *not* need to be an integer

# Definition of Gamma Distribution

> [!def]+ Gamma Distribution
> A random variable $X$ has a **Gamma distribution** with parameters $\alpha > 0$ and $\lambda > 0$ if the density function of $X$ is
> $$f(x) = \frac{\lambda^\alpha x^{\alpha-1}e^{-\lambda x}}{\Gamma(\alpha)}, \quad \text{for } x > 0$$
> where
> $$\Gamma(\alpha) = \int_0^\infty t^{\alpha-1}e^{-t} dt$$
>
> We write: $X \sim \text{Gamma}(\alpha, \lambda)$

## Parameters

- $\alpha$: Shape parameter
    - If $\alpha = 1$: $\text{Gamma}(\alpha, \lambda) \equiv \text{Exp}(\lambda)$
- $\lambda$: Rate parameter

## Gamma Function Properties

- $\Gamma(a)$ is called the **Gamma function**, with the following properties:
- For $a > 1$:
    - $\Gamma(a) = (a-1)\Gamma(a-1)$
- If $a$ is a positive integer:
    - $\Gamma(a) = (a-1)!$
- $\Gamma(1) = 1$
- $\Gamma(1/2) = \sqrt{\pi}$

# Expectation, Variance, and MGF

> [!thm]+ Expectation
> $$E[X] = \frac{\alpha}{\lambda}$$

> [!thm]+ Variance
> $$\text{Var}[X] = \frac{\alpha}{\lambda^2}$$

> [!thm]+ Moment Generating Function
> $$M_X(t) = \left(\frac{\lambda}{\lambda-t}\right)^\alpha \quad \text{for } t < \lambda$$

Similar to that of [[Exponential Distribution#Properties|exponential distribution]], except with the $\alpha$ value.

# Gamma PDF

![](https://i.imgur.com/l6vUF1o.png)

- We call $\alpha$ the **shape parameter** because $\alpha$ changes the shape of the PDF

# R Commands for Gamma Distribution

For a Gamma distribution with shape parameter $\alpha$ and rate parameter $\lambda$:

- $f(x)$ (PDF):
    - `dgamma(x, α, λ)`
- $F(x)$ (CDF):
    - `pgamma(x, α, λ)`
- $F^{-1}(p)$ (Quantile function):
    - `qgamma(p, α, λ)`
- Generate $k$ independent observations:
    - `rgamma(k, α, λ)`

```r title:Examples
# PDF at x = 0.5
dgamma(0.5, 1, 5)
[1] 0.410425

# CDF at x = 0.5
pgamma(0.5, 1, 5)
[1] 0.917915

# Note: When α = 1, Gamma(α,λ) ≡ Exp(λ)
# This is why these give the same results:
dexp(0.5, 5)
[1] 0.410425

pexp(0.5, 5)
[1] 0.917915
```

# Example. Emergency Vehicles

- On average, 0.3 emergency vehicles with their sirens on pass by Gracia’s apartment every hour during the day
- $\implies \lambda = 0.3$ per hour = rate that the vehicles pass by
- & The time in hours until the *third* vehicle follows a $\text{Gamma}(3, 0.3)$
    - $\alpha = 3, \lambda = 0.3$

> [!question]+ What is the probability that we see 3 emergency vehicles before the 8-hour workday is over?
> - $\equiv$ The time to 3 events is less than 8 hours $\equiv P(X < 8)$
> ```r
> pgamma(8, 3, 0.3)
> [1] 0.4302913
> ```

# Example. Computer Warranty

- The time in months for your computer to fail follows a gamma distribution with mean 24 months and standard deviation 6 months
    - Assume all months are equal length

> [!question]+ Compute the rate and shape parameters for this gamma distribution.
> - Givens:
>     - $E[X] = 24 = \frac{\alpha}{\lambda}$
>     - $Var[X] = 6^{2} = 36 = \frac{\alpha}{\lambda^{2}}$
>
> Therefore:
> $$\lambda = \frac{24}{36} = \frac{2}{3}$$
>
> Substituting back into $E[X]$ equation:
> $$24 = \frac{\alpha}{\frac{2}{3}}$$
> $$\alpha = 24 \cdot \frac{2}{3} = 16$$
>
> Thus:
> - Shape parameter: $\alpha = 16$
> - Rate parameter: $\lambda = \frac{2}{3}$

> [!question]+ The company wants to offer a warranty for the computers. They want to make the cutoff for the warranty period such that only 20% of users will need to use it. What should this cutoff be?
> ![|center|400](https://i.imgur.com/C2tbF2G.png)
>
> > [!note]- R Code for Graph
> >
> > ```r
> > # Plotting the density curve
> > curve(dgamma(x, 16, 2/3),
> >       from = -4, to = 60,
> >       n = 10000,
> >       col = "darkblue",
> >       lwd = 2,
> >       xaxt = "n",
> >       ylab = "Density",
> >       main = "Gamma(16, 2/3) PDF")
> > ```
>
> From question above, $X \sim \text{Gamma}\left( 16, \frac{2}{3} \right)$
>
> We want to find the time $t$ where $P(X < x) = 0.2$
> - This means 20% of computers will fail before time $x$
>
> We can find this using the quantile function:
> ```r
> qgamma(0.2, 16, 2/3)
> [1] 18.86084
> ```
>
> Therefore, the warranty period should be approximately 19 months.
>
> The graph shows:
> - The PDF of the Gamma(16, 2/3) distribution
> - The shaded area represents the 20% of computers that will fail before the warranty period
> - The cutoff point at approximately 19 months
