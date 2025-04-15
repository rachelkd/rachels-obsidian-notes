---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 1
Module:
  - "[[1 - Introduction and Analyzing Running Time]]"
Lecture:
  - "2"
Chapter: "2.2"
Slides/Notes:
  - zotero://open-pdf/library/items/XS7QKT8C?page=8&annotation=ZHIB6WK8
Date:
Date created: Thu., Jan. 16, 2025, 9:53:52 pm
Date modified: Thu., Jan. 16, 2025, 10:20:39 pm
---

# Analyzing Algorithms

- **Analyzing** an algorithm:
    - Predicting the *resources* that the algorithm requires
- Resources:
    - Memory, communication bandwidth, computer hardware
    - & Computational time
<!-- break -->
- Assume a generic one-processor **random-access machine** (RAM) model of computation as our implementation technology
    - Algorithms will be implemented as computer programs
    - Instructions are executed one after another, with no concurrent operations
- RAM model contains instructions commonly found in real computers
    - Arithmetic (add, subtract, multiply, divide, remainder, floor, ceiling)
    - Data movement (load, store, copy)
    - Control (conditional and unconditional branch, subroutine call and return)
    - & Each such instruction takes a *constant* amount of time

## Analysis of Insertion Sort

- Time taken by the `INSERTION_SORT` procedure depends on the *input*
    - i.e., Sorting a thousand numbers takes longer than sorting three numbers
    - Can take different amounts of time to sort two input sequences of the same size
        - Depending on how nearly sorted they already are

> [!summary]+ In general, the time taken by an algorithm ==grows== with the size of the ==input==
>
> - → Traditional to describe the running time of a program as a ==function== of the ==size== of its input

> [!def]+ Input size
>
> - Depends on the problem being studied
> - For many problems (sorting, computing discrete Fourier transforms):
>     - ==Number of items in the input==
>     - e.g., Array size $n$ for sorting
> - For other problems e.g., multiplying two integers:
>     - Total number of ==bits== needed to represent the input in ordinary binary notation
> - Sometimes more appropriate to describe size of input with ==two== numbers vs. one
>     - e.g., If algorithm input is a graph, input size can be described by:
>         - Number of ==vertices==, and
>         - Number of ==edges== in graph
> - & Indicate which input size measure is being used with each problem you study!

> [!def]+ Running time
>
> - Number of ==primitive operations== or “steps” executed
