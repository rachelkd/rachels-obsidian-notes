---
dg-publish: false
tags: [cs, lecture, math, note, university]
Course Code:
  - "[[CSC236]]"
aliases: []
Week: 9
Module:
  - "[[3 - Divide-and-Conquer Algorithms]]"
Lecture:
  - "18"
  - "19"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/wS4QNgusB0Qg3KYiRwWoPT
Date: 2024-11-08
Date created: Tue., Nov. 19, 2024, 12:39:07 am
Date modified: Sun., Jan. 12, 2025, 10:16:28 pm
---

# Prep A: Geometric Series

## Claims

1. **Geometric Series with Constant Multiple**:
    - A geometric series is a constant multiple of a geometric series with the same ratio but initial term 1.

2. **Constant Multiple of a Geometric Series**:
    - A constant multiple of a geometric series is a geometric series with the same ratio.

3. **Summation of Geometric Series**:
    - If $m, n \in \mathbb{Z}, m \leq n + 1, c, r \in \mathbb{R}$, with $r \neq 0$ if $m < 0$, then
   $$
   \sum_{k=m}^{n} c \cdot r^k 
   $$
    is a geometric series with ratio $r$.

4. **Consecutive Part of a Geometric Series**:
    - Any consecutive part of a geometric series is a geometric series with the same ratio.

5. **Reverse of a Geometric Series**:
    - The reverse of a geometric series with non-zero ratio $r$ is a geometric series with ratio $1/r$.

6. **Series with Positive Terms and Fixed Ratio**:
    - If a series has at least two terms, and its terms are positive with a fixed ratio between consecutive terms, then it’s a geometric series with positive initial term and ratio.

## Behaviour of Geometric Series

> [!info]- Key Properties of Geometric Series
> Based on ratio $r$:
> - $r < 1$: Terms decrease → 0
> - $r = 1$: Terms remain constant
> - $r > 1$: Terms increase → ∞

Consider a geometric series with a *positive initial term* and *positive ratio* (as a function of the number of terms).

- Simplest representation with ratio $r>1$:
    - Geometric series with initial term 1 and ratio 2
    - $$\sum\limits_{k=0}^{n} 2^{k}$$

> [!obs]+ Key Insight
> For ratio > 1:
>
> 1. Each new term more than doubles previous sum
>     - $$\sum_{k=0}^n 2^k > 2 \cdot \sum_{k=0}^{n-1} 2^k$$
> 2. $\iff$ Each new term > sum of all previous terms
>     - $$2^n > \sum_{k=0}^{n-1} 2^k$$
>     - Since $a + b > 2b \iff a > b$

### Proofs

#### Prove $\sum_{k=0}^n 2^k > 2 \cdot \sum_{k=0}^{n-1} 2^k$ for All $n \geq 1$

**Base Case.** When $n = 1$:
$$\sum_{k=0}^1 2^k = 1 + 2 = 3 > 2 \cdot 1 = 2 \cdot \sum_{k=0}^0 2^k$$

**Inductive Step.**
Let $m \in \mathbb{N}, m \geq 1$.
Assume $\sum\limits_{k=0}^m 2^k > 2 \cdot \sum\limits_{k=0}^{m-1} 2^k$ [IH].
WTP: $$\sum_{k=0}^{m+1} 2^k > 2 \cdot \sum_{k=0}^m 2^k$$

Then:
$$
\begin{align*}
\sum_{k=0}^{m+1} 2^k &= 2^{m+1} + \sum_{k=0}^m 2^k \\
&> 2^{m+1} + 2 \cdot \sum_{k=0}^{m-1} 2^k \quad \text{(by IH)} \\
&= 2^{m+1} + 2 \cdot (\sum_{k=0}^m 2^k - 2^m) \\
&= 2^{m+1} + 2 \cdot \sum_{k=0}^m 2^k - 2 \cdot 2^m \\
&= 2 \cdot \sum_{k=0}^m 2^k + (2^{m+1} - 2 \cdot 2^m) \\
&= 2 \cdot \sum_{k=0}^m 2^k
\end{align*}
$$

