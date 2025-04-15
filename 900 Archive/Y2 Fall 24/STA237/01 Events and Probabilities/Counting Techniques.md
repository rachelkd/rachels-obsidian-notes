---
Date created: Tue., Sep. 17, 2024, 5:01:30 pm
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 2
Module:
  - 1 - First Principles of Probability
Date: 2024-09-10
---

(Textbook 1.6-1.7)

# Multiplication Principle

- If there are $n$ outcomes for Experiment 1 and $m$ outcomes for Experiment 2, then there are:
    - $n \times m$ outcomes for the two experiments together
- Permutations and combinations are applications of the multiplication principle
- To go from order mattering to order not mattering:
    - *Divide* by the number of possible orders

# Permutations

> [!def]- Permutation
> - An *ordering* of the elements of the set

- e.g., $\{ a, b, c \}$ has 6 permutations:
    - $\{ a,b,c \}, \{ a,c,b \}, \{ b,a,c \}, \{ b,c,a \}, \{ c,a,b \}, \{ c,b,a \}$

> [!question]- How many permutations does an $n$-element set have?
> - $n$ possibilities for the first element, $n-1$ for the second, and so on, until 1 possibility for the last element
> $$n \times (n-1) \times \dots \times 1 = n!$$

# Sampling with and without Replacement

> [!def]- Sampling with replacement
> - Unit that is selected is returned to the population after being picked
> - e.g., Rolling one die multiple times

> [!def]- Sampling without replacement
> - Unit that is selected is *not* returned to the population after being picked
> - e.g., Picking people for a survey

- In R, `sample()` function takes argument
    - `replace = TRUE` for sampling with replacement
    - `replace = FALSE` for sampling without replacement

# Binary Sequences

> [!def]- Binary sequence
> - A sequence where each element is a 0 or 1
> - Used to enumerate questions such as “How many ways can we pick $k$ elements from a total of $n$?”
>     - $\equiv$ “How many binary sequences of length $n$ have $k$ 1s?”

## Example. Pizza Toppings

A pizza can have 3 possible toppings: cheese, pepperoni, and mushrooms. How many different 2 topping pizzas are there?

- Sample space:
    - $\Omega = \{ \emptyset, C, P, M, CP, CM, PM, CPM \}$
    - There are three ways to make a 2 topping pizza
- How many ways can a binary sequence of length three have two 1s?
    - $\Omega = \{ \underbrace{000}_{\text{CPM}}, \underbrace{100}_{\text{CPM}}, 010, 001, 110, 101, 011, 111 \}$
    - Each digit enumerates one of cheese, pepperoni, and mushrooms
    - 0 = it is not on the pizza, 1 = it is on the pizza

# Combinations and Binomial Coefficients

> [!question] How do we count the number of binary sequences of length $n$ with exactly $k$ 1s?

- **Binomial coefficient**
    - $$nCk = \binom{n}{k} = \frac{n!}{k!(n-k)!}$$
    - ![|250](https://i.imgur.com/K2LXL7e.png)

# Example. Voting Outcomes

There are two candidates in an election: $A$ and $B$. $A$ receives $p$ votes, and $B$ receives $q$ votes. How many possible ways could this have occurred in the population?

- Total population size:
    - $p + q$
- Denote as binary sequence:
    - $0 \text{ if voted for } B, 1 \text{ if voted for } A$
    - $\{ 0, 1, 1, 0, \dots \}$
        - Length $p + q$
        - $p$ 1’s
        - $q$ 0’s
- & $\binom{p + q}{p} = \binom{p + q}{(p + q) - p} = \binom{p + q}{q}$ ways of ordering

# Example. Pick Three Sodas Where Order Does and Doesn’t Matter

There is 1 bottle of each of the following sodas at the store: Coke, Pepsi, and Sprite. You pick 2 sodas at random. How many ways can you pick 2 sodas if the order you pick them matters? What if the order doesn’t matter?

![](https://i.imgur.com/LrUIjOq.png)
