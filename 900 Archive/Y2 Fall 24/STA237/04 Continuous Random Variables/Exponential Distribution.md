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
Date created: Sat., Nov. 9, 2024, 6:32:15 pm
Date modified: Fri., Dec. 6, 2024, 5:05:08 pm
---

> How long do I need to wait for an event to happen?

---

# Example. Emergency Vehicles

Same example from [[Poisson Distribution#Example. Emergency Vehicles|Poisson Distribution]].

- On average, 0.3 emergency vehicles with their sirens on pass by Gracia’s apartment every hour during the day
- We used the **Poisson** distribution to model ==the number of vehicles== that pass during an 8-hour workday

> [!question] What if instead, we were interested in the <mark style="background: #D2B3FFA6;">time until the first vehicle passes</mark> by?

- We have the **[[Geometric Distribution]]** to model the ==number of trials until our event of interest happens==, but
    - Geometric distribution is *discrete*
    - Time is *continuous*
    - We can still use geometric distribution to *approximate* the amount of time until the event of interest occurs

> [!tip]+ What if we divide the time that passes into 1-minute-long intervals?
>
> - Each interval is *non-overlapping* $\implies$ independent
> - Each minute = 1 trial
> - Failure: No vehicles pass by
> - Success: 1 or more vehicles pass by
> - Can count the number of trials until the first success
> - → Gives us the number of minutes until we can see the first vehicle

- ! We can only get as accurate as the nearest minute
  - e.g., No difference between vehicle arriving at 12:01 vs. 12:59
    - In both cases, success will be recorded as the 12th minute
- We can subdivide time into smaller and smaller intervals
- If we take the limit as time gets smaller and smaller, the geometric distribution converges to the **exponential distribution**

# Exponential Distribution

> [!def]+ Exponential Distribution
> A random variable $X$ has an **exponential distribution** with (*rate*) parameter $\lambda > 0$ if its density function has the form
> $$f(x) = \lambda e^{-\lambda x}, \quad \text{for }x > 0$$
> and 0, otherwise. We write $X \sim \text{Exp}(\lambda )$

> [!thm]+ Expectation
> $$E[X] = \frac{1}{\lambda }$$
>
> > [!note]- Derivation
> > $$\begin{align*}
> > E[X] &= \int_{-\infty}^{\infty} xf(x)dx = \int_0^{\infty} x\lambda e^{-\lambda x}dx = \lambda \int_0^{\infty} xe^{-\lambda x}dx \\
> > &= \lambda \left[\left(-\frac{1}{\lambda}x - \frac{1}\lambda^{2}\right)e^{-\lambda x}\right]_0^{\infty} && \text{(by parts)} \\
> > &= \lambda \left[\lim_{x \to \infty}\left(-\frac{1}{\lambda}x - \frac{1}{\lambda^2}\right)e^{-\lambda x} - \left(-\frac{1}{\lambda}(0) - \frac{1}{\lambda^2}\right)e^{-\lambda(0)}\right] \\
> > &= \lambda \left[0 + \frac{1}{\lambda^2}\right] \\
> > &= \frac{1}{\lambda}
> > \end{align*}$$

> [!thm]+ Variance
> $$\text{Var}[X] = \frac{1}{\lambda^2}$$
>
> > [!note]- Derivation
> > $$\begin{align*}
> > \text{Var}[X] &= E[X^2] - (E[X])^2 \\
> > &= \int_0^\infty x^2 \lambda e^{-\lambda x} dx - \frac{1}{\lambda^2} \\
> > &= \frac{2}{\lambda^2} - \frac{1}{\lambda^2} \\
> > &= \frac{1}{\lambda^2}
> > \end{align*}$$

> [!thm]+ MGF
> $$M_X(t) = \frac{\lambda}{\lambda-t} \text{ for } t < \lambda$$
>
> > [!note]- Derivation
> > $$\begin{align*}
> > M_X(t) &= E[e^{tX}] \\
> > &= \int_0^\infty e^{tx} \lambda e^{-\lambda x} dx \\
> > &= \lambda \int_0^\infty e^{-(\lambda-t)x} dx \\
> > &= \lambda \left[-\frac{1}{\lambda-t}e^{-(\lambda-t)x}\right]_0^\infty \\
> > &= \frac{\lambda}{\lambda-t} \quad \text{for } t < \lambda
> > \end{align*}$$

## Properties

- Support is positive reals
    - Distribution measures time; time cannot be negative
- $\lambda$ is called the “rate” parameter
    - Represents the average rate of events per unit time
    - $\frac{1}{\lambda}$ gives the average time between events
- Exponential distributions are often used to model:
    - Lifetimes of products
    - Time until failure
    - Time between events
- Key identifier: When you see phrases like:
    - “time until”
    - “distance until”
  - Similar “until” scenarios
    → Clues that exponential distribution may be appropriate

## Example. Emergency Vehicles

- Time in continuous → Must use a continuous random variable

> [!obs] The ==limit of the geometric distribution== as the number of trials within a time goes to infinity ($n \to \infty$) and $p \to \frac{\lambda}{n}$ is the **exponential distribution**

> [!obs]+ It also turns out that: If the number of events within a time interval follows a $\text{Poisson}(\lambda)$ distribution,
>
> - The time (or distance) in between events (or to the first event) follows an $\text{Exponential}(\lambda)$ distribution
> - See [[Poisson Distribution]]

- The rate parameter $\lambda$ is the same in both distributions
- Same assumptions as Poisson distribution must be met!

> [!important]+ Assumptions about Exponential Distribution
>
> - ==Independent non-overlapping intervals==
>   - No matter how many events happen in one interval, it does not impact any other interval as long as there is no overlap
> - ==Individuality of events==
>   - No two events happen at the same time, but
>   - They can be any positive $\epsilon$ apart
> - ==Uniformity==
>   - Events occur at a uniform rate

![](https://i.imgur.com/QCFZQ7u.png)
![](https://i.imgur.com/6HpHJeP.png)

- ==Smaller rate of events $\iff$ Longer wait times==
  - e.g., If we expect 1 event to occur per hour, then time in between events is expected to be 1 hour
  - If we expect 20 events per hour, the time in between events is going to be $\frac{1}{20 \text{ events/h}} \cdot 60 \text{ min/h} = 3$ minutes
- PDF is ==lower at smaller values==
  - Less likely to have a small wait time

## R Commands for Exponential Distribution

> [!note] R takes λ (rate) as the argument

- PDF: $f(x)$
  - `dexp(x, λ)`
- CDF: $F(x)$
  - `pexp(x, λ)`
- Quantile function: $F^{-1}(p)$
  - `qexp(p, λ)`
- Generate random values
  - `rexp(k, λ)` generates k independent observations

### Examples with $\lambda = 5$

```r
# PDF at x = 0.5
dexp(0.5, 5)
[1] 0.410425

# CDF at x = 0.5
pexp(0.5, 5)
[1] 0.917915

# 25th percentile
qexp(0.25, 5)
[1] 0.05753641

# 75th percentile
qexp(0.75, 5)
[1] 0.2772589

# Generate 10 random values
rexp(10, 5)
[1] 0.17754765 0.11751327 0.04573995 0.20889999 0.03910140 0.09988424
[7] 0.01004202 0.00263283 0.25353986 0.03791917
```

# CDF

> [!thm]+ Cumulative Density Function
> $$F(x) = P(X \leq x) = 1 - e^{-\lambda x}, \quad \text{if }x > 0,$$
> and 0, otherwise.
>
> > [!note]- Derivation
> > $$\begin{align*}
> > F(x) &= P(X \leq x) = \int_{-\infty}^x f(u)du = \int_0^x \lambda e^{-\lambda k}dk \\
> > &= \left[-e^{-\lambda k}\right]_0^x \\
> > &= -e^{-\lambda x} - (-e^{-\lambda \cdot 0}) \\
> > &= -e^{-\lambda x} - (-1) \\
> > &= 1-e^{-\lambda x} \text{ if } x > 0
> > \end{align*}$$

## Quantiles: Inverse CDF

> [!thm]+ Inverse CDF Formula
> For $X \sim \text{Exp}(\lambda)$, if we want to find $x$ such that $P(X \leq x) = p$:
> $$\begin{align*}
> 1-e^{-\lambda x} &= p \\
> e^{-\lambda x} &= 1-p \\
> -\lambda x &= \ln(1-p) \\
> x &= -\frac{\ln(1-p)}{\lambda}
> \end{align*}$$
> This gives us $F^{-1}(p)$, the inverse CDF (quantile function).

## Example. Finding Percentiles

Find the 70th percentile of $\text{Exp}(0.25)$ using the plots, CDF formula, and check with R.

![](https://i.imgur.com/AlXPdlQ.png)

```r
qexp(0.7, 0.25)
[1] 4.815891
```

Using the inverse CDF formula:
$$\begin{align*}
1-e^{-\lambda x} &= p \\
1-e^{-0.25x} &= 0.7 \\
e^{-0.25x} &= 0.3 \\
-0.25x &= \ln(0.3) \\
x &= -\frac{\ln(0.3)}{0.25} \\
x &= 4.815891
\end{align*}$$
# Rate

> [!example]+ Recall Emergency Vehicles Example:
> - On average, 0.3 emergency vehicles with their sirens on pass by Gracia’s apartment every hour during the day
> - Rate that vehicles pass by: $\lambda = 0.3$

- & $\implies$ Time in hours until the first vehicle follows a $\text{Exp}(0.3)$ distribution

- If we wanted to measure the time in minutes
    - ==Convert rate== to be in minutes
    - Same as for the Poisson distribution
    - Would now follow a $\text{Exp}\left( \frac{0.3}{60} \right) = \text{Exp}(0.005)$ distribution
- If we wanted to measure the time until the first vehicle in days
    - Would follow a $\text{Exp}(0.3 \times 24) = \text{Exp}(7.2)$ distribution

> [!important] The rate $\lambda$ must be in the same unit as your unit of measurement!

# Example. Emergency Vehicles

> [!question]+ What is the probability that the time until we see the first vehicle is 1 hour or less?
> - Let $X$ be the time in hours until the first vehicle
> - $X \sim \text{Exp}(0.3)$
> - Looking for $P(X \leq 1)$
> - Can use R:
>     - `pexp(1, 0.3)` returns `0.2591818`
> - If $X \sim \text{Exp}(\lambda)$, then $f(x) = \lambda e^{-\lambda x} \implies F(x) = 1 - e^{-\lambda x}$
> $$P(X \leq 1) = F(1) = 1 - e^{-0.3\times 1} = 0.259$$

> [!question]+ What is the median time we will need to wait until we see the first emergency vehicle?
> - Looking for $q$ such that $P(X \leq q) = 0.5$
> - Using [[#Quantiles Inverse CDF|inverse CDF formula]], $$q = F^{-1}(0.5) = \frac{\ln(0.5)}{-0.3} = 2.310$$
> - `qexp(0.5, 0.3)` returns `2.310491`
> - The median time until the first emergency vehicle is 2.31 hours
> - Check using formula $$F(2.31) = 1 - e^{-0.3 \times 2.31} = 0.5$$
>
> See [[Properties of Continuous RVs#Quantiles for a Continuous RV]]

# Memoryless Property

- Exponential distribution can be thought of as the continuous version of the [[Geometric Distribution]]
- Recall: Geometric distribution has the **[[Geometric Distribution#Properties of the Geometric Distribution|memoryless property]]**
    - If $X \sim \text{Geo}(p)$, then $P(X > n + k | X > k) = P(X > n)$ for $n, k = 0, 1, \dots$
- Exponential distribution also has the **memoryless property**

> [!def]+ Memorylessness for Exponential Distribution
> Let $X \sim \text{Exp}(\lambda)$. For $0 < s < t$,
> $$P(X > s + t | X > s) = \frac{P(X > s + t)}{P(X > s)} = \frac{e^{-\lambda(s+t)}}{e^{-\lambda s}} = e^{-\lambda t} = P(X > t)$$

- These are the only two distributions with the memoryless property

## Example. Emergency Vehicles

> [!question]+ If we see no emergency vehicles in the first hour, what is the probability that we see no vehicles in the first 3 hours?
> - Let $X$ be the time until first vehicle, $X \sim \text{Exp}(0.3)$
> - Looking for $P(X > 3|X > 1)$
>
> ![|center|400](https://i.imgur.com/QaI6wrf.png)
>
> - Using the memoryless property:
> $$\begin{align*}
> P(X > 3|X > 1) &= \frac{P(X > 3)}{P(X > 1)} \\
> &= \frac{1-P(X \leq 3)}{1-P(X \leq 1)} \\
> &= \frac{1-F(3)}{1-F(1)} \\
> &= \frac{e^{-0.3 \times 3}}{e^{-0.3 \times 1}} \\
> &= e^{-0.6} \\
> &\approx 0.5488116
> \end{align*}$$
> - & However many vehicles you already saw does not impact however many vehicles you will see in the next two hours
>     - Independence of non-overlapping intervals assumption at play here

> [!question]+ What is the probability we see no emergency vehicles in the first 2 hours?
> - Looking for $P(X > 2)$
> - Using the CDF:
> $$\begin{align*}
> P(X > 2) &= 1 - P(X \leq 2) \\
> &= 1 - F(2) \\
> &= e^{-0.6} \\
> &\approx 0.5488116
> \end{align*}$$
### R Code

```r
# Probability of Waiting More than 3 Hours given Waited More than 1 Hour
(1-pexp(3,0.3))/(1-pexp(1,0.3))
[1] 0.5488116

# Probability of Waiting More than 2 Hours
1-pexp(2,0.3)
[1] 0.5488116
```

# Shifted Exponential Distribution

- Support of exponential distribution is $X > 0$
- **Shifted exponential distribution** is often used to model lifespan of electronic components (e.g., batteries) or other products
    - Unlikely that if you manufacture a component that it will die right away
    - Usually minimum guaranteed lifespan/warranty period

For example, we can have
- $f(y) = 0.1e^{-0.1(y-0.5)}$ for $y > 0.5$ and 0 otherwise
- Can interpret 0.5 as the *minimum life* or *warranty period*
- Looks similar to the PDF of exponential distribution with $\lambda = 0.1$

> [!question]+ How does the distribution of $Y$ compare to that of $X \sim \text{Exp(0.1)}$?
> $$\begin{align*}
> X &= Y - 0.5 \\
> Y &= X + 0.5 \quad \text{where } X \sim \text{Exp}(\lambda = 0.1)
> \end{align*}$$

- We can use properties of expectation and variance to find the expectation and variance of a **shifted exponential distribution**

## Example. Shifted Exponential Distribution

- The lifetime of an electrical component can be modelled with the random variable $Y$ with PDF $f(y) = 0.1 e^{-0.1(y - 0.5)}$ for $f > 0.5$ and 0 otherwise

> [!question]+ Find $E(Y)$ and $V(Y)$ using what we know about the distribution of $X \sim \text{Exp}(0.1)$
> $$\begin{align*}
> E(Y) &= E(X + 0.5) \\
> &= E(X) + 0.5 \\
> &= \frac{1}{0.1} + 0.5 \\
> &= 10.5 \\
> Var(Y) &= Var(X + 0.5) \\
> &= Var(X) \\
> &= \frac{1}{0.1^{2}} \\
> &= 100
> \end{align*}$$
