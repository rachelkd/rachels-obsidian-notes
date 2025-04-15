---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 6
Module:
  - "[[3 - Discrete Random Variables]]"
Lecture:
  - "11"
Chapter: 
Slides/Notes: 
Date: 2024-10-10
Date created: Thu., Oct. 17, 2024, 1:25:52 am
Date modified: Sun., Nov. 10, 2024, 3:33:26 pm
---

# A Preview

**Example. Roll Up the Rim**

- Each Tim Horton’s drink you buy has a ⅙ chance of winning a drink or donut.
- You buy drinks until you win *two* drink/donut prizes
    - $\underbrace{FFF\dots FS}_{X_{1} \sim \text{Geo}\left( \frac{1}{6} \right)}\underbrace{FF\dots FS}_{X_{2} \sim \text{Geo}\left( \frac{1}{6} \right)}$

> [!note]+ We can think of the negative geometric distribution as the ==sum of multiple *independent* geometric== random variables
> $$\overbrace{\underbrace{00\dots01}_{X_{1}}\underbrace{00\dots01}_{X_{2}} \underbrace{00\dots01}_{X_{n}} }^{X}$$

# From Geometric to Negative Binomial

- $X \sim \text{NegBin}(r, p)$ if it counts the number of trials until the $r$-th success
- $X = X_{1} + \dots + X_{r}$, where the $X_{i}$ are i.i.d. $\text{Geo}(p)$ RVs
    - Independent and identically distributed

> [!important]+ We now also need to ==consider the position of the successes in our sequence of failures and successes==
> - Final trial must be the $r$-th success
> - Other $r-1$ successes can be anywhere

> [!def]+ Negative binomial
> - $X \sim \text{NegBin}(r, p)$
> - $X$ = Total # of trials to the $r$-th success
> - Sum of independent geometrics

# Formulas

- PMF
    - $$p(x) = P(X = x) = \binom{x - 1}{r - 1} p^{r} (1 - p)^{x-r}$$
    - $x=r, r+1, \dots$
- $E[X]$
    - $$E[X] = \frac{r}{p}$$
- $\text{Var}[X]$
    - $$\text{Var}[X] = \frac{r(1-p)}{p^{2}}$$
- $m_{X}(t)$
    - $$m_{X}(t) = \frac{(pe^{t})^{r}}{[1-e^{t}(1-p)]^{r}}$$
