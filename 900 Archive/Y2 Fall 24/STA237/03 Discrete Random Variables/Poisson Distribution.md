---
dg-publish: false
tags: [lecture, note, university]
Course Code:
  - "[[STA237]]"
Week: 6
Module:
  - "[[3 - Discrete Random Variables]]"
Lecture:
  - "10"
Chapter:
Slides/Notes:
Date: 2024-10-08
Date created: Thu., Oct. 10, 2024, 11:59:49 am
Date modified: Sun., Nov. 24, 2024, 2:49:11 am
---

# Poisson Distribution

> Counting the number of “things” per “unit”

### Example. Emergency Vehicles

- On average, 0.3 emergency vehicles with their sirens on pass by Gracia’s apartment every hour during the day
- Gracia works from home for 8 hours; wants to model the # of emergency vehicles that pass by
- ? Why is using a [[The Bernoulli and Binomial Distributions|binomial distribution]] with parameters $n = 8$ (8 hours) and $p = 0.3$ *not* appropriate for this situation? We can assume that every period of non-overlapping time is independent
    - Able to have more than 1 emergency vehicle pass by in a single hour
    - → Violates assumption of binomial distribution
        - Each trial, we are only supposed to have 2 outcomes: success (a vehicle passes by), failure (no vehicles pass by)
- ? What if we divide 8 hours into 480 minutes vs. 1-hour chunks?
    - Average number of emergency vehicles that pass by every minute is $\frac{0.3}{60} = 0.005$
    - Very unlikely $\because$ 1 minute interval is so small that > 1 emergency vehicle will pass by every minute
    - Each minute, either there can be an emergency vehicle (success) or not (failure)
    - → Number of emergency vehicles that pass by can range from $0, 1, \dots, 480$
    - In real life, even if it is unlikely, there can be an unlimited number of emergency vehicles that pass by in any period

# Definition

> [!def]- Poisson distribution describes…
>
> - The chances a random event will occur in an interval (time or space)
> - Assumes that:
>     - Probability the outcome occurs within each trial is very small, but
>     - Number of trials (i.e., interval of time or space) is very large so that the event is bound to happen sometimes anyway

> [!def]- Poisson distribution
>
> - Is the limiting distribution of the Binomial as $n \to \infty$ and $p \to 0$, but $np \to \lambda > 0$
>     - As we subdivide our interval into smaller and smaller pieces, we will not need to worry about the chance that 2 events occur in the same interval
>     - Each interval only has 2 outcomes:
>         - Event occurs (success), or
>         - It doesn’t (failure)

> [!note]+ Notation: $X \sim \text{Pois}(\lambda), \lambda > 0$
>
> - $X$ counts the number of *events* that occur in 1 unit of time, if the events occurs at a rate of $\lambda$/unit

## Formulas

- PMF:
    - $$P(X=x) = \frac{{e^{-\lambda}\lambda^{x}}}{x!}$$
- Expectation:
    - $$E[X] = \lambda$$
- Variance:
    - $$V[X] = \lambda$$
- MGF:
    - $$M_{X}(t) = e^{\lambda(e^{t}-1)}$$

## Example. Emergency Vehicles

- Let $X$ be the number of times an event occurs in an interval of length $t$, where the event occurs at a rate of $\lambda$ per unit
- Then, $X \sim \text{Poi}(\lambda  t)$ with PMF: $P(X = x) = \frac{{e^{-(\lambda t)}(\lambda  t)^{x}}}{x!}$ for $x = 0, 1, 2, \dots$
- $\lambda$ is called the **intensity** or **rate of occurrence**
    - Represents the average rate at which events occur per unit of time
