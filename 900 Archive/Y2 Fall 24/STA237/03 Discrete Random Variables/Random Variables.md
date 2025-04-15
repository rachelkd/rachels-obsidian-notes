---
dg-publish: true
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 4
Module:
  - "[[3 - Discrete Random Variables]]"
Date: 2024-09-24
Date created: Sat., Sep. 28, 2024, 7:24:27 pm
Date modified: Thu., Dec. 5, 2024, 5:55:13 pm
---

# Random Variables

- Outcomes of a random experiment take on numerical values
    - e.g., We might be interested in how many heads occur in three coin tosses
    - Let $X$ be the number of heads
    - Then $X = 0, 1, 2, 3$ depending on the outcome of the coin tosses
    - Object $X$ is called a **random variable**

> [!def]- Random variable
> - A random variable *assigns* numerical values to the *outcomes* of a random experiment

- Allow us to use algebraic expressions, equalities, and inequalities when manipulating events
- **Discrete random variables**
    - Number of possible values of the random variable is *countable*
    - Often associated with a discrete sample space for the underlying experiment
    - e.g., “Number of times you filled your water bottle today”
- **Continuous random variables**
    - Not countable
    - e.g., “Amount of water (in litres) you have drank today”

![](https://i.imgur.com/TUo0kTW.png)

## Ex. Tossing Three Coins

- In tossing three coins, let $X$ be the number of heads
- Event of getting two heads:
    - Can be written as $\{X = 2\}$
- Probability of getting two heads:
    - $P(X = 2) = P(\{ HHT, HTH, THH \}) = \frac{3}{8}$
- Write $\{X = 2\}$ for the event that the random variable takes the value 2

> [!info] We write $\{X = x\}$ for the event that the random variable $X$ takes the value $x$, where $x$ is a specific number

# Random Variables as Functions

- ==A random variable is actually a *function*==
    - Domain is the sample space of the experiment
    - ==Assigns every outcome of the sample space a real number==
- Consider above [[#Ex. Tossing Three Coins|example]]:
    - $$X(\omega) = \begin{cases} 0, \text{if } \omega = TTT, \\ 1, \text{if } \omega = HTT, THT, TTH \\ 2, \text{if } \omega = HHT, HTH, THH, \\ 3, \text{if } \omega = HHH  \end{cases}$$
    - Probability of getting exactly two heads: $P(X = 2)$
        - Shorthand for $P(\{ \omega : X(w) = 2 \})$
    - Probability of getting at most one head in three coin tosses: $P(X \leq 1) = P(\{ \omega : X(\omega) \leq 1 \}) = P(\{TTT, HTT, THT, TTH \})$

# Discrete Uniform Distribution

- **“Uniformly distributed”**
    - A random variable $X$ takes values in a finite set, all of whose elements are [[Equally Likely Outcomes|equally likely]] $\iff$ $X$ is *uniformly distributed*

> [!def]- Uniform random variable
> Let $S = \{ s_{1}, \cdots, s_{k} \}$ be a finite [[Sets and Set-Builder Notation|set]]. A random variable $X$ is *uniformly distributed on $S$* if $$P(X = s_{i}) = \frac{1}{k}, \text{for } i = 1, \cdots, k$$
> - We write $X \sim \text{Unif}(S)$
> - $\sim$ = “is distributed as”

## Examples

![](https://i.imgur.com/RffJH98.png)

![](https://i.imgur.com/KxBPAdy.png)
