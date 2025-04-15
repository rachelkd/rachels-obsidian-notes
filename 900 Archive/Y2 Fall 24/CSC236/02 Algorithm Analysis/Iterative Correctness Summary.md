---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC236]]"
Week: 8
Module:
  - "[[2 - Algorithm Analysis]]"
Lecture:
  - "15"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/lGJ2KrXoWpIXVM2wVn0jvD
Date: 2024-11-04
Date created: Thu., Nov. 7, 2024, 12:21:30 pm
Date modified: Mon., Dec. 9, 2024, 3:39:39 pm
---

- See [Week 7 prep/review](https://share.goodnotes.com/s/lGJ2KrXoWpIXVM2wVn0jvD)
- Overall structure:
    - **Goal**: Prove for each input, $\text{Pre} \implies \text{Termination} \wedge \text{Post}$
    - **Given**: Pre, Post, algorithm — including loop condition $C$
    - **1. Partial Correctness**:
        - $\text{Pre} \wedge \text{Termination} \implies \text{Post}$

        - > [!warning] “Partial correctness” is not used with a consistent meaning each term

        - Define Invariant $I$
        - Prove Invariant: $\text{Pre} \implies \left[ I_{0} \wedge \forall k, (I_{k} \wedge C_{k} \implies I_{k + 1}) \right]$
            - “Pick a $k$ where the invariant is true at the end of iteration $k$ and the loop condition uses $k$ (start of iteration $k + 1$); prove that $I_{k + 1}$ is true”
        - Prove Post:
            - $\text{Pre} \implies [I \wedge \neg C \implies \text{Post}]$
    - **2. Termination**:
        - Define Variant $V$
        - Prove $\forall k, V_{k} \in \mathbb{N}$
        - Prove $\forall k, V_{k + 1} < V_{k}$
            - In context: $\text{Pre} \implies [I_{0} \implies V_{0} \in \mathbb{N} \wedge \forall k, I_{k} \wedge C_{k} \implies V_{k + 1} \in \mathbb{N} \wedge V_{k + 1} < V_{k}]$
            - Can also assume $I_{k + 1}$ where we assume $I_{k}$ since we proved that $I_{k + 1}$ is true when $I_{k}$ is true