Thus, by induction, $\sum\limits_{k=0}^n 2^k > 2 \cdot \sum\limits_{k=0}^{n-1} 2^k$ for all $n \geq 1$.

#### Prove $2^n > \sum_{k=0}^{n-1} 2^k$ for All $n \geq 1$

**Base Case.** When $n = 1$:
$$2^1 = 2 > 1 = \sum_{k=0}^{0} 2^k$$

**Inductive Step.**
Assume $2^m > \sum\limits_{k=0}^{m-1} 2^k$ for some $m \geq 1$ [IH].
WTP: $$2^{m+1} > \sum_{k=0}^{m} 2^k$$

Then:
$$
\begin{align*}
\sum_{k=0}^{m} 2^k = 2^m + \sum_{k=0}^{m-1} 2^k &< 2^m + 2^m \quad \text{(by IH)} \\
&= 2 \cdot 2^m \\ 
&= 2^{m+1}
\end{align*}
$$

Thus, by induction, $2^n > \sum\limits_{k=0}^{n-1} 2^k$ for all $n \geq 1$.

> [!obs]+ The final term dominates the series asymptotically
> - This property holds for any ratio $r \geq 2$
> - Does not hold for all $r \in (1, 2)$

> [!thm]+ Asymptotic Behavior
> $$\sum_{k=0}^n 2^k = 1 + 2 + \cdots + 2^n \in \Theta(2^n)$$
>
> This means the sum is asymptotically dominated by its final term.
>
> ![|center|400](https://i.imgur.com/UVP3HZk.png)

> [!thm]+ If $f = g + h$ where $g \in \Omega(h)$, then $f \in \Theta(g)$
>
> **Intuition**
> - If $g$ grows at least as fast as $h$, then
>     - $g + h$ grows at essentially the same rate as $g$
> - The smaller term $h$ becomes negligible compared to $g$
>
> **Example**
> - Let $f(n) = 2^n + n^2$
> - Here $g(n) = 2^n$ and $h(n) = n^2$
> - Since $2^n \in \Omega(n^2)$
> - Therefore $2^n + n^2 \in \Theta(2^n)$

### Binary Trees

> [!obs]+ Sum is $\Theta$ of final term regardless of index
> - $$\sum_{k=0}^{n-1} 2^k \in \Theta(2^{n-1})$$
> - Since $\Theta(2^{n-1}) = \Theta(2^n)$, including/excluding $2^n$ does not affect asymptotic analysis

> [!example]+ Binary Tree Application
> In a complete binary tree:
> - Each level $k$ has $2^k$ nodes
> - Level $k$ has more nodes than all previous levels combined
> - Number of leaves > number of internal nodes
> - Total nodes $\in \Theta(\text{number of leaves})$

- Analysis in binary representation:
    - $$2^{n} = 1 \overbrace{0 \cdots 0_{2}}^{n \text{ bits}} > \overbrace{1 \cdots 1}^{n \text{ bits}} = \sum_{k=0}^{n-1}2^k$$
- The ==leading bit dominates==
    - $$\sum_{k=0}^n 2^k = 11…1_2 = 10…0_2 + 1…1_2 \in \Theta(10…0_2) = \Theta(2^n)$$
    - All other bits are relatively insignificant for asymptotic analysis

> [!note]+ Connection to Algorithm Analysis
> This explains why in many divide-and-conquer algorithms:
> - The work at each level forms a geometric series
> - The total work is dominated by either:
>     - The first level (when ratio < 1)
>     - The last level (when ratio > 1)
>     - All levels equally (when ratio = 1)
> - See [[Recursive Runtime Analysis#The Master Theorem|The Master Theorem]]

## Geometric Series: Closed Form and Analysis

> [!thm]+ Closed Form Formula
> For $r \neq 1$:
> $$1 + r + r^2 + \cdots + r^n = \frac{1-r^{n+1}}{1-r} = \frac{r^{n+1}-1}{r-1}$$

> [!info]+ Behavior Based on Ratio
> For ratio $r$:
> 1. When $r > 1$:
>     - Terms increase exponentially
>     - Final term ≈ $\frac{r-1}{r}$ of entire sum
>     - Sum $\in \Theta(r^{n+1}) = \Theta(r^n)$
> 2. When $r < 1$:
>     - Terms decay exponentially
>     - Sum bounded above by constant $\frac{1}{1-r}$
>     - Sum $\in \Theta(1)$

> [!example]+ Key Examples
> 1. For $r = 2$ (increasing):
>     - $$2^n \leq 1 + 2 + 2^2 + \cdots + 2^n < \frac{2^{n+1}}{(2-1)} = 2 \cdot 2^{n}$$
>     - $$\therefore \sum_{k=0}^n 2^k \in \Theta(2^n)$$
>
> 2. For $r = \frac{1}{2}$ (decreasing):
>     - $$1 \leq 1 + \frac{1}{2} + \frac{1}{4} + \cdots + (\frac{1}{2})^n < 2$$
>     - $$\therefore \sum_{k=0}^n (\frac{1}{2})^k \in \Theta(1)$$

- ==Series is $\Theta$ of its dominant term (if one exists)==
- True regardless of start/end points when ratio fixed
    - Adding terms in decreasing direction: adds at most constant
    - Removing terms in decreasing direction: makes series more like dominant term

## General Case: Arbitrary Start and End Terms

> [!thm]+ General Form
> Let $s$ and $t$ be sequences with $t_n - s_n \in \mathbb{N}$ for all $n \in \mathbb{N}$. Then:
> $$r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} \in \begin{cases} 
> \Theta(r^{s_n}), & r < 1 \\
> \Theta(r^{t_n}), & r > 1
> \end{cases}$$

### Proof

**WTP:** $r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} \in \Theta(r^{t_n})$ for $r > 1$ and $r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} \in \Theta(r^{s_n})$ for $r < 1$.

