---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Binary counters, Dynamic arrays, Multipop stack]
Week: 10
Module:
  - "[[7 - Amortized Analysis]]"
Lecture:
  - "19"
  - "20"
Chapter: 
Slides/Notes: 
Date: 2025-03-18
Date created: Tue., Mar. 25, 2025, 2:59:20 am
Date modified: Thu., Mar. 27, 2025, 9:36:51 pm
---

# Amortized Analysis

> [!info]+ Operations do not stand alone!
> Often:
> - We perform *sequences* of operations on data structures
> - Are interested in the time complexity of the *entire sequence*

> [!def]+ Worst-case sequence complexity (WCSC for $m$ operations)
> Maximum time over all real sequences of $m$ operations
> - $\text{WCSC} \leq m \times \text{worst case time complexity of any one operation in a sequence of } m \text{ operations}$

## Amortized Sequence Complexity

> [!def]+ Amortized sequence complexity (of a single operation)
> $$\text{ASC} = \frac{\text{WCSC}}{m}$$
> - Amortized complexity of an operation
> - Represents the “average” worst-case complexity of each operation
> - Different than average-case time
>     - ! No probability!
> - Often makes more sense in real situation

### Approaches to Calculating Amortized Complexity

> [!summary]+ Three approaches
> - & **Aggregate**
>     - Add together the costs for $m$ operations
>     - Divide by $m$
>     - ^ Works well when there is only *one* operation
>     - Brute force approach
> - & **Accounting**
>     - Charge each operation a *fee* (represents the *amortized cost*)
>     - Pay the real cost
>     - Save the overcharge to pay for future costs when the fee is not enough
>         - Save the overcharge on a specific element in the data structure
> - **Potential**
>     - Metaphor of “potential energy”
>     - Like accounting except extra “energy” is associated with the whole data structure
>         - Not a specific element
>     - Not covered in [[CSC263]]

## Binary Counters

Suppose we are counting up with a binary counter.

- ? When does 1‘s digit get flipped?
    - $m$, every time
- ? When does the 2’s digit get flipped?
    - $\frac{m}{2}$
- ? When does the 4‘s digit get flipped?
    - $\frac{m}{4}$

### Binary Counters: Aggregate Approach

- **Binary counter**
    - Sequence of $k$ bits with single operation `Increment`
    - $k$ is fixed

```python
Increment(A)
    i <- 0
    while i < length[A] and A[i] = 1
        do A[i] <- 0
        i <- i + 1
    if i < length[A]
        then A[i] <- 1
```

| Initial counter   | 00000 | Value | Cost |
| ----------------- | ----- | ----- | ---- |
| After `Increment` | 00001 | 1     | 1    |
| After `Increment` | 00010 | 2     | 2    |
| After `Increment` | 00011 | 3     | 1    |
| After `Increment` | 00100 | 4     | 3    |
| After `Increment` | 00101 | 5     | 1    |
| …                 | …     | …     | …    |

| Bit number | Changes/flips       | Total changes in $m$ operations |
| ---------- | ------------------- | ------------------------------- |
| 0          | Every time          | $m$                             |
| 1          | Every 2 times       | $\frac{m}{2}$                   |
| 2          | Every 4 times       | $\frac{m}{4}$                   |
| 3          | Every 8 times       | $\frac{m}{8}$                   |
| …          | …                   | …                               |
| $i$        | Every $2^{i}$ times | $\frac{m}{2^{i}}$               |

- & Sum up the *total* changes in $m$ operations

$$
\begin{align}
\sum\limits_{i=0}^{k-1} \frac{m}{2^{i}} & = m \sum\limits_{i=0}^{k-1} \frac{1}{2^{i}} \\
 & < m \sum\limits_{i=0}^{\infty} \frac{1}{2^{i}} \\
 & = 2m && \left( \text{geometric sum: } \sum\limits_{i=0}^{\infty} a_{1}(r)^{i-1} = \frac{a_{1}}{1-r}, r < 1 \right)
\end{align}
$$
- Running time for $m$ operations
    - $\mathcal{O}(2m)$
- Amortized complexity for a *single* operation
    - $\mathcal{O\left( \frac{2m}{m} \right)} = \mathcal{O}(1)$

