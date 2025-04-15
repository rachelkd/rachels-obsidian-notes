---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 2
Module:
  - "[[2 - Combinational Circuit Design]]"
Lecture:
  - "6"
  - "7"
Chapter: 
Slides/Notes:
  - "[[circuits_slides.pdf]]"
Date: 2025-01-17
Date created: Fri., Jan. 17, 2025, 3:47:23 pm
Date modified: Mon., Jan. 20, 2025, 3:47:42 pm
---

# Karnaugh Maps

![](https://upload.wikimedia.org/wikipedia/commons/0/02/K-map_6%2C8%2C9%2C10%2C11%2C12%2C13%2C14_anti-race.svg)

## Reducing Boolean Expressions

| $A$ | $B$ | $C$ | $Y$     |
| --- | --- | --- | ------- |
| 0   | 0   | 0   | $m_{0}$ |
| 0   | 0   | 1   | $m_{1}$ |
| 0   | 1   | 0   | $m_{2}$ |
| 0   | 1   | 1   | $m_{3}$ |
| 1   | 0   | 0   | $m_{4}$ |
| 1   | 0   | 1   | $m_{5}$ |
| 1   | 1   | 0   | $m_{6}$ |
| 1   | 1   | 1   | $m_{7}$ |

> [!question]+ What rows could we combine with $m_{0}$?
> - $m_{0} + m_{1} \equiv \overline{A} \cdot \overline{B}$
> - $m_{0} + m_{2} \equiv \overline{A} \cdot \overline{C}$
> - $m_{0} + m_{1} + m_{4} + m_{5} \equiv \overline{B}$
>
> > [!obs] Not always clear by looking at the truth table which rows can be combined!

## Karnaugh Maps

> [!def]+ Karnaugh maps (K-maps)
> - Represent the same information as a truth table
> - In a format that helps us see what minterms can be combined
>     - 2D grid of *minterms*
>     - Arranged so that adjacent minterms in the grid differ by a ==single literal==
> - Values in grid are output for that minterm
>
> |                | $\overline{B} \cdot \overline{C}$ | $\overline{B} \cdot C$ | $B \cdot C$ | $B \cdot \overline{C}$ |
> | -------------- | --------------------------------- | ---------------------- | ----------- | ---------------------- |
> | $\overline{A}$ | 0                                 | 0                      | 1           | 0                      |
> | $A$            | 1                                 | 0                      | 1           | 1                      |

- & Can be of any size, and have any number of inputs
    - e.g., the four-input example below

|                                   | $\overline{C} \cdot \overline{D}$ | $\overline{C} \cdot D$ | $C \cdot D$ | $C \cdot \overline{D}$ |
| --------------------------------- | --------------------------------- | ---------------------- | ----------- | ---------------------- |
| $\overline{A} \cdot \overline{B}$ | $m_{0}$                           | $m_{1}$                | $m_{3}$     | $m_{2}$                |
| $\overline{A} \cdot B$            | $m_{4}$                           | $m_{5}$                | $m_{7}$     | $m_{6}$                |
| $A \cdot B$                       | $m_{12}$                          | $m_{13}$               | $m_{15}$    | $m_{14}$               |
| $A \cdot \overline{B}$            | $m_{8}$                           | $m_{9}$                | $m_{11}$    | $m_{10}$               |

> [!goal]+ Adjacent minterms only differ by a single value
> - & Can be grouped into a single term that omits that value

- & Draw boxes over groups of high output values
    - Boxes must be ==rectangular==, and
        - ==Aligned with map==
    - Number of values contained within each box must be ==power of 2==
    - Boxes may ==overlap==
    - Boxes may ==wrap across edges== of map

![](https://i.imgur.com/LDFCpGq.png)

- Once you find the minimal number of boxes that cover all the high outputs:
    - & Create boolean expressions from the inputs that are common to all elements in the box
- e.g., For the table above:
    - Vertical box: $B \cdot C$
    - Horizontal box: $A \cdot \overline{C}$
    - Overall equation: $Y = B \cdot C + A \cdot \overline{C}$
    - ![](https://i.imgur.com/a0Wh2Z3.png)

## Karnaugh Maps and Maxterms

- Can also use Karnaugh maps to group *maxterms* together
- Involves group the ==zero== entries together
    - vs. grouping entries with one values like for *minterms*

## Quick Exercises

|                              | $\overline{C}\,\overline{D}$ | $\overline{C}\,D$ | $C\,D$ | $C\,\overline{D}$ |
| ---------------------------- | ---------------------------- | ----------------- | ------ | ----------------- |
| $\overline{A}\,\overline{B}$ | 0                            | 0                 | ==1==  | ==1==             |
| $\overline{A}\,B$            | ==1==                        | ==1==             | 0      | 0                 |
| $A\,B$                       | ==1==                        | ==1==             | 0      | 0                 |
| $A\,\overline{B}$            | 0                            | 0                 | 0      | 0                 |

- $F = B \cdot \overline{C} + \overline{A} \cdot \overline{B} \cdot C$

|                              | $\overline{C}\,\overline{D}$ | $\overline{C}\,D$ | $C\,D$         | $C\,\overline{D}$ |
| ---------------------------- | ---------------------------- | ----------------- | -------------- | ----------------- |
| $\overline{A}\,\overline{B}$ | 0                            | $1*$              | ==$1*$==       | ==1==             |
| $\overline{A}\,B$            | 0                            | 0                 | ==1==          | ==1==             |
| $A\,B$                       | ==1==                        | $1 \heartsuit$    | $1 \heartsuit$ | 0                 |
| $A\,\overline{B}$            | ==1==                        | 0                 | 0              | 0                 |

- $Y = A \cdot \overline{C} \cdot \overline{D} + A\cdot B\cdot D + \overline{A} \cdot C + \overline{A} \cdot \overline{B} \cdot D$

## Karnaugh Map Review

From lecture 7.

> [!summary]+ Karnaugh Maps
> - Can be of any size
> - Can have any number of inputs
>     - e.g., 4-input example below
>     - ![](https://i.imgur.com/HjCOzND.png)
> - Adjacent minterms only differ by a single value
>     - → Can be grouped into a single term that omits that value

![](https://i.imgur.com/4pcvt64.png)

$$
\begin{align}
Y  & = \underbrace{ \overline{A} \cdot B \cdot C + A \cdot B \cdot C }_{ \text{Vertical group} } + \underbrace{ A\cdot B\cdot \overline{C} + A \cdot \overline{B} \cdot \overline{C} }_{ \text{Horizontal grouping} } \\
 & = B\cdot C + A \cdot \overline{C}
\end{align}
$$

- K-maps provide an *illustration* of a circuit’s minterms (or maxterms)
- Also provide a guide to how neighbour terms may be combined

### Reminder on Reducing Circuits

- Eliminating variables in K-maps by drawing larger ($>1$ element) rectangular groupings results in
    - A circuit with a lower **cost function**
- Resulting expressing is still in **sum-of-products** form
    - If simplified further, it is no longer in SOP i.e., *sum-of-minterms* form

> [!note] It is not only the number of gates that matters when reducing circuits, but also the number of ==inputs== to each gate.

![](https://i.imgur.com/oKNuwBB.png)
