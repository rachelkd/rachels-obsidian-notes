---
dg-publish: true
tags: ["#cs", "#lecture", "#math", "#note", university]
Course Code:
  - "[[CSC236]]"
Week: 1
Module:
  - "[[1 - Induction]]"
Date: 2024-09-09
Date created: Mon., Sep. 9, 2024, 1:14:14 pm
Date modified: Mon., Dec. 9, 2024, 4:32:42 am
---

- What is the **Principle of Simple Induction**?
    - $$P(0) \land \bigg(\forall n, P(n) \implies P(n+1)\bigg) \implies \forall n, P(n)$$

#### Syntactic Details

- What is $P$?
    - $P$ is a predicate
    - $P : \mathbb{N} \rightarrow \{ T, F \}$
- Default domain $\mathbb{N}$
    - → “$\forall n$” = $\forall n \in \mathbb{N}$
- Variable names ==can be reused when no overlap== in scope
- What do we *prove*?
    - $P(0) \; \land \; \bigg( \forall n, P(n) \implies P(n + 1) \bigg)$
- What do we *conclude*?
    - $\forall n, P(n)$

# Example: What Amounts of Money $n$ that Can Be Made up Using only 3 Cent and 7 Cent Coins?

![|center|600](https://i.imgur.com/rCRtinT.png)

### Intuition

- From 0 cents - 11 cents:
    - case varies
- From 12 cents onwards:
    - always yes

![|center|500](https://i.imgur.com/KOtz0PE.png)

> [!info] Every time you have the urge to write “$…$”, that is **induction**.

# Formalizing Our Example

> [!question]- What is our **predicate**?
> For each $n \in \mathbb{N}$, let $P(n)$ be:
> $$\exists t, s \in \mathbb{N}, n = 3t + 7s$$
>
> > [!warning]+ You cannot quantify $n$ after “let $P(n)$”!
> > - i.e., $\forall n \in \mathbb{N}, \exists t,s \in \mathbb{N} …$ is *incorrect*!

---

![[Principle of Simple Induction.pdf]]
