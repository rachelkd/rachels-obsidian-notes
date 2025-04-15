---
dg-publish: false
tags: ['lecture', 'note', stats, university]
Course Code:
  - '[[STA237]]'
Week: 5
Module:
  - '[[3 - Discrete Random Variables]]'
Date: 2024-10-01
Date created: Fri., Oct. 4, 2024, 11:43:13 am
Date modified: Fri., Dec. 6, 2024, 12:02:53 am
---

# Bernoulli Random Variables

- A **Bernoulli random variable** is most easily recognized as a variable that only takes on ==2 mutually exclusive values==
    - Any situation that deals with outcomes that are:
        - Yes/No
        - True/False
        - Heads/Tails
        - Boys/Girls
        - etc.
    - can be modelled with a Bernoulli distribution
    - Outcomes are *not* numerical values → label “yes/success” = 1, “no/failure” = 0
        - Have given our two outcomes a numerical value

# Bernoulli: Physical Setup

We conduct an experiment a single time (i.e., one trial).

- Have two possible mutually exclusive outcomes, labelled success vs. failure where:
    - $P(success) = p, P(failure) = 1 - p$
- Define $X = \begin{cases} 1 && \text{if a success occurs} \\ 0 && \text{if a failure occures} \end{cases}$
- Notation:
    - $X \sim \text{Ber}(p)$, or
    - $\text{Bern}(p)$
    - $X \sim \text{Bernoulli}(p)$
- Probability function:
    - $P(X = x) = p^{x}(1-p)^{1-x},$ for $x = 0, 1$
- e.g., Flip a coin *once*, and let $X = 1$ if we observe a Head

# Bernoulli Probabilities

- Only have to find 2 probabilities:
    - One for $X = 1$ (success)
    - One for $X =0$ (failure)

## Example. Dice Roll

- I roll a dice and I’m interested in if I get a 3 or something else
    - is Bernoulli $\because$ can either get a 3 (success, $X = 1$), or anything else (failure, $X =0$)
    - 6 possible faces to land on $\implies P(X = 1) = \frac{1}{6}$
    - $\implies P(X = 0) = 1 - P(X = 1) = 1 - \frac{1}{6} = \frac{5}{6}$

# Expected Values of Bernoulli Random Variables

- Bernoulli RV is an example of a *discrete random variable* → Use expected value formula to find Bernoulli expected value
- Using dice roll example above where $X = 1 :$ roll a 6 (success):
    - $X = 0$ if I roll anything other than 6 → failure
    - $P(X = 1) = \frac{1}{6} \implies P(X = 0) = \frac{5}{6}$
    - $E[X] = \frac{1}{6} \times 1 + \frac{5}{6} \times 0 = \frac{1}{6}$
    - % Expected value is *not* a weighted sum of the number on the die!

# Bernoulli Probabilities

- $P(X = x) = p^{x}(1-p)^{1-x},$ for $x = 0, 1$
- Expectation:
    - $E[X] = p$
    - $E[X] = P(\text{success}) \times 1 + P(\text{failure}) \times 0 = p + (1-p) \times 0 = p$
- Variance:
    - $V[X] = p(1-p)$
    - $E[X^{2}] = p \times 1^{2} + (1-p) \times 0^{2} = p$
    - $V[X] = E[X^{2}] - E[X]^{2} = p-p^{2} = p(1-p)$
- MGF:
    - $M_{X}(t) = E(e^{tX}) = (1-p) + pe^{t}$
    - $E[e^{tX}] = e^{0t} \times (1-p) + e^{1t} \times p = (1-p) + pe^{t}$

# Binomial Random Variables

- This is the setup to look for to help *identify* a **Binomial random variable**
    - ==Repeated independent Bernoulli trials==
    - Interested in number successes
    - **Two outcomes** for each trial (1 = success, 0 = failure)
    - Trials are **independent**
    - **Same probability** $p$ of seeing the outcome of interest
    - **Fixed number of total trials $n$**
- When we talk about the distribution for a Binomial, we need to refer to two pieces of info:
    - Probability of outcome of interest for each trial
    - Number of Bernoulli trials dealing with
- All problems that use the binomial distribution can be distilled into:
    - “We flip a coin $n$ times, which probability of heads being $p$ each time”
- Notation: $X \sim \text{Binom}(n, p)$ or $\text{Bin}(n, p)$

# Binomial PMF

- How do we calculate $P(X = k)$?
    - Need $k$ successes and $n -k$ failures (from $n$ trials)
    - How many ways can we choose $k$ successes out of $n$ trials?
        - $\binom{n}{k}$
    - What is the probability of $k$ successes?
        - $p^{k}$
    - What is the probability of $n - k$ failures?
        - $(1-p)^{n-k}$
    - Trials are independent $\implies$ can multiply everything together
    - $$\begin{align} &P(X = k) \\ &= (\text{\# of ways to order the successes and failures}) \\ &\times (\text{probability of getting } k \text{ successes and } n-k \text{ failures in any order}) \\ &= nCk \times p^{k} \times (1-p)^{n-k} \end{align}$$

## Example. Three Coin Flips

Define $H :$ success, $P(H) = p$.
$P(\text{get 1 H in total}) = P(X = 1), X \sim \text{Binom}(3, p)$

$HHT, THT, TTH \to \binom{3}{1}$

$\implies P(\text{get 1 H}) = p \times (1-p) \times (1-p) + (1-p) \times p \times (1-p) + (1-p) \times (1-p) \times p$
$\implies P(X = 1) = \binom{3}{1} p^{1} (1-p)^{2}$

# Properties of the Binomial Distribution

- If $X \sim \text{Binom}(n, p)$, then we can think of $X$ as the ==sum of $n$ independent Bernoulli trials==:
    - $X = X_1 + \cdots + X_n$ where the $X_i$’s are i.i.d. $\text{Ber}(p)$
    - $X_i \overset{\text{i.i.d.}}{\sim} \text{Ber}(p)$
    - **i.i.d.**:
        - Independent and Identically Distributed
- $E[X] = np$
    - $E[X] = E[X_1 + \cdots + X_n] = E[X_1] + \cdots + E[X_n] = \underbrace{p + \cdots + p}_{n \text{ times}} = np$
- $V[X] = np(1-p)$
    - $V[X] = V[X_1 + \cdots + X_n] = V[X_1] + \cdots + V[X_n] = \underbrace{p(1-p) + \cdots + p(1-p)}_{n \text{ times}} = np(1-p)$
- $M_X(t) = [(1-p) + pe^t]^n$
    - $M_X(t) = M_{X_1 + \cdots + X_n}(t) = [M_{X_1}(t)]^n = [(1-p) + pe^t]^n$, since $X_{i}$’s are independent
    - ? Using this mgf, show $E(X) = np$ when $X \sim \text{Binom}(n, p)$.

# Binomial PMFs with Different $p$

![](https://i.imgur.com/mzqyOoN.png)

# Binomial PMFs with Different $n$

![](https://i.imgur.com/Pyg6Iox.png)
