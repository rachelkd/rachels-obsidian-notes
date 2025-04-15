---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
Module:
  - "[[1 - Transistors]]"
Week: 1
Lecture:
  - "2"
Chapter: 
Slides/Notes:
  - "[[tutorial_lab1_slides.pdf]]"
Date: 2025-01-08
aliases: []
Date created: Wed., Jan. 8, 2025, 1:41:14 pm
Date modified: Sat., Jan. 18, 2025, 3:33:33 pm
---

# Lab 1 Preparation

## Part 1: Multiplexer

> [!goal] Design the circuit for a multiplexer

- $F = X \bar{S} + YS$
    - X and not S *or* Y and S
    - $\equiv X * S' + Y * S$
    - $\equiv \big(X \text{ and }(\text{not }S)\big)$ or $(Y \text{ and }S)$
    - Common short-hand expression for the output wire $F$
        - $(X \wedge \neg S) \vee (Y \land S)$

> [!note]+
> - Multiplication symbols represent `AND`
> - Addition symbols represent `OR`

1. Show the truth table for the three input $X, Y, S$ and output $F$
2. Represent logical expression in gates
3. & Show how you’d build this circuit using IC chips
