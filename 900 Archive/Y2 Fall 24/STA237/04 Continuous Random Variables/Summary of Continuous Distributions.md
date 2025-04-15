---
dg-publish: false
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
  - https://share.goodnotes.com/s/IJgTcGqAZl4SKDLIfYpPM4
  - https://share.goodnotes.com/s/rFkXloK4CsBIqmMjfIpoxe
Date: 2024-11-07
Date created: Tue., Nov. 12, 2024, 1:51:26 pm
Date modified: Fri., Nov. 22, 2024, 10:20:28 pm
---

# Summary of Continuous Distributions

### Uniform Distribution

If $X \sim \text{Unif}(a,b)$:
- Support: $[a,b]$
- PDF: $f(x) = \begin{cases} \frac{1}{b-a}, & a \leq x \leq b \\ 0, & \text{otherwise} \end{cases}$
- CDF: $F(x) = \begin{cases} 0, & x < a \\ \frac{x-a}{b-a}, & a \leq x \leq b \\ 1, & x > b \end{cases}$
- $E[X] = \frac{a+b}{2}$
- $\text{Var}[X] = \frac{(b-a)^2}{12}$
- MGF: $M_X(t) = \begin{cases} \frac{e^{tb}-e^{ta}}{t(b-a)} & \text{if } t \neq 0 \\ 1 & \text{if } t = 0 \end{cases}$

### Normal Distribution

If $X \sim N(\mu,\sigma^2)$:
- Support: $(-\infty,\infty)$
- PDF: $f(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
- CDF: No closed form (use R or tables)
- $E[X] = \mu$
- $\text{Var}[X] = \sigma^2$
- MGF: $M_X(t) = e^{\mu t + (\sigma^2t^2)/2}$

### Exponential Distribution

- Models the time *until* the first event or between events

If $X \sim \text{Exp}(\lambda)$:
- Support: $[0,\infty)$
- PDF: $f(x) = \lambda e^{-\lambda x}$ for $x \geq 0$
- CDF: $F(x) = 1-e^{-\lambda x}$ for $x \geq 0$
- $E[X] = \frac{1}{\lambda}$
- $\text{Var}[X] = \frac{1}{\lambda^2}$
- MGF: $M_X(t) = \frac{\lambda}{\lambda-t}$ for $t < \lambda$

### Gamma Distribution

- Models the time until $\alpha$ events happen

If $X \sim \text{Gamma}(\alpha,\lambda)$:
- Support: $[0,\infty)$
- PDF: $f(x) = \frac{\lambda^\alpha x^{\alpha-1}e^{-\lambda x}}{\Gamma(\alpha)}$ for $x \geq 0$
- $E[X] = \frac{\alpha}{\lambda}$
- $\text{Var}[X] = \frac{\alpha}{\lambda^2}$
- MGF: $M_X(t) = \left(\frac{\lambda}{\lambda-t}\right)^\alpha$ for $t < \lambda$
- Note: When $\alpha = 1$, reduces to $\text{Exp}(\lambda)$

> [!info]+ Exponential and Gamma distributions
> - Number of events that occur follow a [[Poisson Distribution]] with rate $\lambda$
>
> > [!summary]+ Assumptions
> > - Same as Poisson distribution
> > - ==Independent non-overlapping intervals==
> > - ==Individuality of events== (no two events at the same time)
> > - ==Uniformity== (events occur at a uniform rate)
>
> - Time to first event (or time in between events) follows $\text{Exp}(\lambda)$
> - Time to $\alpha$-th event follows $\text{Gamma}(\alpha, \lambda)$
> - Can also be used to model distances, etc.
>     - Can be used in *any* setup where the number of events follow the Poisson distribution
>     - e.g., Time you wait for a bus, distance on the road until a pothole, length of fabric until a defect
> - Exponential distribution can also be thought of as the *limiting distribution* of the Geometric
>     - Similar to how Poisson is the *limiting distribution* of the Binomial

# Alternative Parameterization

- In [[Exponential Distribution]] and [[Gamma Distribution]], we *parameterize* these distributions with rate parameter $\lambda$
- Other textbooks and sources ==may use the **scale** parameter $\frac{1}{\lambda}$==
- In R: Can specify either rate or scale for the ==Gamma== distribution
    - Only ==rate parameter for Exponential==

```r
dexp(x, rate = 1, log = FALSE)
```

For $X \sim \text{Gamma}(\alpha = 3, \lambda = 0.3)$,

```r
dgamma(x, shape, rate = 1, scale = 1/rate, log = FALSE)
```

What is $P(X < 10)$?

```r
pgamma(10, 3, 0.3)
[1] 0.5768099

pgamma(10, shape = 3, rate = 0.3)
[1] 0.5768099

pgamma(10, shape = 3, scale = 1/0.3)
[1] 0.5768099
```
