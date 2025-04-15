---
dg-publish: false
tags: ["#lecture", "#note", lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 2
Module:
  - 1 - First Principles of Probability
Date: 2024-09-10
Chapter: 1.4
Slides/Notes: []
Date created: Wed., Sep. 11, 2024, 9:32:14 pm
Date modified: Wed., Nov. 27, 2024, 4:27:17 am
---

# Probability Functions

Assume for the next several chapters that the sample space is *discrete*.

- **Discrete**:
    - Sample space is either ==finite== or ==countably infinite==
- **Countably infinite**:
    - A set is countably infinite if the elements of the set can be arranged as a sequence
        - e.g., natural numbers 1, 2, 3, …
    - All countably infinite sets can be put in one-to-one correspondence with the natural numbers:
        - Sample space is *countably infinite* $\iff$ can be written as $\Omega = \{\omega_{1}, \omega_{2}, …\}$
- **Finite**
    - Sample space is finite $\iff$ can be written as $\Omega = \{\omega_{1}, \omega_{2}, \omega_{3}, …, w_{k}\}$
- Examples of an infinite set that is not countably infinite i.e., *uncountable*:
    - Set of all reals
    - An interval of reals, e.g., $(0, 1)$

> [!def]- Probability function
> - Assigns numbers between 0 and 1 to events according to three defining properties:
>
> > [!note]+ Given a random experiment with discrete sample space $\Omega$, a **probability function** $P$ is a function on $\Omega$ with the following properties:
> > 1. $$P(\omega) \geq 0, \forall \omega \in \Omega$$
> > 2. $$\sum\limits_{\omega \in \Omega} P(\omega) = 1 \tag{1.1}$$
> > 3. For all events $A\subseteq \Omega$, $$P(A) = \sum\limits_{\omega\in A} P(\omega) \tag{1.2}$$

- Equations 1.1, 1.2 use a generalized $\sum\limits$ notation
- $\sum_{\omega\in \Omega}$ means:
    - Sum is over all $\omega$ that are elements of the sample space, $\Omega$
    - i.e., sum of all outcomes in the sample space
- Finite sample space $\Omega = \{\omega_{1}, …, \omega_{k}\}$ case:
    - (1.1) becomes $\sum\limits_{\omega \in \Omega} P(\omega) = P(\omega_{1)}+ … + P(\omega_{k}) = 1$
- Countably infinite sample space $\Omega = \{\omega_{1}, \omega_{2}, …\}$ case:
    - (1.1) becomes $\sum\limits_{\omega \in \Omega} P(\omega) = P(\omega_{1}) + P(\omega_{2})+…=\sum\limits_{i = 1}^{\infty} P(\omega_{i}) = 1$

# Set Notation

Textbook Chapter 1.4

> [!note]- Events can be combined together to create new events using connectives
> - “Or”, “and”, and “not”

For sets $A, B \subseteq \Omega$,

- **Union** $A \cup B$
    - Set of all elements of $\Omega$ that are in either $A$, $B$, or both
    - “Or”
- **Intersection** $A \cap B$
    - Set of all elements of $\Omega$ that are in *both* $A, b$
    - “And”
- **Complement** $A^{C}$
    - Set of all elements of $\Omega$ that are *not* in $A$
    - “Not”
- **Mutually exclusive** (**disjoint**) $A \cap B = \emptyset$
    - Two events are mutually exclusive, or disjoint, if they have no outcomes in common


![|center|500](https://i.imgur.com/MSFJCcl.png)

![|center|500](https://i.imgur.com/hAE2tH9.png)

# Properties of Probabilities

> [!thm]+ Addition Rule for mutually exclusive events
> If $A$ and $B$ are mutually exclusive events, then
> $$P(A \cup B) = P(A) + P(B)$$
>
> <u>Extension.</u>
> Suppose $A_{1}, A_{2}, \dots$ is a sequence of pairwise mutually exclusive events (i.e., $A_{i}, A_{j}$ are mutually exclusive for all $i \neq j$).
> $$P \left(\bigcup_{i = 1}^{\infty} A_{i} \right) = \sum_{i = 1}^{\infty} P(A_{i})$$

- Addition Rule is a consequence of third defining property of a *probability function*
    - $$\begin{align*} P(A \cup B) &= \sum_{w \in A \cup B} P(\omega)  \\ &= \sum_{w \in A} P(\omega) + \sum_{w \in B} P(\omega) && \text{(because the events are disjoint)} \\ &= P(A) + P(B) \end{align*}$$
    - Disjoint → No outcome $\omega$ will be counted twice

> [!thm]+ Properties of Probabilities
> 1. If $A$ implies $B$ i.e., $A \subseteq B$, then $P(A) \leq P(B)$
> 2. $P(A^{C}) = 1 - P(A)$
> 3. For all events $A$ and $B$, $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