- $\lambda t$ represents the average number of occurrences in $t$ units of time
- Recall from [[#Example. Emergency Vehicles]]: 0.3 emergency vehicles pass/hour
    - Want to model number of emergency vehicles that pass over 8 hours
- $X$ = Number of emergency vehicles over 8 hours
- $\implies X \sim \text{Pois}(0.3 \times 8) = \text{Pois}(2.4)$

![](https://i.imgur.com/Z4iuJwp.png)

# Examples of Random Variables Modelled by a Poisson Distribution

- Number of calls arriving at a switchboard in a minute:
    - $\lambda =$ average number of calls/min
- Number of bankruptcies that are filed in a month:
    - $\lambda =$ average number of bankruptcies/month
- Number of customers arriving to their bank in a day:
    - $\lambda =$ average number of customers arriving/day
- Number of chocolate chips in a cookie:
    - $\lambda =$ average # chips/cookie
- Number of bacteria per mL of a solution
    - $\lambda =$ average # of bacteria/mL

# Physical Setup of the Poisson Distribution

- **Independent**:
    - Number of occurrences in non-overlapping intervals are independent
- **Individuality**:
    - Only one event can occur at the same time, but
    - Distance between two events can be *very, very small*
- **Uniformity** (homogeneity):
    - Events occur at a uniform rate of $\lambda$ over time

![|400](https://i.imgur.com/eTh1AKi.png)

## Example. Coffee Shop

- A coffee shop manager wishes to provide prompt service for customers at the drive-through window
    - Average arrival rate: 7 customers per 15-min period
- Let $X$ denote number of customers arriving per 15-min period
- Assume $X$ has a Poisson distribution

<!-- break -->
- $X \sim \text{Pois}(\lambda = 7)$
- Find probability that ten customers arrive in a particular 15-min period
    - $P(X = 10) = \frac{{e^{-7}\lambda ^{10}}}{10!} = 0.07048$
    - `dpois(10, 7)` → `0.07098327`
- Find probability that 20 customers arrive in a particular ==30-min period==
    - 7 customers/15 min. → 14 customers/30 min.
    - $X \sim \text{Pois}(\lambda=14)$
    - $P(X = 20) = \frac{{e^{-14}14^{20}}}{20!} = 0.0286$
    - `dpois(20, 14)` → `0.02859653`
- Solve for:
    - Average number of customers to arrive in a 15-min period
        - $E[X] = \lambda = 7$
    - Average number of customers to arrive in a 20-min period
        - $E[X] = \frac{7}{15} \times 20 = 9.3$
    - Average number of customers to arrive in a 5-min period
        - $E[X] = \frac{7}{15} \times 5 = 2.3$
    - Average number of customers to arrive in a 2-hour period
        - 120 minutes
        - $E[X] = \frac{7}{15} \times 120 = 56$

# When to Use Binomials Vs Poisson

- Both count the number of times an event of interest occurs
- Ask yourself:
    - Can we specify in advance the maximum value of $X$?
    - Does it make sense to ask how often the event did *not* occur/Can we count the number of *failures*?
- Yes to both:
    - $X$ is **[[The Bernoulli and Binomial Distributions|binomial]]**
- No to both:
    - $X$ is **Poisson**
    - In coffee shop example, there is no maximum number of customers
    - Does not make sense to ask how many people did not come

# Poisson and an Approximation to the Binomial

- For a binomial distribution with very large $n$ and very small $p$, we can approximate probabilities with the $\text{Pois}(\lambda = np)$ distribution
- Approximation gets larger as $n \to \infty, p \to 0$ with $np \to \lambda$

## Example. Car Batteries

Suppose that an automobile parts wholesaler claims that 0.5% of car batteries in a shipment are defective. A random sample of 200 batteries is taken, and four are found to be defective.

- Solve for exact probability that four or more batteries among 200 are found to be defective
    - $X \sim \text{Binom}(n = 200, p = 0.005)$
    - $$\begin{align*} P(X \geq 4) &= 1 - P(X < 4) \\ &= 1 - P(X \leq 3) \\ &= 1 - P(X = 3) - P(X = 2) - P(X = 1) - P(X = 0) \\ &= 0.01868\end{align*}$$
- Use the Poisson approximation to find the probability that four or more car batteries in a random sample of 200 would be found defective
    - $\lambda = np = 200 \times 0.005 = 1$
    - $X \sim \text{Pois}(\lambda = 1)$
    - $$\begin{align*} P(X \geq 4) &= 1 - P(X \leq 3) \\ &= 1 - P(X \leq 3) \\ &= 1 - \left( e^{-1} \left( \frac{1^{0}}{0!} + \frac{1^{1}}{1!} + \frac{1^{2}}{2!} + \frac{1^{3}}{3!} \right)  \right) \\ &= 0.01899 \end{align*}$$