## Accounting Approach

- Charge each operation an amortized cost — the *fee*
    - Fee can be more or less than actual cost
    - Fee can be different for different *types* of operations
- When amortized cost $\hat{C_{i}} >$ actual cost $C_{i}$
    - Credit is assigned to an associated element in the data structure
- When amortized cost $<$ actual cost
    - Difference must be paid by existing credit
- ! Can never be in debt

$$\sum\limits_{i=0}^{k} \hat{C_{i}} \geq \sum\limits_{i=0}^{k} C_{i}, \forall k $$
e.g.,
$$\hat{C}_{3} + \hat{C}_{2} + \hat{C}_{1} \geq C_{1} + C_{2} + C_{3}$$
- % Fee can be less than the actual cost of a single operation, but the sum of all fees has to be $\geq$ actual cost of all operations

### Multipop Stack

- **Multipop stack**
    - A *stack* with the regular `Push` and `Pop` operations
        - Each take $\mathcal{O}(1)$ time
    - With additional `Multipop` operation

```python
Multipop(S, k)
    while not Stack-Empty(S) and k != 0
        do Pop(s)
        k <- k - 1
```

#### Multipop Stack: Accounting Approach

|            | `Push(S, x)` | `Pop(S)` | `Multipop(S, k)` |
| ---------- | ------------ | -------- | ---------------- |
| Costs      | 1            | 1        | $min(\|S\|, k)$  |
| Fee/charge | $2           | $0       | $0               |

> [!thm]+ Credit invariant
> - Every element in the stack as a $1 credit

***Proof (by induction on number of operations).***

**Base case.**

- 0 operations
- Empty stack has 0 elements and 0 credits
- Trivially true

**Induction step.**

Assume: After $k$ ops, every element in stack holds $1 credit.

> [!note]+ Need to show for every operation

`Push`:
Charge $2, but cost is only $1, so we store the leftover on the newly-pushed item.

`Pop`:
Charge $0, but cost is $1.
We pay for this pop with the stored $1 on the item we are popping.
Every remaining item keeps its dollar.

`Multipop`:
Charge $0, but cost is $1 per item that gets popped.
Pay for each item with its stored $1.

Since invariant implies credit is always $\geq 0$, fee is enough to cover cost.
So, the amortized cost is $\leq 2$ per operation.

> [!note]+ This is $\mathcal{O}(1)$.
> - Amortized cost is $\leq 2$ per operation
> - After $m$ operations, total is $\leq 2m$
> - $\mathcal{O}\left( \frac{2m}{m} \right) = \mathcal{O}(1)$

### Binary Counter: Accounting Approach

```python
Increment(A)
    i <- 0
    while i < length[A] and A[i] = 1
        do A[i] <- 0
        i <- i + 1
    if i < length[A]
        then A[i] <- 1
```

- Line 3
    - Flip 1 → 0
- Line 6
    - Flip 0 → 1
    - Happens at most 1 time

<!-- break -->
- & Charge each operation $2
    - $1 to flip from 0 → 1
    - Use stored credits to flip from 1 → 0
    - $1 extra credit
        - Store with the bit that just changed to 1

> [!thm]+ Credit invariant
> - Every 1 bit in the counter holds a $1 credit

***Proof (by induction on calls to `Increment`).***

**Base case.**

- Initially, the counter is 0
- There is no credit
- → Trivially true

**Induction step.**

- Assume invariant is true
- Call `Increment`
- Cost of flipping 1 → 0 (line 3) will be paid for by the $1 credit on that 1
- At most, only one zero will be flipped to a 1 (in line 6)
    - Cost will be covered by the first dollar of the fee
    - Remaining $1 is left on that 1
- All other bits are unchanged

$\therefore$ Invariant holds.

> [!note]+ This shows that the total charge for a sequence of $m$ operations is $2m$
> - Total charge is an *upper bound* on total cost
> - $\implies$ Amortized cost per operation is $\leq \frac{2m}{m} = 2$
> - $\implies \mathcal{O}(1)$

## Dynamic Arrays

