---
dg-publish: true
tags: ["#lecture", "#note", cs, university]
Course Code:
  - "[[CSC236]]"
Week: 5
Module:
  - "[[2 - Algorithm Analysis]]"
Chapter:
Slides/Notes:
  - https://share.goodnotes.com/s/gFeR6vOiwVvm1LID1CTXhF
Date: 2024-10-04
Date created: Mon., Oct. 7, 2024, 8:00:56 pm
Date modified: Tue., Nov. 5, 2024, 7:58:59 pm
---

# Example. Quotient Remainder Theorem

> [!def]+ Quotient Remainder Theorem
>
> - Divide 236 by 25:
> $$236 = \underbrace{\underline{\textcolor{blue}{0}}}_{\text{quotient}} \times 25 + \underbrace{\underline{\textcolor{blue}{236}}}_{\text{remainder}}$$

→ Remainder too big? → Subtract 25 from remainder: $1 \cdot 25 + 211$

> [!thm]+ Insight.
> There is always $q, r \in \mathbb{N}$ such that $236 = q \cdot 25 + r$.

> [!thm]+ Insight.
> This eventually stops.

> [!thm]+ Lemma.
> If $236 = q \cdot 25 + r$ and $r \geq 25$, then there is a smaller remainder.
> **Proof.**
>
> $$\begin{align*}
> 236 &= q \cdot 25 + r \\
> &= q \cdot 25 + 25 + \overbrace{r - 25}^{\geq 0} \\
> &= (q + 1) \cdot 25 + (r - 25)
> \end{align*}$$
>
> $\therefore$ If we know that $r_{m}$ is the *minimum* possible remainder, then we can conclude $r_{m} < 25$.

# Well-Ordering Principle (WOP)

> [!def]- Well-Ordering Principle
> Each non-empty subset of $\mathbb{N}$ contains a minimum element
> $$\bigg[ \forall S \subseteq \mathbb{N}, S \neq \emptyset \implies \Big( \exists m \in S, \forall n \in S, n \geq m \Big) \bigg]$$
>
> - *Contrapositive*:
>     - Every subset of $\mathbb{N}$ that has no minimum is empty

**Notes.**

- Applies to infinite subsets of $\mathbb{N}$
- Unique to $\mathbb{N}$
    - Not for $\mathbb{Z}$: $\{ \dots, -3, -2, -1 \}$ for counterexample
    - Not for $\mathbb{Q}^{+}$: $\left\{  1, \frac{1}{2}, \frac{1}{4}, \frac{1}{8}, \dots  \right\}$

# WTP: Let’s Generalize

- Instead of starting with something and plugging in numbers, we started with numbers to see what was happening
- When we understand what is happening → *Generalize*
    - Reasoning did not depend on the numbers → Give it a variable

Let $n \in \mathbb{N}$.
Let $d \in \mathbb{N}^{+}$ (cannot divide by 0).

WTP: $$\exists q, r, r < d \wedge n = q \cdot d + r$$

- Want to prove with **well-ordering**
    - $\implies$ Need to create a set of natural numbers
    - Set is related to the thing you are trying to minimize
    - & Insight: ==Create a set of every possible remainder==

**(Direct) Proof.**

Consider the set $R = \{ r \in \mathbb{N} : \exists q \in \mathbb{N}, n = qd + r \}$.

- ($R \subseteq \mathbb{N}$ by definition)
- We can use *well-ordering* if the set is not empty
    - $R \neq \emptyset  \because n \in R$
    - 0 witnesses $q$, so $n = 0d + n$

<!-- break -->
- By WOP, $R$ contains a minimum element $r_{m}$.
    - i.e., $\forall r \in R, r \geq r_{m}$
- By our Lemma, $r_{m} < d$
    - ($d$ was 25 in our lemma)
- $r_{m} \in R$, so $\exists q, n= qd + r_{m}$
    - Let $q_{m}$ witness this: $n = q_{m} d + r_{m}$

