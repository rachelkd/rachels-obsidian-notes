---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
Module:
  - "[[1 - Introduction and Analyzing Running Time]]"
Week: 1
Lecture:
  - "1"
  - "2"
Chapter: "1"
Slides/Notes:
  - https://share.goodnotes.com/s/3HJKZQMRwREbWGzPhZiC7M
Date: 2025-01-09
aliases: []
Date created: Thu., Jan. 9, 2025, 2:12:04 pm
Date modified: Tue., Apr. 15, 2025, 3:24:37 pm
---

# Algorithm Analysis

## Abstract Data Types and Data Structures

> [!def]+ Abstract Data Type (ADT)
>
> - A set of *objects* together with a set of *operations* on these objects
> - Theoretical model of an entity and the set of operations that can be performed on that entity (Course Notes)

- & Keyword: *Abstract*
    - Can be understood and communicated without any code at all
- e.g., Stack ADT:
    - `PUSH(S, v)`: Add element $v$ to the collection $S$
    - `POP(S)`: Removes the most recently added element that was not yet removed
    - `ISEMPTY(S)`: Returns whether the collection $S$ is empty

> [!def]+ Data Structure
>
> - An ==implementation== of an ADT
> - Value in a program which can be used to store and operate on data (Course Notes)

- e.g., Data structures for Stack:
    1. Linked list (Keep pointer to head)
        - `ISEMPTY`: `test head == None`
        - `PUSH`: Insert at the front of the list
        - `POP` Remove front of the list (if not empty)
    2. Array with counter (Size of stack)
        - `ISEMPTY`: `test counter == 0`
        - `PUSH`: Insert at front of array and increase counter
        - `POP`: Remove front of array (if not empty) and decrease counter

## Review

> [!def]+ Complexity
>
> - Amount of ==resources== required for running an algorithm
>     - Measured as a *function* of *input size*

- Resources:
    - ==Running time==, or
    - ==Memory space==

<!-- break -->
- **Time complexity**
    - Number of ==steps== executed by an algorithm
- **Space complexity**
    - Number of ==units of space== required by an algorithm
        - e.g., Number of elements in a list,
        - Number of nodes in a tree
- **Running-time analysis**
    - Relationship between an algorithm’s ==input size== and the number of ==basic operations== the algorithm performs
- **Basic operation**
    - Any operation whose run-time ==does not depend== on the input size
        - e.g., Arithmetic operations, assignments, array accesses, comparisons, return statements, etc.

> [!important] We do not try to precisely quantify the exact number of basic operations.

## Measuring Running Time by Counting Steps

- & Represent as a function $T(n)$ of *input size* $n$
- Do not care about exact step counts
    - Estimate for $T(n)$ is sufficient
- Every **chunk** of instructions is represented by a constant
- **Chunk**
    - Sequence of instructions that always gets executed together

> [!note]+
> Runtime can be measured by:
>
> - Counting the number of times ==all lines== are executed, or
> - Number of times ==some important lines== are executed
>
> > [!tip]+ It is up to the problem, or what the question asks.
> >
> > - Read the question carefully!

- Often: No simple algebraic expression for $T(n)$
    - & We try to prove bounds on $T(n)$ using **asymptotic notation**

> [!thm]+ Asymptotic notation
>
> - **Upper bound**
>     - $T(n) \in \mathcal{O}(f(n))$
> - **Lower bound**
>     - $T(n) \in \Omega(f(n))$
> - **Tight bound**
>     - $T(n) \in \Theta(f(n))$
>     - $T(n) \in \mathcal{O}(f(n))$ and $T(n) \in \Omega(f(n))$

## Big-O Notation Rules

> [!thm] If $T(n)$ is a *polynomial* of degree $k$, then $T(n) \in \mathcal{O}(n^{k})$.

> [!thm] If $T(n) = g(n) + f(n)$ and $f(n)$ *asymptotically dominates* $g(n)$, then $T(n) \in \mathcal{O}(f(n))$.

