---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Las Vegas algorithm, Monte Carlo algorithm, randomized algorithms]
Week: 12
Module:
  - "[[5 - Randomized Algorithms]]"
Lecture:
  - "23"
  - "24"
Chapter: 
Slides/Notes: 
Date: 2025-04-01
Date created: Tue., Mar. 25, 2025, 2:59:20 am
Date modified: Wed., Apr. 16, 2025, 4:56:41 am
---

# Quicksort

## The Idea

- **Input**
    - An array $A$ of numbers with size $n$
- **Output**
    - Return $A$ such that elements in $A$ are in sorted non-decreasing order

> [!impl]+ Implementation
> 1. If $\text{len}(A)$ is 1: return
> 2. Pick a pivot $p$
>     - Usually last element of $A$
> 3. Partition the array into:
>     - $A_{1}$: elements of $A$ *less* than $p$
>     - $A_{2}$: elements of $A$ *equal* to $p$
>     - $A_{3}$: elements of $A$ greater than $p$
> 4. Recursively repeat this procedure for $A_{1}$ and $A_{3}$

(Complete implementation in Ch. 7 CLRS)

![](https://i.imgur.com/1Rs5pWt.png)

```c
Quicksort(A, p, r)
    if p < r
        // Partition the subarray around the pivot, which ends up in A[q].
        q = Partition(A, p, r)
        Quicksort(A, p, q - 1)  // Recursively sort the low side
        Quicksort(A, q + 1, r)  // Recursively sort the high side
```

```c
Partition(A, p, r)
    x = A[r]                         // x = Pivot
    i = p - 1                        // Highest index into the low side
    for j = p to r - 1               // Process each element other than the pivot
        if A[j] <= x                 // Does this element belong on the low side?
            i = i + 1                // Index of a new slot in the low side
            exchange A[i] with A[j]  // Put this element there
exchange A[i + 1] with A[r]          // Pivot goes just to the right of the low side
return i + 1                         // New index of the pivot
```

![](https://i.imgur.com/Ut4C7Ie.png)

> [!thm]+ Loop invariant
> After every iteration (or at the beginning of each iteration of the loop of lines 4-7), for any array index $k$, the following conditions hold:
> 1. if $p \leq k \leq i$, then $A[k] \leq x$
> 2. if $i + 1 \leq k \leq j - 1$, then $A[k] > x$
> 3. if $k = r$, then $A[k] = x$
> 4. Values in $A[j : r - 1]$ have unknown relationships to $x$ i.e., unorganized

- % We increment `j` at the end of the for loop body for each iteration

![](https://i.imgur.com/9b0f3mN.png)

![](https://i.imgur.com/fluom75.png)

## Worst-case Running Time

Let $T(n)$ denote the **worst-case** running time of `Quicksort` for an input $A$ of size $n$.

- % We choose the *number of comparisons* (line 5) performed during `Quicksort` as the **representative operation**

> [!obs]+ Observations
> 1. A pivot never goes into a sub-array on which a recursive call is made. That is, ==*each element* in $A$ can be chosen as the **pivot** at most *once*==.
> 2. In each recursive call: elements in the sub-array are *only compared to pivots*
> 3. After being a pivot, that pivot will *never be compared* with anyone anymore.

> [!important]+ Implication
> Every pair $(a, b)$ in $A$ are compared with each other *at most once* (but can also be 0).

> [!important]+ Conclusion
> The total number of comparisons is less than or equal to the total number of pairs.

> [!info]+ Number of pairs in an array of size $n$
> $$
> \binom{n}{2} = \frac{n(n-1)}{2} \in \Theta(n^{2})
> $$

![|400](https://i.imgur.com/DkNuaSN.png)

- % This is what it says in Marsha’s notes, but I think it even goes down to $+1$
    - Consider calling `Partition` on $A[1:2]$ (inclusive of both, index starting from 1)
    - → Does one comparison

## Average-case Running Time

> [!important]+ Quicksort is quick in the average case.

### Review: Average-case and Indicator Variables

Recall from [[Algorithm Analysis]] and [[Introduction and Analyzing Running Time (Course Notes)]]:

![[Algorithm Analysis#^dffdb9]]

![[Introduction and Analyzing Running Time (Course Notes)#^0d2672]]

### Analysis

Then, we have:

- $S_{n}$
    - All permutations of $n$ numbers
    - Assume all elements are *distinct*
- Probability assumption
    - Each permutation appears equally likely
- $t_{n}(A)$
    - Number of comparisons performed on an array $A$ of size $n$
- Compute $\mathbb{E}[t_{n}]$

Let $z_{1}, z_{2}, \dots, z_{n}$ be the sequence of elements in $A$ in the *sorted order*.

![|400](https://i.imgur.com/sIENn2P.png)

We define the following indicator random variable:
$$
X_{i,j} = \begin{cases}
1 && \text{if the values }z_{i} \text{ and } z_{j} \text{ are compared}, \\
0 && \text{otherwise}
\end{cases}
$$
Then,
$$
t_{n} = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} X_{i, j}
$$
So,
$$
\mathbb{E}\left[ t_{n} \right] = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} \text{Pr}[X_{i, j} = 1] = \sum\limits_{i=1}^{n} \sum\limits_{j=i+1}^{n} \text{Pr}\left( z_{i} \text{ is compared to } z_{j} \right) 
$$

#### Computing $\text{Pr}\left[ X_{i, j} = 1 \right]$

> [!obs]+ Observations
> 1. $z_{i}$ and $z_{j}$ are compared when:
>     - (1) one of them is chosen as the *pivot*
>     - (2) the other is in the *same partition*
> 2. $z_{i}$ and $z_{j}$ are in different partitions if a number $z_{k}$, ( $z_{i} < z_{k} < z_{j}$) is chose as a pivot before $z_{i}$ and $z_{j}$ is chosen
> 3. & → $z_{i}$ and $z_{j}$ are compared by `Quicksort` iff one of $z_{i}$ or $z_{j}$ is selected as pivot before $z_{i + 1}, z_{i + 2}, \dots, z_{j - 1}$

> [!important]+ Conclusion
> $z_{i}$ and $z_{j}$ are compared by `Quicksort` iff out of the numbers of $\{ z_{i}, z_{i + 1}, \dots, z_{j - 1}, z_{j} \}$, one of them is selected to be the pivot first.

$$
\begin{align}
\text{Pr}[X_{i, j} = 1] & = \text{Pr}[z_{i} \text{ is compared with } z_{j}] \\
 & = \text{Pr}\left[ z_{i} \text{ or } z_{j} \text{ is the first element among } \{ \underbrace{ z_{i}, z_{i + 1}, \dots, z_{j - 1}, z_{j} }_{ j - i + 1 } \} \text{ chosen as pivot} \right]  \\
 & = \frac{2}{j - i + 1}
\end{align}
$$

Then,
$$
\begin{align}
\mathbb{E}\left[ t_{n} \right] & = \sum\limits_{i=1}^{n} \sum\limits_{j=i+1}^{n} \text{Pr}\left( z_{i} \text{ is compared to } z_{j} \right) \\
 & = \sum\limits_{i=1}^{n} \sum\limits_{j=i+1}^{n} \frac{2}{j - i + 1} \\
 & = 2(n + 1) \underbrace{ \sum\limits_{k=1}^{n - 1} \frac{1}{k + 1} }_{ \Theta(\log n) } - 2(n - 1) \\
 & \implies \mathbb{E}[t_{n}] \in \Theta(n \log n)
\end{align}
$$

## Randomized Quicksort

$$
\text{Quicksort} = \begin{cases}
\Theta(n^{2}), && \text{worst-case} \\
\Theta(n \log n), && \text{average-case} \\
\Theta(n), && \text{best-case (same elements)}
\end{cases}
$$

> [!danger]+ Problem
> - Average-case analysis works only if **each permutation is equally likely**
> - ! In practice, this may not be true:
>     - Often impossible to know what the *input distribution* really is
>     - If person who provides the inputs is malicious, they can provide the worst-inputs and cause worst-case runtime

> [!check]+ Solution
> - Average-case analysis works only if **each permutation is equally likely**
>     - In practice, this may not be true
> - & Shuffle the input array **uniformly randomly**
> - Implication:
>     - & After shuffling, arrays look like drawn from uniform distribution

Then, the assumption in the average-case analysis is true.
So, average-case $\Theta(n \log n)$ is guaranteed.

## Randomized Algorithms

> [!def]+ Las Vegas algorithm
> - The solutions generated by the algorithm is guaranteed to be correct, but runtime depends on random choices
>     - e.g., randomized quicksort

- Another way of randomizing quicksort:
    - Choose pivot randomly
        - (Sections 7.3-4 of CLRS)

> [!def]+ Monte Carlo algorithm
> - Runtime of algorithm is deterministic, but the output is based on random choices

### Las Vegas vs. Monte Carlo

> [!example]+ Binary array where half the elements are 0, and half the elements are 1
> - @ Return index of 1
> - Las Vegas:
>     - Keep picking randomly until we get 1
> - Monte Carlo:
>     - Pick $k$ times
>     - Probability that it is 1 is $1 - (\frac{1}{2})^{k} = 1 - \text{Pr}[\text{all 0's}]$

## After Lecture

- After-lecture Readings: Chapter 5 of the course notes, Chapter 7 of CLRS.
- Problems at the end of Chapter 5 of the course notes.
- Problems 7.1-1, 7.1-4, 7.2-2, 7.2-3 in CLRS.