**Proof of Lemma.**
Claim: $r_{m} < d$

<u>For a contradiction</u>, assume that $r_{m} \geq d$.
Then, $r_{m} - d \in \mathbb{N} \because r_{m}, d \in \mathbb{N}$ and $$n = q_{m}d + r_{m} = (q_{m} + 1)d + (r_{m} - d)$$
So, $r_{m} - d \in R$ (with witness $q_{m} + 1$).

We have a contradiction:

- $r_{m} - d < r_{m}$ because $d > 0$
- Contradicts $\forall r \in R, r \geq r_{m}$ (from WOP)

Thus, $r_{m} < d \wedge n = q_{m}d + r_{m}$.
So, $(q_{m}, r_{m})$ witness what we WTP.

<div class="right-align"> <span class="math display">\blacksquare</span> </div>

**Alternate proof that $r_{m} < d$.**
Prove: $$\forall r \in R, \underbrace{r \geq d \implies \underbrace{\exists r' \in R, r' < r}_{r \text{ is not the min.}}}_{\forall r' \in R, r' \geq r \implies r < d}$$

> [!info]- When we want to prove something with well-ordering, you need to think
>
> - How can I use non-empty subsets of natural numbers to help me with this proof?
> - Need to somehow bring in a non-empty subset of natural numbers into your proof

- WTP the existence of a pair of numbers, $q, r$, with certain properties
- Well-ordering principle does not tell us anything about *pairs of natural numbers*
    - Tells us subsets of natural numbers → a single natural number is the minimum
- Need to figure out which number you are going to pick
    - What set of natural numbers are you going to look at?
- We pick $r$ and not $q$ because it would be helpful to *minimize* $r$
    - Well-ordering principle tells us that there is going to be a smallest one of $r$
    - This fact is going to help us, because we are looking for something small (hinted at in $r < d$)

# Generalizations

> [!info]+
>
> - Already discussed that WOP is intrinsic to $\mathbb{N}$
>     - Not true for other sets of numbers (e.g., $\mathbb{Z}, \mathbb{Q}$, even if it is bounded)

1. & Every non-empty subset $S \subseteq \mathbb{Z}$ that is *bounded below* (i.e., $\exists B \in \mathbb{R}, \forall x \in S, x \geq B$) has a **minimum** element.
    - Cannot get smaller and smaller infinitely because they are integers!
    - $0$ is one bound that happens to hold for $\mathbb{N}$, but this also works for integers bounded below
    - e.g., $S = \{ x \in \mathbb{Z} : x \geq \overbrace{-\pi}^{B} \} = \{ -3, -2, -1, \dots \}$
        - Formally, $T - \{ \underbrace{x + \lceil \pi \rceil}_{\geq 0} : x \in S \}$
        - Goal: $T \subseteq \mathbb{N}$
            - $\geq 0$ and $\in \mathbb{Z}$
            - So, $T \subseteq \mathbb{N}$
        - $\therefore T$ has a minimum: $x_{m}$
        - $\therefore x_{m} - \lceil \pi \rceil$ is the minimum in $S$
2. & Every non-empty $S \subseteq \mathbb{Z}$ that is *bounded above* has a **maximum** element
    - Insight:
    - A set bounded by $B$: ![|200](https://i.imgur.com/FkhY3YJ.png)
    - Given set $S \subseteq \mathbb{Z}$, bounded by $B$, create $T = \{ \lceil B \rceil - x : x \in S \}$
    - $\implies T \subseteq \mathbb{N}$, and
    - $T \neq \emptyset \iff S \neq \emptyset$
    - Therefore, by WOP, $T$ has a minimum $x_{m}$
    - $\implies$ *Maximum* in $S = \lceil B \rceil - x_{m}$
        - Since $\lceil B \rceil - x_{m} = \lceil B \rceil - \underbrace{(\lceil B \rceil - x)}_{x_{m}}$
