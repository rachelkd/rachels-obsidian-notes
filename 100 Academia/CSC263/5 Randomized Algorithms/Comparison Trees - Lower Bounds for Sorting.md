---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [comparison trees]
Week: 12
Module:
  - "[[5 - Randomized Algorithms]]"
Lecture:
  - "24"
Chapter: 
Slides/Notes: 
Date: 2025-04-03
Date created: Tue., Apr. 15, 2025, 4:48:59 pm
Date modified: Tue., Apr. 15, 2025, 5:43:47 pm
---

# Comparison Trees: Lower Bounds for Sorting

## Discover Comparison Trees

- So far:
    - Have focused our analyses on the complexity of *individual* algorithms, or *sequences* of operations for a single data structure ([[Amortized Analysis]])
    - That is:
        - Given an algorithm $A$ to solve a problem $P$, we have considered the question:
            - ? What is the best (*minimum*) function $f(n)$ for which we can guarantee that $A$ solves $P$ in time $\mathcal{O}(f(n))$

We are changing out POV slightly and asking:

> [!question]+ Given a problem $P$, how efficiently can we solve $P$ if we are allowed to use *any* algorithm and data structure we want?

- Analyses we have carried out so far provide us with *upper bounds* on the *complexity* of the problem $P$
    - When we find one algorithm $A$ that solves $P$ with worst-case running time in $\mathcal{O}(f(n))$:
        - Know that $P$ can be solved using no more than time $f(n)$
- & We want to know about **lower bounds on problem complexity**
    - @ Prove that *every* algorithm that solves problem $P$ requires time $\Omega(g(n))$, for some function $g(n)$

> [!note]+ In contrast to upper bounds, we want our lower bounds $g(n)$ to be as *large* as possible
> - Not helpful to say “every algorithm requires time $\Omega(1)$”
> - When we discuss upper and lower bounds:
>     - @ Try to find bounds that match
>     - If we have an algorithm with worst-case running time $O(f(n))$ and can show matching lower bound (i.e., every algorithm takes at least $\Omega(f(n))$ to solve the problem):
>         - & Know algorithm is **optimal** with respect to worst-case time complexity
>         - e.g., we know sorting algorithms that run in worst-case time $\mathcal{O}(n\log n)$, and we will see that any *comparison-based* sorting algorithm requires time $\Omega(n\log n)$

> [!tip]+ We often prove *lower bounds* only for a certain **class** of algorithms
> - Can be done by specifying the types of operations that algorithm is allowed to perform

We focus on comparison-based algorithms.

- **Comparison-based algorithms**
    - Those whose primary operation is to *compare input values* with each other
- % This class of algorithms is useful for sorting, searching, etc.
- Standard sorting algorithms are all comparison-based
    - e.g., mergesort, quicksort, insertion sort
- & Comparison-based algorithms can be represented in the form of **comparison trees**
    - Also known as *decision trees*

### Comparison Trees

> [!example]+ Consider the problem solved by the binary search algorithm.
> - **Problem:**
>     - Search in a sorted list
> - **Input:**
>     - A sorted array $A[1\dots n]$
>     - A value $x$
>     - % We consider valid array indices to be in the range $[1, n]$ instead of $[0, n-1]$
> - **Output:**
>     - Index $k$ if $A[k] = x$
>     - Otherwise, 0 if $x$ does not appear in $A$
> - Consider behaviour of the binary search algorithm on an arbitrary input $(A, x)$ with $\text{len}(A) = 3$
>     - Algorithm first compares $A[2]$ with $x$
>     - On the basis of result:
>         - Algorithm then compares $x$ with $A[1]$ or $A[3]$
>     - After second comparison:
>         - Only one index can possibly be equal to $x$
>         - → Check to see if value at this index is $x$ and return the output
> - This sequence of comparisons performed by algorithm can be summarized by the following comparison tree:
>     - ![](https://i.imgur.com/9XUgj6Q.png)

> [!note]+ Convention
> - & Use the same comparison operator in every node when possible
>     - → Bottom-most nodes are written using $\leq$ instead of $=$

> [!note]+ Things to note about this comparison tree
> - Internal nodes (nodes with children) represent *comparisons* made by algorithm during its computation
>     - Root is first comparison
>     - Its children are next comparison (depending on result of first)
>     - And so on
> - Edges show the *outcome* of each comparison
> - External nodes (leaves) show the *output* of algorithm
>     - Okay for different nodes to have same value
>     - Represents different ways that algorithm can arrive at same output

- For any specific input:
    - Execution of algorithm can be traced in decision tree as a path from *root* to a *leaf* node
    - e.g., if $A = [1, 5, 10]$ and $x = 4$:
        - Path in tree pictured above is $[4 \leq 5, \text{True}, 4 \leq 1, \text{False}, 5 \leq 4, \text{False}, 0]$
- Number of comparisons performed by algorithm on any input $=$ *length of its execution path*

> [!thm]+ Conclusions
> - Worst-case time complexity is proportional the the *height* of the tree
> - Average-case time complexity is proportional the to *average depth* of the leaves

- % A single comparison tree can only represent behaviour of an algorithm for a fixed input size
    - But captures all possible executions of the algorithm for every input of that size
    - → Each input size corresponds to a different tree (with a common structure)
    - → One algorithm corresponds to an infinite family of comparison trees (one for each input size)

## Lecture Preamble

> [!question]+ How fast can we sort?
> - Existence of algorithms that run in worst-case time $\mathcal{O}(n \log n)$ confirm that sorting can be done in time $\mathcal{O}(n \log n)$, but does not rule out the *existence of algorithm that could do better*
> - We know how to analyze worst-case complexity of algorithms
>     - Worst-case complexity of **problems** involves extra work

For a problem $P$, let $C(P)$ be the **best worst-case running time of any algorithm that solves $P$**.

- Upper bound on $C(P)$:
    - Give an algorithm and analyze its time
- Lower bound on $C(P)$:
    - ???
    - $ Every algorithm requires a certain amount of time

<!-- break -->
- In practice:
    - & Prove lower bounds on *classes* of algorithms

## Comparison Trees

- **Comparison trees**
    - Represent algorithms that work by *pairwise comparison of elements*
    - e.g., binary search in $A[1\dots3]$ (sorted), returning index of $x$ or 0

![](https://i.imgur.com/9XUgj6Q.png)

Consider the comparison tree above and suppose $A = [3, 5, 8]$.

- ? What is the worst-case number of comparisons?
- ? What is the expected number of comparisons if we select $x$ uniformly from $\{ 1, \dots, 10 \}$?
- ? What if we select $x$ uniformly from $\{ 1, \dots, 100 \}$?