**Case 1:** Suppose $r > 1$. Then for each $n \in \mathbb{N}$,
$$
1 \cdot r^{t_n} \leq r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} = r^{s_n} \left(1 + \cdots + r^{t_n - s_n}\right),
$$
so
$$
r^{s_n} + \cdots + r^{t_n} = r^{s_n} \cdot \frac{r^{t_n - s_n + 1} - 1}{r - 1} < r^{s_n} \cdot \frac{r^{t_n - s_n + 1}}{r - 1} = \frac{r}{r - 1} \cdot r^{t_n}.
$$

Thus, we have:
$$
r^{s_n} + \cdots + r^{t_n} \in \Theta(r^{t_n}).
$$

**Case 2:** Suppose $r < 1$. Then $\frac{1}{r} > 1$, and for each $n \in \mathbb{N}$,
$$
r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} = \left(\frac{1}{r}\right)^{-s_n} + \left(\frac{1}{r}\right)^{-s_n-1} + \cdots + \left(\frac{1}{r}\right)^{-t_n}.
$$
This can be rewritten as:
$$
\begin{align*}
&= \left(\frac{1}{r}\right)^{-t_n} + \left(\frac{1}{r}\right)^{-t_n+1} + \cdots + \left(\frac{1}{r}\right)^{-s_n}
\end{align*}
$$

and $-s_{n} - (-t_{n}) = t_{n} - s_{n} \in \mathbb{N} \implies -s_{n} > -t_{n}$, so $\left( \frac{1}{r} \right)^{-s_{n}}$ is the dominant term.
Thus, by the first case, we conclude:
$$
r^{s_n} + \cdots + r^{t_n} \in \Theta(r^{s_n}).
$$

### Alternate Proof

**Case 1:** $r > 1$

Let $r^{s_n} + r^{s_n+1} + \cdots + r^{t_n}$ be our series.
For each $n \in \mathbb{N}$,
$$
\begin{align*}
\sum_{k=s_n}^{t_n} r^k &= r^{s_n}(1 + r + \cdots + r^{t_n-s_n}) \\
&= r^{s_n} \cdot \frac{r^{t_n-s_n+1}-1}{r-1} \\
&= \frac{r^{t_n+1}-r^{s_n}}{r-1} \\
&< \frac{r^{t_n+1}}{r-1} \\
&= \frac{r}{r-1} r^{t_{n}} \\
&\in \Theta(r^{t_n})
\end{align*}
$$

