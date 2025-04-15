---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC236]]"
Week: 10
Module:
  - "[[3 - Divide-and-Conquer Algorithms]]"
Lecture:
  - "18"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/kdza4J6PUXrLzhbGXzqr4p
  - https://share.goodnotes.com/s/sPRcefX3LiRnVzRuUVAyov
Date: 2024-11-15
Date created: Tue., Nov. 19, 2024, 4:04:31 pm
Date modified: Tue., Nov. 19, 2024, 4:41:59 pm
---

# The Master Theorem

> [!thm]+ The Master Theorem
> *Every* recurrence of the form $T(n) = a \cdot T\left( \frac{n}{b} \right) + \Theta(n^{d})$ has the closed-form solution:
>
> $$T(n) \in \begin{cases}
> \Theta(n^d) && \text{if } a < b^d \\
> \Theta(n^d \log_b n) && \text{if } a = b^d \\
> \Theta(n^{\log_b a}) && \text{if } a > b^d
> \end{cases}$$

> [!note]+ Interpretation
> - When $a < b^d$: Non-recursive work dominates
> - When $a = b^d$: Both parts contribute equally
> - When $a > b^d$: Recursive work dominates
>
> Where:
> - $a$ = number of recursive calls
> - $b$ = factor by which input size decreases
> - $d$ = exponent of non-recursive work

- This can be derived (abstractly) using the same process

> [!obs]+ Intuition (with recursion tree).
> ![](https://i.imgur.com/WP7FVGu.png)
> $$\text{Total steps} = n^{d} \sum_{k=0}^{\log _{b}n}\left( \frac{a}{b^{d}} \right)^{k}$$

## Prep C: Master Theorem

Consider function $f$ for $a \in \mathbb{N}^+$, $b > 1$, and $d \geq 0$:

$$f(n) = \begin{cases}
n^d + a \cdot f(\frac{n}{b}), & n > 1 \\
1, & n = 1
\end{cases}$$

This represents runtime of divide-and-conquer algorithms where:

- Problem of size $n$ divided into $a$ subproblems
- Each subproblem has size $\frac{n}{b}$
- Non-recursive work takes $n^d$ basic steps

> [!important] Asymptotic analysis ignores:
> - Floors and ceilings
> - Exact base cases
> - Whether $\frac{n}{b}$ is natural number
> - Potential base case overshooting

> [!summary]+ Parameters
> - $a$ (branching factor):
>     - Must be whole number
>     - Number of recursive calls
>     - ==Determines recursion tree width==
>     - Usually need $a \geq 1$
> 
> - $b$ (size reduction factor):
>     - Must be $> 1$
>     - How much problem size decreases
>     - Determines recursion tree height
>         - Increasing $b \implies$ less internal levels
>     - ==Tree has $\log_b n$ internal levels + 1 leaf level==

> [!question]+ How does increasing $a$ affect the tree?
> - Increases width of tree at each level
> - Each node will have $a$ children instead of original number
> - Total nodes at level $k$ becomes $a^k$
> - Makes tree "bushier" horizontally

> [!question]+ How does increasing $b$ affect the tree?
> - Decreases height of tree
> - Problem size decreases faster at each level
> - Fewer levels overall since $\log_b n$ gets smaller
> - Makes tree "shorter" vertically

> [!question]+ What is the size of problem at different levels?
> - At root (level 0):
>     - Size = $n$
> - At level 1 (below root):
>     - Size = $\frac{n}{b}$ for each node
>     - Total of $a$ nodes at this level
> - At level $k$:
>     - Size = $\frac{n}{b^k}$ for each node
>     - Total of $a^k$ nodes at this level

### Node Analysis in Recursion Tree

- Each node contribution:
    - Has input size $n$ (varies by level)
    - Contributes $n^d$ basic steps
    - All nodes at same level have same input size
- For a node with input size $n$:
    - $f(n/b)$ represents:
        - Solution to one subproblem
        - One child node's total contribution
    - $a \cdot f(n/b)$ represents:
        - Combined solution of all subproblems
        - Total contribution of all child nodes
- Base Case Uniformity:
    - Leaves originally contribute 1
    - Can be rewritten as $1^d = 1$
    - Allows uniform view: every node contributes $(size)^d$
    - Updated function:
        $$f(n) = \begin{cases}
        n^d + a \cdot f(\frac{n}{b}), & n > 1 \\
        n^d, & n = 1
        \end{cases}$$

- Interpretation of Values:
    - $a, b, d$ are unitless parameters
    - $f(n), f(n/b), n^d$ represent “basic steps”

### Important Considerations for Analysis

- Parameters Precision:
    - ==Parameters $a$, $b$, and $d$ cannot be $\Theta$ approximations==
    - Precise values affect final $\Theta$ despite:
        - $a$ appearing as constant factor
        - Number of levels being logarithmic, which is
            - Independent from $b$

- What Can Be Ignored:
    - Constant addition in $n$:
        - Floors and ceilings in division
        - Details left to Hadzilacos
    - Constant addition in number of levels:
        - Moving base cases to other constants
### Generalization Beyond Powers of $b$

- For polynomial-bounded functions:
    - Being off by factor of $b$ between exact powers
    - Produces at most constant factor error
    - Example: Mergesort
        - ==Function grows less than quadratic==
        - Input within factor of 2 of $n$
        - Result off by at most factor of $2^2 = 4$
        - Better bounds possible for $k > 1$
- Contrast:
    - Practice Problem #5:
        - Function grows very fast
        - ==+1 change in input changes $\Theta$ class==

# Karatsuba’s Algorithm

> [!def]+ Binary list
> - List where every element is 0 or 1

```python
# Pre(X, Y): X, Y Are Binary Lists ∧ len(X) = len(Y) > 0
def M(X, Y):
    n = len(X)  # = len(Y) > 0
    if n == 1: return [X[0] * Y[0]]
    if n % 2 == 1:  # n is odd
        X = X + [0]
        Y = Y + [0]
        n = n + 1
    
    X0, X1 = X[0 : n // 2], X[n // 2, n]
    Y0, Y1 = Y[0 : n // 2], Y[n // 2, n]
    
    P1 = M(X0, Y0)
    P2 = M(X1, Y1)
    P3 = M(A(X0, Y0), A(X1, Y1))  # A returns size n / 2
    
    return A(n * [0] + P3, n // 2 * [0] + S(P2, A(P1, P3)), P1)
```

- Assume A, S are helpers with runtime $\Theta(n)$

![](https://i.imgur.com/Zzhs0We.png)

### Recurrence Relation

$$T(n) = 3T\left( \frac{n}{2} \right) + \Theta(n^{1})$$
where $a=3, b=2, d=1$
$$\therefore T(n) \in \Theta(n^{\log_{b}a}) = \Theta(n^{\log_{2}3})$$