![|691](https://i.imgur.com/a7ok9iO.png)

## Different Cases of Running Time

Let $t(x)$ represent the ==number of steps== executed by an algorithm $A$ on input $x$.

> [!def]+ Worst-case running time of $A$
>
> - *Maximum* running time of $A$ for all inputs of size $n$
>
> $$
> T(n) = max\{ t(x) : x \text{ is an input of size }n \}
> $$

> [!def]+ Best-case running time of $A$
>
> - *Minimum* running time of $A$ for all inputs of size $n$
>
> $$
> T(n) = min\{ t(x) : x \text{ is an input of size }n \}
> $$

> [!def]+ Average-case running time of $A$
>
> - *Expected* running time of $A$ for all inputs of size $n$
>
> $$
> T(n) = \mathbb{E}[t_{n}]
> $$

## Worst-Case Running Time Analysis

1. Find the **input size** $n$
    - For numbers: number of bits
    - For lists: number of elements
    - For graphs: number of vertices and/or edges
2. Identify case in which the ==performance== of the algorithm is *worst*
    - i.e., Takes longer to terminate
3. Give an *approximation* of number of basic operations that execute in the case
    - Denote by $T(n)$
4. Give an **upper**/**lower**/**tight-bound** for $T(n)$

### Example. `LinkedSearch`

$L$ is a linked list.

```python
def LinkedSearch(L):
    z = L.head
    while z != None and z.key != 42:
        z = z.next
    return z
```

- Input size:
    - $n = len(L)$
- What is the worst case?
    - Line 4 executes $n$ times
    - Happens when 42 is not in the list
        - Witness for $\Omega$
- Worst-case run time:
    - $\Omega(n)$
- Upper/lower/tight-bound for $T(n)$:
    - $\mathcal{O}(n), \Omega(n), \Theta(n)$

### Example. `EvilEvens`

$L$ is a list.

```python
def EvilEvens(L):
    if every number in L is even:
        repeat L.length times:
            calculate and print the sum of L
        return 1
    else:
        return 0
```

- Input size:
    - $n = len(L)$
- What is the worst-case?
    - When every element of $L$ is even
- Worst-case run-time:
    - All even: $T(n) = n^{2} + n$
        - $n^{2}$ term is for lines 3-4
        - The check on line 2 takes $\Theta(n)$ time
    - Not all even: $T(n) = n$

## Aside: Bounds v.s. Cases

> [!warning]+ Common Misconceptions
>
> - “$\mathcal{O}$ is for describing worst-case running time”
> - “$\Omega$ is for describing best-case running time”

- & $\mathcal{O}, \Omega$ specify bounds over a ==mathematical function==
- & Worst-case and best-case correspond to *algorithms*
- $\mathcal{O}, \Omega$ can both be used to upper-bound and lower-bound the WC running time
    - Same with best-case running time

## Average-Case Running Time Analysis

> [!summary]+ Computing average-case running time for an algorithm $A$
>
> 1. Define $S_{n}$
>     - Space of ==all inputs== of size $n$
> 2. Assume a *probability distribution* over $S_{n}$
>     - Specifying likelihood of each input
> 3. Define the *random variable* $t_{n}$ over $S_{n}$, representing the running time of $A$
>     - $t_{n}(x)$ is the number of ==steps== executed by $A$ on an input $x$ in $S_{n}$
> 4. Compute the *expected* value of $t_{n}(x)$
> $$\mathbb{E}[t_{n}] = \sum_{i} i \times Pr[t_{n} = i]$$
>
> - $i$ takes on ==all possible times==
> - $Pr[t_{n} = i]$ is the ==probability that the algorithm takes time $i$== (according to the probability distribution)

^dffdb9

```python title:LinkedSearch
def LinkedSearch(L):
    z = L.head
    while z != None and z.key != 42:
        z = z.next
    return z
```

- LinkedSearch takes on the times $1$ to $n+1$
    - $n + 1$ handles the case that 42 is not in the list

> [!tip]+ To know $Pr[t_{n} = i]$, we need to know the ==probability distribution== on the inputs
>
> - e.g., By specifying how inputs are generated

- Example distribution:
    - For each key in the linked list, we pick an integer between 1 and 100 inclusive, independently, uniformly at random (from some universe)
    - Chance that number is 3: $\frac{1}{100}$
    - Change that number is 4: $\frac{1}{100}$
    - Chance that number is 3 again: $\frac{1}{100}$

### Example. `LinkedSearch`

$$
S_{n} = \{ L:L \text{ is a list of size }n \text{ that includes integers from 1 to 100} \}
$$
Then,
$$
\begin{align}
Pr(t_{n} = 1)  & = \frac{1}{100}  &  &  \text{(found 41)} \\
Pr(t_{n} = 2)  & = \frac{99}{100} \cdot \frac{1}{100}  &  &  \text{(not 42, then is 42)} \\
Pr(t_{n} = k)  & = \left( \frac{99}{100} \right)^{k-1} \cdot \frac{1}{100}
\end{align}
$$
Computing the expected value of $t_{n}$,
$$
E[t_{n}] = \sum\limits_{i=1}^{n+1} i \cdot Pr(t_{n} = i)
$$
Note that the final iteration is the case when 42 is not in the list, which has probability $\left( \frac{99}{100} \right)^{n}$.

$$
\begin{align}
 & = \sum\limits_{i=1}^{n} i \cdot \left( \frac{99}{100} \right)^{i-1}\left( \frac{1}{100} \right) + \left( \frac{99}{100} \right)^{n}\left( n+1 \right)  \\ \\
 & = \sum\limits_{i=1}^{n} i \cdot p^{i - 1}(1-p) + (n + 1) p^{n}  &  &  \left( \text{Let } p=\frac{99}{100} \iff 1-p = \frac{1}{100} \right) \\
 & = \frac{1 - p}{p} \sum\limits_{i=1}^{n} ip^{i} + (n + 1)p^{n}  \\
 & = \frac{\cancel{ 1-p }}{\cancel{ p }} \left[ \frac{p^{n\cancel{ +1 }}(n(p-1)-1)+\cancelto{ 1 }{ p }}{(1-p)^{\cancel{ 2 }}} \right] + (n + 1) p^{n}  && \left( \sum\limits_{k=1}^{n} ka^{k} =\frac{(a^{n+1}(n(a-1)-1)+a)}{(1-a)^{2}} \right) \\
 & = \frac{1}{1-p} \left[ p^{n}(np - n - 1) + 1 + p^{n}(n+1)(1-p) \right]  \\
 & = \frac{1}{1-p} \left[ p^{n}(np - n - 1) + 1 + p^{n}(n + 1 - p - np) \right]  \\
 & = \frac{1}{1-p} \left( 1- p^{n}p \right)  \\
 & = \frac{1}{1-p}(1 - p^{n + 1}) \\
 & = 100 - \underbrace{ \left( \frac{99}{100} \right) ^{n} }_{ \text{As }n \to \infty, \text{ this term }\to \, 0 } \cdot 99 \\
 & = \Theta(1)
\end{align}
$$
