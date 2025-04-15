---
dg-publish: false
tags: [lecture, note, stats, university]
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
Date created: Thu., Oct. 10, 2024, 3:37:31 pm
Date modified: Fri., Dec. 6, 2024, 1:17:05 am
---

- A function of a random variable can follow a different distribution

Consider again our [[#Example. Coffee Shop|coffee shop]] example.
- Customers arrive at an average rate of seven per 15-min interval ($\lambda = 7$)
- Coffee shop can currently serve up to 10 customers per 15-min period without significant delay
- Based on Poisson distribution, $$P(X \leq 10) = 0.90$$ (15-min interval)
    - `ppois(10, 7)` → `0.9014792`

Consider now an 8-hour workday made up of 32 ($8 \times 4$) non-overlapping 15-min intervals.
- ? What is the probability that among these 32 intervals, at least 30 have 10 or less customers arrive?
- What are our two random variables?
    - $X =$ # of customers arriving in a specified interval
    - $Y =$ # of 15 min intervals among 32 that have 10 or less customers arrive

We are interested in $Y$.

- What is the distribution of $Y$?
    - $Y$ has a maximum value: $0 \leq y \leq 32$
    - We can also count failures
        - Each interval has two outcomes:
            - Success: 10 or less customers
            - Failure: More than 10 customers
- ==Check four assumptions== of binomial:
    - [p] 2 outcomes
    - [p] Independent trials
        - Each trial is 15 mins
        - By assumption of Poisson, non-overlapping intervals are independent → # people in each interval doesn’t affect any other interval
    - [p] Fixed number of trials
        - 32 15-min intervals within an 8 hour day
        - Fixed number of intervals, 32 trials
    - [p] Same $P(\text{success})$ in every trial
        - From [[Poisson Distribution#Physical Setup of the Poisson Distribution|uniformity]] assumption of the Poisson
        - $P(\text{success}) = P(X \leq 10) =$ `ppois(10, 7)` = 0.9
        - Same regardless of which interval


$Y \sim \text{Binom}(n = 32, p = 0.9)$
$$\begin{align*}
P(Y \geq 30) &= P(Y = 30) + P(Y = 31) + P(Y = 32) \\
&= \binom{32}{30} (0.9)^{30} (0.1)^{2} + \binom{32}{31} (0.9)^{31} (0.1)^{1} + \binom{32}{32} (0.9)^{32} (0.1)^{0} \\
&= 0.3667
\end{align*}$$

We have a 37% probability that 30 or more 15-min intervals in the workday have 10 or less customers.
