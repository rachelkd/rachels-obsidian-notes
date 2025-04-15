---
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 5
Module:
  - "[[3 - Discrete Random Variables]]"
Date: 2024-10-01
Date created: Fri., Oct. 4, 2024, 9:54:45 pm
Date modified: Fri., Dec. 6, 2024, 1:02:49 am
---

# Sample Spaces with Infinite Outcomes

**Recall**:
You flip a coin until you get a heads.
You are interested in the event you need at least 3 flips.
(Textbook Example 1.4)

- $\Omega = \{ H, TH, TTH, \dots \}$

Consider that we are interested in the *number* of flips we need to get our first heads
- Represented by a **geometric random variable**

# Geometric Experiment

> [!def]+ Geometric experiment
> - A sequence of Bernoulli trials that continue until the first success is observed
> - $n$ is *not* fixed (unlike in the [[The Bernoulli and Binomial Distributions|binomial]])
> - Conditions to be met:
>     1. Independent trials
>     2. Two outcomes per trial
>     3. Same $p$ across trials
> - Geometric RV $X$ counts the ==number of trials *up to and including* the first success==

- Notation:
    - $X \sim \text{Geo}(p)$, or $\text{Geom}(p)$

# Geometric PMF and CDF

- $P(X = k) = (1-p)^{k-1}p$ for $k = 1,2,3$
- To need $k$ trials to get to the *first* success:
    - How many failures do we have?
        - $k - 1$
    - How many successes do we have?
        - 1
    - How many ways can we order this sequence of failures and successes?
        - 1
        - Sequence must look like: $\underbrace{\overset{(1-p)}{F} \dots \overset{(1-p)}{F}}_{k-1} \overset{{p}}{S}$
        - Trials are independent → Can multiply everything together
- CDF:
    - $P(X \leq k) = 1 - (1 - p)^{\lfloor k \rfloor}$
    - $P(X > k) = P(X = k+1) + P(X = k + 2) + \dots = (1-p)^{k}$

![](https://i.imgur.com/3g8WZVN.png)

# Binomial Vs Geometric RVs

See [[The Bernoulli and Binomial Distributions]].

- Binomial:
    - $X \sim \text{Bin}(n, p)$
    - We are counting successes among a *fixed number of trials*
    - $X$ can be $0$
    - Max of $X$ is $n$
- Geometric
    - $X \sim \text{Geo}(p)$
    - Counting the number of trials until we get a *fixed number of successes*
    - $X$ must be 1 or more
    - $X$ can be arbitrarily large

# Properties of the Geometric Distribution

- $E[X] = \frac{1}{p}$
- $V[X] = \frac{{1-p}}{p^{2}}$
- $M_{X}(t) = \frac{pe^t}{1-(1-p)e^{t}}$
- Memoryless Property:
    - $P(X > n + k | X > k) = P(X > n)$ for $n, k = 0, 1, 2, \dots$
    - e.g., We are interested in the number of coin flips until we get our first heads. If we flipped 3 times and they were all tails, what’s the probability of getting a heads on the next flip?
        - Can reword to: What’s the probability of needing 1 more flip to get a success, given that we already got 3 failures
        - The 3 tails we got do not affect any future flips!
    - & # of previously failed trials do not affect # of future trials needed for a success
