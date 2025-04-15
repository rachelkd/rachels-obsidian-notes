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
  - "5"
Chapter: 
Slides/Notes:
  - "[[circuits_slides.pdf]]"
Date: 2025-01-15
Date created: Fri., Jan. 17, 2025, 11:28:30 am
Date modified: Thu., Jan. 23, 2025, 8:59:11 pm
---

# Minterms and Maxterms

> [!def]+ Minterm
> - An ==AND== expression with every input present in true or complemented form
> - Specifies a row in the truth table where the input values of that row set the output *high*

^844d55

> [!def]+ Maxterm
> - An ==OR== expression with every input present in true or complemented form
> - Specifies a row in the truth table where the input values of that row set the output *low*

^c31634

> [!example]+ Given four inputs $A, \;B, \;C, \;D$,
> - Valid minterms:
>     - $A \cdot \overline{B} \cdot C \cdot D, \;\overline{A} \cdot B \cdot \overline{C} \cdot D, \;A \cdot B \cdot C \cdot D$
> - Valid maxterms:
>     - $A + \overline{B} + C +D, \;\overline{A} + B + \overline{C} + D, \;A + B + C + D$
> - Neither minterms nor maxterms:
>     - $A \cdot B + C \cdot D, \; A \cdot B \cdot D, \;A + B$

## Aside: Boolean Expression Notation

> [!thm]+ AND operations are denoted in these expressions by the ==multiplication== symbol
> - $A \cdot B \cdot C$ or $A ^{*} B ^{*} C \approx A \land B \land C$

> [!thm]+ OR operations are denoted by the ==addition== symbol
> - $A + B + C \approx A \vee B \vee C$

> [!thm]+ NOT is denoted by multiple symbols
> - $\neg A$
> - $A'$
> - $\overline{A}$

> [!thm]+ XOR occurs rarely in circuit expressions
> - $A \oplus B$

## Intuition Behind Minterms

Consider the minterm $m_{15} = A \cdot B \cdot C \cdot D$.

> [!question]+ How do you describe the logical expression $m_{15}$?
> - $m_{15}$ describes the case where the output is low at all times, *except* when $A = 1, B = 1, C = 1, D = 1$
> - ![](https://i.imgur.com/sGYGGUZ.png)

Consider the maxterm $M_{0} = A + B + C + D$

> [!question]+ What is this behaviour?
> - $M_{0}$ is ==always high==, *except* in the one case where all four input values are low
> - ![](https://i.imgur.com/VMkNQcW.png)

## Minterm and Maxterm Notation

> [!important]+ Circuits are often described using minterms or maxterms
> - Form of logic shorthand

- Given $n$ inputs, there are $2^{n}$ minterms and maxterms possible
    - Same as the number of rows in the truth table

> [!note]+ Naming scheme
> - Minterms
>     - Labelled as $m_{x}$
> - Maxterms
>     - Labelled as $M_{x}$
> - $x$ subscript indicates the ==row== in the truth table
> - $x$ starts at 0
>     - When all inputs are low
> - Ends with $2^{n} - 1$

- e.g., If we have three inputs:
    - Minterms are $m_{0} \; (\overline{A} \cdot \overline{B} \cdot \overline{C}$) to $m_{7} \; (A \cdot B \cdot C$)
    - Maxterms are $M_{0}$ ($A + B + C$) to $M_{7}$ ($\overline{A} + \overline{B} + \overline{C}$)

## Converting Table Row to Minterm and Maxterm

Recall from earlier:

![[#^844d55]]

- Consider:
    - ? What expression results in a *high* output for only the first row of the truth table?
        - i.e., When all inputs are zero
        - $Y = \overline{A} \cdot \overline{B} \cdot \overline{C} = m_{0}$

> [!tip]+ Convert the expression to a binary number to get the minterm subscript!
> - e.g., $\overline{A} \cdot \overline{B} \cdot \overline{C} \to 000 \to 0 \implies$ subscript is 0
> - $\overline{A} \cdot B \cdot \overline{C} \to 010 \to 2 \implies$ subscript is 2

![[#^c31634]]

- Consider
    - ? What expression results in a *low* output for only the first row of the truth table?
        - i.e., When all inputs are zero
        - $Y = A + B + C = M_{0}$
        - & Strategy: Take the complement of every input’s state
            - Since first row corresponds to all inputs are zero, the OR expression that would make the first row low/false would be A is high, or B is high, or C is high

### Quick Exercises

> [!question]+ Given 4 inputs $A, B, C, D$, write $m_{9}, m_{15}, m_{16}, M_{2}$.
>
> $$
> \begin{align}
> m_{9} & = A \cdot \overline{B} \cdot \overline{C} \cdot D \\
> m_{15} & = A \cdot B \cdot C \cdot D \\
> m_{16}  & = \text{ does not exist (requires 5 inputs)} \\
> M_{2} & = A + B + \overline{C} + D
> \end{align}
> $$

> [!question]+ Which minterm is $\overline{A} \cdot B \cdot \overline{C} \cdot D$?
> - $0101 \to 5$, so this is $m_{5}$

> [!question]+ Which maxterm is $A + B + C + \overline{D}$?
> - Corresponds to $A=0, B=0, C=0, D=1$, and $0001$ in binary is 1, so this is $M_{1}$

## Minterms into Circuits

> [!question]+ How are minterms used for circuits?
> - A single minterm indicates a ==set of inputs== that will make the output ==high==

- e.g., $m_{2}$
    - Output only goes high in the third line of this truth table
    - (Assuming 4 inputs)
    - ![](https://i.imgur.com/ClcSMD0.png)

> [!question]+ What if we want to combine minterms?
> - Use an ==OR operation==
> - Result is an output that goes high in both minterm cases

- e.g., Consider $m_{2} + m_{8}$
    - Third and ninth lines of this truth table result in high output
    - ![](https://i.imgur.com/tkElaXc.png)

## Combining Minterms and Maxterms

> [!summary]+ Two canonical forms of boolean expressions
>
> > [!info]+ Sum-of-Minterms (SOM)
> > - Each minterm corresponds to a single high output in the truth table
> > - → Combined high outputs are a *union* of these minterm expressions
> > - Expressed in *Sum-of-Products* form
>
> > [!info]+ Product-of-Maxterms (POM)
> > - Each maxterm only produces a single low output in the truth table
> > - → Combined low outputs are an *intersection*
> > - Expressed in *Product-of-Sums* form

^b272ac

> [!note]+ Minterm and maxterm expressions are used for ==efficiency== reasons
> - More compact than displaying entire truth tables
> - & Sum-of-minterms are useful in cases with very ==few input combinations that produce high output==
> - & Product-of-maxterms are useful when expressing truth tables that have very ==few low output cases==

### Example. SOM with $Y = m_{2} + m_{6} + m_{7} + m_{10}$

![](https://i.imgur.com/3I29Rh5.png)

### Example. POM with $Y = M_{3} \cdot M_{5} \cdot M_{7} \cdot M_{10} \cdot M_{14}$

![](https://i.imgur.com/h1QszuK.jpeg)

## Converting SOM to Gates

> [!tip]+ Once you have a sum-of-minterms expression, it is easy to convert this to the equivalent combination of gates.
> ![](https://i.imgur.com/oR1Ugnb.png)
