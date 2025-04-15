---
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 5
Module:
  - "[[3 - Discrete Random Variables]]"
Date: 2024-10-01
Date created: Fri., Oct. 4, 2024, 11:16:00 am
Date modified: Sun., Nov. 17, 2024, 9:30:33 pm
---

# Discrete Uniform Distribution

> [!def]- $X$ is a **discrete uniform distribution** if
>
> - It takes integer values in $[a, b] : a, a+1, \dots, b-1, b$
> - It is *equally likely* for $X$ to take any of these values

- There are $b - a + 1$ different possible values for $X$ to take
- Probability of $X$ taking any of these values is $\frac{1}{b-a+1_{[=n]}}$
- Notation: $X$ is uniformly distributed on a set $S$
    - $X \sim \text{Unif}(S)$
    - $X \sim U[S]$
    - $X \sim \text{Unif}[a, b]$
    - $\sim$ : “is distributed as,” or “follows”

<!-- break -->
- PMF:
    - $$p_{x}(x) = \begin{cases} \frac{1}{b-a+1}, && a \leq x \leq b  \\ 0, && \text{otherwise} \end{cases}$$
- CDF:
    - $$F_{X}(x) = \begin{cases} 0, && x < a \\ \frac{\lfloor x \rfloor - a + 1}{b-a+1}, && a \leq x < b \\ 1, && x \geq b \end{cases}$$
- ![|400](https://i.imgur.com/Dad67RS.png)

# Properties of the Discrete Uniform Distribution

- Expectation:
    - $$E[X] = \frac{{a+b}}{2}$$
- Variance:
    - $$V[X] = \frac{{(b-a+1)^{2} - 1}}{12}$$
- MGF:
    - $$M_{X}(T) = \frac{{e^{at}-e^{(b+1)t}}}{(b-a+1)(1-e^t)}$$

## Proofs

Assume $X \sim U[1, n]$.
$$\begin{align}
E[X] &= \sum_{\text{all }x} x \cdot P(X = x)  \\
&= \sum_{k=1}^{n} k \left( \frac{1}{n}\right)  \\
&= \frac{1}{n} \frac{{n(n+1)}}{2}  \\
&= \frac{{n + 1}}{2}
\end{align}$$

$$\begin{align}
E[X^{2}] = \sum_{k=1}^{n} k^{2} \left( \frac{1}{n} \right) &= \frac{1}{n} \sum_{k=1}^{n} k^{2}  \\
&= \frac{1}{n} \frac{{n(n+1)(2n+1)}}{6}  \\
&= \frac{{(n+1)(2n+1)}}{6}
\end{align}$$

$$Var[X] = E[X^{2}] - E[X]^{2} = \frac{n^{2}-1}{12}$$
$$\begin{align}
M_{X}(t) = E[e^{tx}] &= \sum_{\forall x \in X} e^{tx} P(X=x)  \\
&= \frac{1}{n} \sum_{k=1}^{n} e^{tk}  \\
&= \frac{1}{n} \sum_{k=1}^{n} (e^{t})^{k}  \\
&= \frac{e^{t}}{n} \sum_{k=0}^{n-1} (e^{t})^{k}  \\
&= \frac{{e^{t}(1-e^{nt})}}{n(1-e^{t})} && \text{(geometric sequence with } r = e^{t})
\end{align}$$
