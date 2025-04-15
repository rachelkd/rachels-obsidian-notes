---
dg-publish: true
tags: ["#cs", "#lecture", university]
Course Code:
  - "[[CSC236]]"
Week: 1
Module:
  - "[[1 - Induction]]"
Date: 2024-08-04
Date created: Sun., Aug. 4, 2024, 4:01:19 pm
Date modified: Wed., Oct. 30, 2024, 5:51:48 pm
---

# Predicates

> [!def] Predicate #definition
> - A *parametrized* logical statement
> - Can be viewed as: a function that takes in one or more arguments → only outputs `True` or `False`

### Examples of Predicates

$$\begin{align} 
& EV(n) \; : \; n \text{ is even} \\
& GR(x, y) \; : \; x > y \\
& FROSH(a) \; : \; a \text{ is a first-year university student}
\end{align}$$



## Domains

- Every predicate has a **domain**
    - set of its possible input values
    - e.g., For the above examples, the domain can be $\mathbb{N}$, $\mathbb{R}$, “the set of all UofT students”, respectively