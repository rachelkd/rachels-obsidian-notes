---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 3
Module:
  - "[[3 - Logic Devices]]"
Lecture:
  - "9"
Chapter:
Slides/Notes:
  - "[[devices_slides.pdf]]"
Date: 2025-01-24
Date created: Fri., Jan. 24, 2025, 11:42:23 am
Date modified: Sat., Feb. 22, 2025, 5:24:30 pm
---

# Comparators

> [!def]+ Comparator
>
> - Circuit that takes in two input vectors
> - Determines if first is greater than, less than, or equal to the second

## Basic Comparators

![|150](https://i.imgur.com/NdRHc4B.png)

- Consider two binary numbers $A$ and $B$
    - $A,B$ are one bit long
- Circuits for this would be:
    - `A == B`: $A \cdot B + \overline{A} \cdot \overline{B}$
    - `A > B:` $A \cdot \overline{B}$
    - `A < B`: $\overline{A} \cdot B$

| $A$ | $B$ |
| --- | --- |
| 0   | 0   |
| 0   | 1   |
| 1   | 0   |
| 1   | 1   |

- ? What if $A$ and $B$ are two bits long?
    - ![|150](https://i.imgur.com/nuoVtlQ.png)
    - Terms for this circuit have to ==expand== to reflect ==second signal==
    - e.g., `A == B`: $(\bbox[ pink ]{ A_{1} \cdot B_{1} + \overline{A_{1}} \cdot \overline{B_{1}} }) \cdot (\bbox[ lavender ]{ A_{0} \cdot B_{0} + \overline{A_{0}} \cdot \overline{B_{0}} })$
        - Pink term makes sure values of bit 1 are the same
        - Purple term makes sure that values of bit 0 are the same

<!-- break -->
- ? Check if $A > B$ or $A < B$?
    - Check if first bit satisfies condition
    - If not, check that first bits are equal, AND
    - Do the one-bit comparison

| Comparison | Check if first bit satisfies   | If not, check that first bits are equal <u>AND</u>                           | One-bit comparison                            |
| ---------- | ------------------------------ | ---------------------------------------------------------------------------- | --------------------------------------------- |
| $A > B$    | $A_{1} \cdot \overline{B_{1}}$ | $\left( A_{1} \cdot B_{1} + \overline{A_{1}} \cdot \overline{B_{1}} \right)$ | $\left( A_{0} \cdot \overline{B_{0}} \right)$ |
| $A < B$    | $\overline{A_{1}} \cdot B_{1}$ | $\left( A_{1} \cdot B_{1} + \overline{A_{1}} \cdot \overline{B_{1}} \right)$ | $\left( \overline{A_{0}} \cdot B_{0} \right)$ |

- $A > B$:
    - $A_{1}\cdot \overline{B_{1}} + \left( A_{1} \cdot B_{1} + \overline{A_{1}} \cdot \overline{B_{1}} \right)\left( A_{0} \cdot \overline{B_{0}} \right)$
- $A < B$:
    - $\overline{A_{1}} \cdot B_{1} + \left( A_{1} \cdot B_{1} + \overline{A_{1}} \cdot \overline{B_{1}} \right)\left( \overline{A_{0}} \cdot B_{0} \right)$

> [!obs]+ This gives us the formulas for a two-digit comparator.
>
> - Note the sections they have in common
>
> ![](https://i.imgur.com/lf3UVhf.png)

- Equality case is common → Came up with a name for this expression called $X$

## General Comparators

- General circuit for comparators requires you to define equations for each case
- Cases:
    - **Equality**
        - If inputs $A, B$ are equal, then all bits must be the same
        - Define $X_{i}$ for any digit $i$
                $$
                X_{i} = A_{i} \cdot B_{i} + \overline{A_{i}} \cdot \overline{B_{i}}
                $$
    - **$A > B$**
        - First non-matching bits occur at bit $i$, where $A_{i} = 1$ and $B_{i} = 0$
        - All higher bits match
                $$
                A > B = A_{n} \cdot \overline{B_{n}} + X_{n} \cdot A_{n-1} \cdot \overline{B_{n-1}} + \dots + A_{0} \cdot \overline{B_{0}} \cdot \prod_{k=1}^{n} X_{k}
                $$
    - **$A < B$**
        - First non-matching bits occur at bit $i$, where $A_{i} = 0$ and $B_{i} = 1$
        - All higher bits match
                $$
                A < B = \overline{A_{n}} \cdot B_{n} + X_{n} \cdot \overline{A_{n-1}} \cdot B_{n-1} + \dots + \overline{A_{0}} \cdot B_{0} \cdot \prod_{k=1}^{n} X_{k}
                $$

## Comparator Truth Table

- Given two input vectors of size $n = 2$, output of circuit is shown below:
    - ![](https://i.imgur.com/X4X3lCh.png)

### Karnaugh Maps for Comparators

![](https://i.imgur.com/lbk25zw.png)

![](https://i.imgur.com/m9zOeQm.png)

![](https://i.imgur.com/4sjwY1H.png)

## Comparing Larger Numbers

- Numbers get larger → Comparator circuit gets more complex
- Can be easier sometimes just to process the result of a ==subtraction operation== instead
    - Easier, less circuitry, just not faster