```python
def insert(A, x):
    # Check whether current array is full
    if A.size == A.allocated:
        new_array = new array of length (A.allocated * 2)
        copy all elements of A.array into new_array
        A.array = new_array
        A.allocated = A.allocated * 2
    
    # Insert the new element into the first empty spot
    A.array[A.size] = x
    A.size = A.size + 1
```

- Line 5:
    - 2 array accesses
        - One to read
        - One to insert into new array
- Line 10:
    - 1 array access
        - Insert new item into new array
- % `size`: Number of elements in the array
- % `allocated`: Space in array that is created already
- % **Expansion factor** is 2

### Dynamic Arrays: Copying Elements

![](https://i.imgur.com/fFYr0sV.png)

### Dynamic Arrays: Aggregate Method

Define $t_{k}$:
$$
t_{k} = \begin{cases}
2k + 1, && k\text{ is a power of } 2 \\
1, && \text{otherwise}
\end{cases}
$$

Define $t_{k}'$:
$$
t_{k}' = \begin{cases}
2k, && k \text{ is a power of } 2 \\
0, && \text{otherwise}
\end{cases}
$$

Time for $n$ operations:

$$
\begin{align}
T(n)  & = \sum\limits_{k=0}^{n - 1} t_{k} \\
 & = \sum\limits_{k=0}^{n - 1} 1 + \sum\limits_{k=0}^{n-1} t_{k}' \\
 & = \underbrace{ n }_{ \text{Line 10's} } + \underbrace{ \sum\limits_{k=0}^{n - 1} t_{k}' }_{ \text{Line 5's} } \\
 & = n + \sum\limits_{i=0}^{\lfloor \log(n - 1) \rfloor } t_{(2^{i})}' \\
 & \leq n + \sum\limits_{i=0}^{\log(n - 1)} t_{2^{i}}' \\
 & = n + \sum\limits_{i=0}^{\log(n - 1)} 2 \cdot 2^{i} \\
 & = n + 2 \sum\limits_{i=0}^{\log(n - 1)} 2^{i} \\
 & = n + 2\left( 2^{\log(n - 1) + 1} - 1 \right) && \left( \sum\limits_{i=0}^{b} 2^{i} = 2^{b + 1} - 1; \text{think about binary numbers} \right) \\
 & = n + 2(2 \cdot 2 ^{\log(n - 1)} - 1) \\
 & = n + 2(2 (n - 1) - 1) \\
 & = 5n - 6 \\
 & = \mathcal{O}(n)
\end{align}
$$
- $5n - 6$ is the total for $n$ operations
- Amortized cost for a single operation:
    - $\frac{\mathcal{O}(n)}{n} = \mathcal{O}(1)$

### Dynamic Arrays: Accounting Method

- & Conduct an amortized analysis of insertions into a dynamic array where the expansion factor is 2
- When we calculated the worst-case performance, we decided to count array accesses
    - Only happened on lines 10 and 5
    - When underlying array had room for a new element:
        - Insert only required one access on line 10
    - When underlying array was full with $n$ elements:
        - Inserting one more required $2n$ accesses on line 5
        - $+1$ on line 10

> [!question]+ Do the analysis using a charge of $2 for each insert. Is this enough of a charge?
> ![](https://i.imgur.com/Yw10iNf.png)

> [!question]+ Try again with $3 per insert. That will leave us a $2 credit on the first item inserted. Intuitively, that feels like it will be enough to do the copy when we have to copy this item to the larger array. Can you prove that this will work and the data structure will never be in debt?
> ![](https://i.imgur.com/s0URP6V.png)

> [!question]+ Try again with $5.
> ![](https://i.imgur.com/lUr3jZ2.png)

> [!thm]+ Credit invariant
> - Every element in the 2nd half of the array has a credit of $4

***Proof (by induction).***

**Base case.**

- 0 operations → Nothing in the array → Trivially true

**Induction step.**

- Assume CI holds (IH).
- & At the point when we need to expand, array is full
    - By IH, the CI holds, so every element in the back half has $4.
        - That can be used to pay for itself, and
        - Pay for one element in the front half in the copying operation on line 5
- Then, line 10 needs only $1 of the $5 fee, leaving $4 on the only element in the back half

→ Conclude that amortized cost $\leq 5$, which is $\mathcal{O}(1)$.