**Case 2:** $r < 1$

Similar to Case 1, but now:
$$
\begin{align*}
\sum_{k=s_n}^{t_n} r^k &= r^{s_n}(1 + r + \cdots + r^{t_n-s_n}) \\
&\leq r^{s_n} \cdot \frac{1}{1-r} \\
&\in \Theta(r^{s_n}) \quad \text{(since $r < 1$, lowest power dominates)}
\end{align*}
$$

> [!obs]+ Special Cases
> 1. When $r \neq 1$:
>     - Series $r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} \in \Theta(\max\{r^{s_n}, r^{t_n}\})$
>     - Largest term (at either end) dominates
>
> 2. When $r = 1$:
>     - All terms equal 1 $\implies$ length now matters
>     - $$r^{s_n} + r^{s_n+1} + \cdots + r^{t_n} = 1^{s_{n}} + 1^{s_{n}+1} + \cdots + 1^{t_{n}} = (t_{n} - s_{n} + 1) \in \Theta(t_{n} - s_{n} + 1)$$
>     - Series $\in \Theta(t_n - s_n)$ when $t_n > s_n$ for large enough $n$

> [!thm]+ Constant Multiple Property
> For non-negative $c_n$:
> $$c_n(r^{s_n} + \cdots + r^{t_n}) \in \begin{cases}
> \Theta(\max\{c_n \cdot r^{s_n}, c_n \cdot r^{t_n}\}), & r \neq 1 \\
> \Theta(c_n \cdot (t_n - s_n)), & r = 1
> \end{cases}$$

# Prep B: Logarithms

## Basic Properties

> [!def]+ Definition of Logarithm
> For $c \in \mathbb{R}^+$ with $c \neq 1$, and $x \in \mathbb{R}^+$:
> - Base $c$ logarithm of $x$ is written as $\log_c x$
> - It is the unique number where $c^{\log_c x} = x$
> - Equivalently: for any $y \in \mathbb{R}$, $\log_c c^y = y$

> [!info]+ Key Properties
> For bases > 1:
> - $\log_c$ is increasing concave function on $(0,\infty)$
> - $\lim_{x \to 0^+} \log_c x = -\infty$
> - $\log_c 1 = 0$
> - $\lim_{x \to \infty} \log_c x = \infty$
> - Grows slower than any polynomial: $\log_c n \in O(n^k)$ for all $k \in \mathbb{R}^+$

## Intuitive Understanding

> [!obs]+ Visualization
> - $\log_c x$ represents “number of $c$s in $x$”
> - $x = c^{\log_c x} = c \cdot c \cdot \ldots \cdot c$ ($\log_c x$ times)
> - Works intuitively for $x > 1$
> - For $x = 1$: empty product (zero $c$s)
> - For $x < 1$: negative number of $c$s (like denominator)

> [!thm]+ Basic Rules
> 1. Multiplication adds logs:
>     - $\log_c(x \cdot y) = \log_c x + \log_c y$
> 2. Division subtracts logs:
>     - $\log_c(x/y) = \log_c x - \log_c y$
>     - Special case: $\log_c(1/y) = -\log_c y$
> 3. Different bases:
>     - If $b \neq c$: $\log_b x = (\log_c x)(\log_b c)$
>     - All logarithms are constant multiples of each other
>     - Therefore, all logs are $\Theta$ of each other

## Advanced Properties

> [!thm]+ Exponentiation Rules
> 1. $(x^y)^z = x^{yz} = x^{zy} = (x^z)^y$
> 2. $(c^z)^{\log_c x} = x^z$
> 3. $y^{\log_c x} = x^{\log_c y}$

> [!important]+ Application to Runtime Analysis
> For constants $c$ and $y$:
> - $y^{\log_c x}$ appears in divide-and-conquer analysis
> - Represents repeated multiplications by $y$
> - Each recursive division by $c$ contributes one multiplication by $y$
> - Final result: $y^{\log_c x}$ transforms multiplication by $c$ into multiplication by $y$
