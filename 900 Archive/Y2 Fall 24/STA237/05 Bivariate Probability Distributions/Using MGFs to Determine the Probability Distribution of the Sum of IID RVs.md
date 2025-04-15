---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 11
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/JvMEYIPHcqWjrtPBX0XusL
Date: 2024-11-21
Date created: Sun., Nov. 24, 2024, 2:21:59 am
Date modified: Fri., Dec. 6, 2024, 6:42:58 pm
---

# Review

- $m_{X}(t) = E(e^{tX})$
    - The mgf exists if $m_{X}(t)$ is defined for an interval of $t$ around 0
    - i.e., For $-h < 0 < +h$, where $h$ is a positive number
- $E(X^{k}) = \frac{d^{k}}{dt^{k}} m_{X}(t) \big|_{t=0} = m_{X}^{(k)}(0)$
- For independent RVs $X$ and $Y$, $m_{X + Y}(t) = m_{X}(t) m_{Y}(t)$
- For an RV $X$ and constants $a$ and $b$, $m_{aX + b}(t) = e^{tb}m_{X}(at)$
- If two RVs have the same MGF, then they have the same distribution

# Distributions of Sums of IID RVs

- Recall:
    - Binomial: Sum of i.i.d. Bernoulli RVs
    - Negative: Sum of i.i.d. Geometric RVs
    - Gamma: Sum of i.i.d. Exponential RVs
- We can show this by using the fact that ==the MGF uniquely specifies a distribution==

# Distributions of Sums of Independent RVs

## The Sum of Independent Poisson RVs is Poisson

- Let $Y_{1}$ and $Y_{2}$ be independent [[Poisson Distribution|Poisson]] random variables with rate parameters $\lambda_{1}$ and $\lambda_{2}$
- What can $Z + Y_{1} + Y_{2}$ be used to model?

> [!question]+ Find the distribution of $Y_{1} + Y_{2}$ using MGFs
> If $X \sim Pois(\lambda)$,
> - PMF: $\frac{e^{-\lambda}\lambda^{x}}{x!}$
> - MGF: $e^{\lambda(e^{t} - 1)}$
>
> $$
> \begin{align*}
> m_{Z}(t) = m_{Y_{1} + Y_{2}}(t) &= m_{Y_{1}}(t)m_{Y_{2}}(t) \\
> &= e^{\lambda_{1} (e^{t} - 1)}e^{\lambda_{2}(e^{t} - 1)} \\
> &= e^{\lambda_{1}(e^{t} - 1) + \lambda_{2}(e^{t} - 1)} \\
> &= e^{(\lambda_{1} + \lambda_{2})(e^{t} - 1)}
> \end{align*}
> $$
>
> > [!obs] $Z \sim Pois(\lambda_{1} + \lambda_{2})$

> [!obs]+ The sum of independent Poisson RVs is Poisson
> ![](https://i.imgur.com/G823qYs.png)
>
> ```r
> set.seed(12345)
> 
> X = rpois(10000, lambda = 5)
> Y = rpois(10000, lambda = 10)
> Z = X + Y
> 
> W = rpois(10000, lambda = 15)
> 
> par(mfrow = c(1, 2))  # Put two plots side by side
> hist(Z, breaks = 20, freq = FALSE, main = "Histogram of Z = X + Y")
> hist(W, breaks = 20, freq = FALSE, main = "Histogram of W ~ Pois(15)")
> ```

## The Sum of Independent Normal RVs is Normal

- Let $Y_{1}$ and $Y_{2}$ be independent [[Normal Distribution|Normal]] random variables with $Y_{1} \sim N(\mu_{1}, \sigma_{1}^{2})$ and $Y_{2} \sim N(\mu_{2}, \sigma_{2}^{2})$
- Find the distribution of $Z = Y_{1} + Y_{2}$ using MGFs

Recall: $X \sim N(\mu, \sigma^{2}) \iff M_{X}(t) = e^{\mu t + \frac{\sigma^{2}t^{2}}{2}}$

> [!question]+ Find the distribution of $Z = Y_{1} + Y_{2}$ using MGFs
>
> $$
> \begin{align*}
> M_{Z}(t) = M_{Y_{1} + Y_{2}}(t) &= M_{Y_{1}}(t)M_{Y_{2}}(t) \\
> &= e^{\mu_{1}t + \frac{\sigma_{1}^{2}t^{2}}{2}}e^{\mu_{2}t + \frac{\sigma_{2}^{2}t^{2}{2}}{2}} \\
> &= e^{(\mu_{1} + \mu_{2})t + \frac{(\sigma_{1}^{2} + \sigma_{2}^{2})t^{2}}{2}}
> \end{align*}
> $$
>
> > [!obs] $Z \sim N(\mu_{1} + \mu_{2}, \text{variance} = \sigma_{1}^{2} + \sigma_{2}^{2})$

> [!obs]+ The sum of independent Normal RVs is Normal
> ![](https://i.imgur.com/RHlQjfP.png)
