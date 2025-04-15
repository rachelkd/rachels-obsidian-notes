---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [De Morgans]
Week: 2
Module:
  - "[[2 - Combinational Circuit Design]]"
Lecture:
  - "6"
Chapter: 
Slides/Notes:
  - "[[circuits_slides.pdf]]"
Date: 2025-01-17
Date created: Fri., Jan. 17, 2025, 11:29:35 am
Date modified: Mon., Feb. 24, 2025, 5:07:38 pm
---

# Reducing Circuits

> [!goal] To minimize the number of gates, we want to reduce the boolean expression as much as possible from a collection of minterms to something similar.

![](https://i.imgur.com/UKbJMo2.png)

- $m_{0} + m_{1} + m_{2} + m_{3}$ simplifies to $\overline{A}$

## Boolean Algebra

> [!thm]+ Axioms
> - $0 \cdot 0 = 0$
> - $1 \cdot 1 = 1$
> - $0 \cdot 1 = 1 \cdot 0 = 0$
> - If $x = 1$, $\overline{x} = 0$

From this, we can extrapolate:

> [!thm]+ Lemma
> $$
> \begin{align}
> x \cdot 0  & = 0  && x + 1 = 1 \\
> x \cdot 1  & = 1 && x + 0 = x \\
> x \cdot x  & = x && x + x = x \\
> x \cdot \overline{x}  & = 0 && x + \overline{x} = 1 \\
> \overline{\overline{x}}  & = x
> \end{align}
> $$

> [!thm]+ Commutative Law
> $$
> \begin{align}
> x \cdot y = y \cdot x && x + y = y + x
> \end{align}
> $$

> [!thm]+ Associative Law
> $$
> \begin{align}
> x \cdot (y \cdot z) & = (x \cdot y) \cdot z \\
> x + (y + z) & = (x + y) + z
> \end{align}
> $$

> [!thm]+ Distributive Law
> $$
> \begin{align}
> x \cdot (y + z)  & = x \cdot y + x \cdot z \\
> x + (y \cdot z) & = (x + y) \cdot (x + z)
> \end{align}
> $$

> [!thm]+ Consensus Law
> $$
> x \cdot y + \overline{x} \cdot z + y \cdot z = x \cdot y + \overline{x} \cdot z
> $$

![](https://i.imgur.com/9K2lUQY.png)

> [!thm]+ Absorption Law
>
> $$
> \begin{align}
> x \cdot (x + y) & = x && x + (x \cdot y) = x
> \end{align}
> $$

> [!thm]+ De Morgan’s Laws
>
> $$
> \begin{align}
> \overline{x} \cdot \overline{y}  & = \overline{x + y} && \overline{x} + \overline{y} = \overline{x \cdot y}
> \end{align}
> $$
> ![](https://i.imgur.com/fe5V1r8.png)

## Converting to NAND Gates

> [!important]+ De Morgan’s Law
> - Out of all of the gates, NANDs are the cheapest to fabricate
> - A sum-of-products ([[Minterms and Maxterms#^b272ac|SOM]]) circuit could be converted into an equivalent circuit of NAND gates
> ![|239](https://i.imgur.com/m76yrde.png)
>
> $$
> \begin{align}
> m_{x} + m_{y} \equiv \overline{\overline{m_{x}}} + \overline{\overline{m_{y}}} \overset{\text{(De Morgan's Law)}}{\equiv} \overline{\overline{m_{x}} \cdot \overline{m_{y}}}
> \end{align}
> $$
>
> > [!note]+ Since $m_{x}, m_{y}$ are sum-of-products i.e., minterms, $\overline{m_{x}}, \overline{m_{y}}$ both use NANDs.
> > ![](https://i.imgur.com/wY1rEzb.png)

## Reduction Goal: Gate Cost

> [!question]+ How do we measure the “simplest” expression?
> - Simple denotes the lowest **gate cost** ($G$) or the lowest **gate cost with NOTs** ($GN$)

![](https://i.imgur.com/ZKm4Kgh.png)

- To calculate the gate cost:
    - Add all gates together, as well as
    - Cost of the NOT gates in the case of the $GN$ cost
- Since we have $\neg A, \neg B, \neg C$ as inputs for some AND gates, we add 3 extra NOT gates for the $GN$ cost

## Reducing Boolean Expressions

- Different final expressions possible depending on what terms you combine
    - e.g., $Y = \bbox[lavender]{\overline{A} \cdot B \cdot C} + \bbox[pink]{A \cdot \overline{B} \cdot \overline{C}} + \bbox[pink]{A \cdot B \cdot \overline{C}} + \bbox[lavender]{A \cdot B \cdot C}$
        - Equivalent to $Y = \bbox[lavender]{B \cdot C} + \bbox[pink]{A \cdot \overline{C}}$ by combining the end terms and middle terms
        - → Reduces the number of gates and inputs

> [!question]+ How do we know which terms to combine?
> - [[Karnaugh Maps]]
