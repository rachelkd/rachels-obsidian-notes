---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 12
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "21"
  - "22"
Chapter: 
Slides/Notes: 
Date: 2024-11-26
Date created: Tue., Dec. 3, 2024, 12:15:02 am
Date modified: Thu., Dec. 5, 2024, 2:09:27 pm
aliases: [LLN]
---

# Law of Large Numbers (LLN)

## Set-up for LLN and CLT

- Let $X_{1}, X_{2}, \dots, X_{n}$ be $n$ independent RVs with the same underlying distribution (any distribution)
    - We say that $X_{1}, X_{2}, \dots, X_{n}$ are independent and identically distributed

> [!important] Specifically, ==$X_{1}, X_{2}, \dots, X_{n}$ have the same mean $\mu$ and variance $\sigma^{2}$==

> [!note] $\mu$ and $\sigma^{2}$ are commonly used to denote the mean and variance of a variety of distributions/RVs, not just the normal distribution.

- We may be interested in the distribution of:
    - The sum $$S_{n} = X_{1} + X_{2} + \dots + X_{n}$$
        - e.g., Total GDP, total number of children born in a town last year, etc.
    - The mean $$\overline{X_{n}} = \frac{{X_{1} + \dots + X_{n}}}{n}$$
        - e.g., Average income per person, average number of children per family
- & $S_{n}$and $\overline{X_{n}}$are functions of random variables, so they themselves are *random variables*

> [!question] What value can we expect $\overline{X_{n}}$ to be close to?

> [!question] What are the distributions of $S_{n}$ and $\overline{X_{n}}$?

> [!def]+ Sampling distribution
> - The distribution of $\overline{X_{n}}$

## The Law of Large Numbers

Recall: **Monte Carlo simulations** from the beginning of the semester.

> [!obs] The more random numbers we generate, the closer our estimate was to the true value we were interested in

> [!abstract]+ The Law of Large Numbers
> - Formalizes this idea that ==$\overline{X_{n}}$ converges towards $\mu$==
> - Describes what happens to $\overline{X_{n}}$ as the sample size $n$ increases

## Two Forms of the Law of Large Numbers

### The Weak Law of Large Numbers (WLLN)

> [!thm]+ Weak Law of Large Numbers
> Let $X_{1}, X_{2}, \dots$ be an i.i.d. sequence of random variables with finite mean $\mu$ and variance $\sigma^{2}$.
> For $n = 1, 2, \dots$, let $S_{n} = X_{1} + \dots + X_{n}$.
> Then,
> $$
> P \left( \left| \frac{S_{n}}{n} - \mu \right| \geq \epsilon  \right) \to 0, 
> $$
> as $n \to \infty$

- Convergence here is known as **convergence in probability**
- WLLN says:
    - $\overline{X_{n}} = \frac{S_{n}}{n}$ converges in probability to $\mu$
- WLLN is about:
    - The **limit of a probability**
    - A **limit of a sequence of numbers**
- Finite $\sigma^{2}$ condition is not necessary, but it makes the proof easier
    - Proof of the WLLN follows from the Chebyshev Inequality (p. 412 of textbook; untestable)

### The Strong Law of Large Numbers (SLLN)

> [!thm]+ Strong Law of Large Numbers
> Let $X_{1}, X_{2}, \dots$ be an i.i.d. sequence of random variables with finite mean $\mu$.
> For $n = 1, 2, \dots$, let $S_{n} = X_{1} + \dots + X_{n}$.
> Then,
>
> $$
> P \left( \lim_{ n \to \infty } \frac{S_{n}}{n} = \mu \right) = 1
> $$
> We say that ==$\frac{S_{n}}{n}$ converges to $\mu$ with probability 1==.

- Convergence here is known as **almost sure convergence**
- SLLN says:
    - $\overline{X_{n}} = \frac{S_{n}}{n}$ converges *almost surely* to $\mu$
- SLLN is about:
    - The **probability of a limit**
    - The **probability of an event expressed as a limit**

### Strong vs. Weak Laws of Large Numbers

- Both LLNs:
    - The sample mean will ==converge to the true mean $\mu$== as the sample size $n$ approaches $\infty$
- Weak law:
    - Describes how a *sequence* of probabilities *converges*
- Strong law:
    - Describes how a sequence of random variables behaves in the limit

> [!important]+ **Almost sure convergence** is a stronger form of convergence than **convergence in probability**.
> - Sequences of RVs that converge almost surely also converge in probability
> - Converse is not necessarily true
> - Will encounter cases where WLLN holds, but not SLLN
>     - In upper year probability classes (STA347)

![|center|500](https://i.imgur.com/fxaGqsM.png)

*Example. Fair Coin Flips*

## Why Do We Need Finite Mean $\mu$?

$E(x) = \int_{-\infty}^{\infty} xf(x) \, dx$ does not converge

![|center|500](https://i.imgur.com/LluEesY.png)

## Application

> [!tip] We can use the Law of Large Numbers to estimate probabilities

Suppose we wanted to find $P(X > 2)$, when $X \sim \text{Exp}(3)$.

> [!note]+ Say we did not have the CDF for the exponential distribution.
> We know this is $$1-F(2) = e^{-3\times2} = 0.00248$$

- Recall: **[[Probability|relative frequency definition of probability]]**
    - The probability of an event is the *proportion of time it happens* in the long run
- Define an indicator variable:
    - $I = 1$ if $X > 2$, and
        - 0 otherwise
    - $\implies P(I = 1) = P(X > 2) \to I \sim \text{Bern}\left( P(X > 2) \right)$
    - $E[I] = P(X > 2)$
- The sample means of $I$ will give us an estimate of $P(X > 2)$

```r
set.seed(237)
n = 100000
x = rexp(n, rate = 3)
I = x > 2
I_means = cumsum(I)/(1 : n)

plot(1:n, I_means, xlab = "n", ylab = "mean of I(X > 2)")
abline(h = exp(-6), col = "red")
```

![|center|400](https://i.imgur.com/jclnZAD.png)

## Sample Size for LLN

- Took many samples for the average of our indicator function to *converge* to $P(X > 2)$
    - Since $X > 2$ is a rare event
- If you are simulating a discrete random variable with a finite number of outcomes, you want to make sure that there is a ==large number of each outcome appearing==
    - Want both $X > 2$ and $X \leq 2$ to appear many times

> [!example]+ Simulate $P(X > 0.25)$
> - We know that $P(X > 0.25) = e^{-3 \times 0.25} = 0.472$
>     - → Both $X > 0.25$ and $X \leq 0.25$ appear frequently
>
> ```r
> set.seed(237)
> n = 1000
> x = rexp(n, rate = 3)
> I_means = cumsum(I)/(1:n)
> 
> plot(1:n, I_means, xlab = "n", ylab = "mean of I(X > 0.25)")
> abline(h = exp(-3 * 0.25)), col = "red")
> ```
>
> ![](https://i.imgur.com/rTvFP3U.png)

- Needed less than 1000 trials until convergence
    - Even for large $n$, estimate is still not exactly 0.25

> [!important] How large of a sample size you need depends heavily on what you are estimating and the context of the problem.
