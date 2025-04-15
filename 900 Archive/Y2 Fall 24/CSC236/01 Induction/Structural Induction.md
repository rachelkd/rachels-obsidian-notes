---
dg-publish: true
tags: ["#lecture", "#note", cs, university]
Course Code:
  - "[[CSC236]]"
Week: 4
Module:
  - "[[1 - Induction]]"
Date: 2024-09-23
Lecture:
  - "6"
  - "7"
  - "8"
  - "9"
Slides/Notes: https://share.goodnotes.com/s/uzOgm9t7LeXVGDVqKTjsXy
Date created: Tue., Sep. 24, 2024, 5:04:40 pm
Date modified: Mon., Dec. 9, 2024, 3:51:38 am
---

- So far, induction has always been over $\mathbb{N}$
    - Other sets? $\mathbb{Z}$, trees, strings?

> [!important] There is a way to set up an induction structure for any set, as long as you can give a correct recursive definition of that set.

# Ex. 1. WTP $\forall$ even $n \in \mathbb{N}, P(n)$

1. Prove: $\forall n, (\exists k, n = 2k) \implies P(n)$
2. Prove: $\forall n, P(2n)$
3. Prove: $P(0) \wedge [\forall n, P(n) \implies P(n + 2)]$

## Justification for 3

Adjust the structure of induction:

- Define set $\mathbb{E}$ for all even natural numbers in a *self-contained*, *recursive* way,
    - i.e., not using any other sets or referring to anything outside of the definition
    1. ==$0 \in \mathbb{E}$==
    2. ==$\forall n \in \mathbb{E}, n + 2 \in \mathbb{E}$==
        - Then, $\mathbb{E} = \{ 0, 2, \pi, \pi + 2, 6, \pi + 4, 8, \cdots \}$ satisfies conditions 1, 2.
            - ⇒ infinitely many sets that satisfy these two conditions
            - Need to add a third condition!
    3. ==$\mathbb{E}$ contains nothing else==
        - or, “$\mathbb{E}$ is the smallest set such that …”
        - “Don’t include anything else, like $\pi$”

- **Intuition:** Prove $P(0) \wedge \forall n, P(n) \implies P(n + 2)$
    - $P(0)$ matches 1. of $\mathbb{E}$ definition
    - Inductive step matches 2. of $\mathbb{E}$ definition

> [!important]+ Structure of the proof (structural induction) matches the structure of your definition
>
> - Allowed to conclude: $\forall n \in S, P(n)$

![](https://i.imgur.com/Uqddi6b.png)

- Take the Simple Ind. structure → Structural Induction
    - Simple Induction depends on the structure of the natural numbers itself

> [!info] You can understand any form of structural induction as long as you understand the recursive structure of the set you are working with.

# Ex. A Set of “expressions”

Define the set of “expressions” $E$ as the *smallest set* that satisfies:

1. `'1', '2', '3'` $\in E$
    - Strings
    - Note: Would often see $[1,2,3] \in E$
        - Not clear that elements are strings
        - Would have to say outside the definition that $E$ is a set of strings
2. $\forall e_{1}, e_{2} \in E,$
    1. $e_{1} \oplus e_{2} \in E$, and
    2. $e_{1} \otimes e_{2} \in E$
3. $E$ contains nothing else (already covered by “smallest set” above)

$$E = \{ 1, 2, 3, 1 \oplus 2, \bbox[pink]{3 \otimes 3}, \cdots \}$$

> [!question]- Do we rule out $3 \oplus 3$ because $e_{1} \neq e_{2}$?
>
> - No
> - Allowed because $e_{1,}e_{2}$ are *dummy variables*; not restricted

## As Code

```python
# Post: returns a random elem. of E *
#                                   * with probability 1
def rand_E():
    if randrange(3) == 0:
        return ['1','2','3'][randrange(3)]
    else:
        e1 = rand_E()
        e2 = rand_E()
        return e1 + ['⨁', '⊗'][randrange(2)] + e2
```

## Define, for Each $e \in E$

- $\text{num}(e) = \text{number of 1's, 2's, and 3's in } e$
    - e.g., $\text{num}(1 \oplus 2 \otimes 2) = 3$
- $\text{op}(e) = \text{number of } \oplus \text{'s and } \otimes \text{'s in } e$
    - e.g., $\text{op}(1 \oplus 2 \otimes 3) = 2$
- $\text{num}(u \cdot v) = \text{num}(u) + \text{num}(v)$
    - i.e., $u \cdot v$ is string concatenation

<!-- break -->
- Notes:
    - Definitions rely on our understanding of strings → ==*not* self-contained!==
    - Advantage: From our understanding, we can say a lot of things about how these functions behave

> [!info]+ Conjecture.
> $$\forall e \in E, \texttt{num(e)} = 1 + \texttt{op(e)}$$

## Proof by Structural Induction on $E$

- Notes:
    - Look at the definition of $E$
    - ⇒ We get base cases
    - Assume $e_{1}, e_{2}$ satisfy this property. Does it still satisfy for the “operations”?

**Base case.**
$$\texttt{num(} \underbrace{1}_{2, 3} \texttt{)} = 1 = 1 + 0 = 1 + \texttt{op(} \underbrace{1}_{2,3} \texttt{)}$$
**Inductive step.**
Let $e_{1}, e_{2} \in E$.
Assume \[IH]:

- $\texttt{num(} e_{1} \texttt{)} = 1 + \texttt{op(} e_{1} \texttt{)}$, and
- $\texttt{num(} e_{2} \texttt{)} = 1 + \texttt{op(} e_{2} \texttt{)}$

Then,
$$\begin{align*}
num(e_{1} \oplus e_{2}) &= num(e_{1}) + num(\oplus) + num(e_{2}) \\
&= \bigg(1 + op(e_{1}) \bigg) + 0 + \bigg(1 + op(e_{2}) \bigg)\\
&= 1 + \bigg(op(e_{1}) + op(\oplus) + op(e_{2}) \bigg) \\
&= 1 + op(e_{1} \oplus e_{2})
\end{align*}$$

---
# A Self-Contained Definition of $E$

Recall:
- Elements of $E$ → treated as *strings*
- Defined for *all* strings $e$ (not just $e \in E$)
    - $\text{num}(e) =$ # of digits (1, 2, or 3) in $e$
    - $\text{op}(e) =$ # operators ($\oplus$ or $\otimes$) in $e$
    - & $\text{val}(e) =$ numerical values of $e$, where $\oplus$ means $+$, $\otimes$ means $\times$

## Problem with Our Previous Definition

> [!question] $\text{val}(1 \oplus 2 \otimes 3) = \text{?}$

- We have two options:
    1. $(1 + 2) \times 3 = 9$
    2. $1 + (2 \times 3) = 7$
    - Which one is correct? → Need some kind of convention
- ? How is $1 \oplus 2 \otimes 3$ generated?
    - Case A:
        - $1 \in E$
        - $2 \otimes 3 \in E$
        - $1 \oplus (2 \otimes 3)$
    - Case B:
        - $1 \oplus 2 \in E$
        - $3 \in E$
        - $(1 \oplus 2) \otimes 3$
    - If we can get info on how it is generated, we can determine which option to use

## New Self-Contained Definition

> [!obs]+ Expressions are *not* strings; they are trees.
> ![|center|400](https://i.imgur.com/wmomZCD.png)

$\implies$ We can give a **self-contained recursive definition** for $\text{num}(e), \text{op}(e), \text{val}(e)$ for each $e \in E$

- $\text{num}(e)$ is defined as:
    - $\text{num}(1) = \text{num}(2) = \text{num}(3) = 1$ (Defined for base elements)
    - $\forall e_{1}, e_{2} \in E,$
        - $\text{num}(e_{1} \oplus e_{2}) = \text{num}(e_{1}) + \text{num}(e_{2})$
        - $\text{num}(e_{1} \otimes e_{2}) = \text{num}(e_{1}) + \text{num}(e_{2})$
- $\text{op}(e)$ is defined as:
    - $\text{op}(1) = \text{op}(2) = \text{op}(3) = 0$
    - $\text{op}(e_{1} \oplus e_{2}) = 1 + \text{op}(e_{1}) + \text{op}(e_{2})$
    - $\text{op}(e_{1} \otimes e_{2}) = 1 + \text{op}(e_{1}) + \text{op}(e_{2})$
- $\text{val}(e)$ is defined as:
    - $\text{val}(1) = 1, \text{val}(2) = 2, \text{val}(3) = 3$
    - $\text{val}(e_{1} \oplus e_{2}) = \text{val}(e_{1}) + \text{val}(e_{2})$
    - $\text{val}(e_{1} \otimes e_{2}) = \text{val}(e_{1}) \times \text{val}(e_{2})$

> [!obs]+ There is a distinction between the recursive structure of an element vs. its “final” value
> - i.e., The “final” value (e.g., $1 \oplus 2 \otimes 3$) has two different recursive structures that give this final value

# Binary Trees
(Specifically tree “structures”)

## Definition of Binary Tree

- $T$ is the smallest set such that:
    - Empty tree $\cdot \in T$
        - Textually: $()$
    - $\forall t_{1}, t_{2} \in T,$ ![|50](https://i.imgur.com/WRPXnU5.png) $\in T$
        - Textually: $(t_{1}, t_{2})$
- Some elements:
    - ![|300](https://i.imgur.com/iloamsU.png)

<!-- break -->
- ? Where are the values?
    - There are no values
    - Solely just structures in definition
- ? Why include empty trees?
    - If we didn’t $\implies$ Need 4 different constructions of trees
        1. Root without children is a tree
        2. Root with only left child is a tree
        3. Root with only right child is a tree
        4. Root with two children is a tree
    - & Simplifies the recursion rule
    - Exercise: Rewrite the definition of binary tree for only non-empty rules (need multiple recursion rules)

## Definitions of $\text{size}(t), \text{height}(t)$

- $\text{size}(t)$ : total # of “nodes”
    - $\text{size}(\cdot) = 0$
    - $\forall t_{1}, t_{2} \in T, \text{size}($![|50](https://i.imgur.com/WRPXnU5.png)$) = 1 + \text{size}(t_{1}) + \text{size}(t_{2})$
- $\text{height}(t)$ : # of “nodes” from root to the farthest root
    - $\text{height}(\cdot) = 0$
    - $\forall t_{1}, t_{2} \in T, \text{height}($![|50](https://i.imgur.com/WRPXnU5.png)$) = 1 + \text{max}\bigg(\text{height}(t_{1}), \text{height}(t_{2}) \bigg)$

<!-- break -->
- Note: We never defined what a node is.

## An Attempt of a Proof by Structural Induction

**Claim.** $\forall t \in T, \text{size}(t) \leq 2^{\text{height}(t)}$
(WTP)

**Proof (by structural induction).**

<u>Base Case.</u>
$\text{size}(\cdot) = 0 \leq 1 = 2^{\text{height}(\cdot)}$

<u>Inductive Step.</u>
Let $t_{1}, t_{2} \in T$.
Assume \[IH]:
- $\text{size}(t_{1}) \leq 2^{\text{height}(t_{1})}$
- $\text{size}(t_{2}) \leq 2^{\text{height}(t_{2})}$

Then,

$\text{size}($![|50](https://i.imgur.com/WRPXnU5.png)$\begin{align}) = 1 + \text{size}(t_{1}) + \text{size}(t_{2}) &&&& \text{(by definition of size)}\end{align}$
$$\begin{align*}
&\leq 1 + 2^{\text{height}(t_{1})} + 2^{\text{height}(t_{2})} && \text{(by IH)} \\
&\leq 1 + 2^{\text{max}\Big(\text{height}(t_{1}), \text{height}(t_{2})\Big)} \cdot 2 && (\because 2^{h_{1}} + 2^{h_{2}} \leq 2^{\text{max}(h_{1}, h_{2})} + 2^{\text{max}(h_{1}, h_{2})} = 2 \cdot 2^{\text{max}\Big(h(t_{1}), h(t_{2})\Big)}) \\
&= \bbox[pink]{1} + 2^{\text{max}(\text{height}(t_{1}), \text{height}(t_{2}))+1}
\end{align*}$$
- ! Problem: “1 +” is not what we want.

> [!question]+ What can we do?
> - Make adjustments?
> - Plug in terms, find “better”/stronger/more precise inequality
> - $\implies$ **Strengthening the IH**
>     - i.e., Changing the whole *WTP*

- We should try to prove something *harder*
    - i.e., Prove a better equality; One that is closer to what is actually true between $\text{size}(t)$ and $\text{height}(t)$
- If inequality is stronger,
    - [c] Have more to conclude at the end of induction step
    - [p] Get more out of your induction hypothesis
    - Often, these things balance out!

## A New Claim and Proof by Structural Induction

**Claim.** $\text{size}(t) \leq 2^{\text{height}(t)} - 1$

$\bigg(\equiv \text{size}(t) < 2^{\text{height}(t)}\bigg)$ since size and height are integers

**Proof by structural induction.**

$\text{size}($![|50](https://i.imgur.com/WRPXnU5.png)$\begin{align}) = 1 + \text{size}(t_{1}) + \text{size}(t_{2}) &&&& \text{(by definition of size)}\end{align}$
$$\begin{align*}
&\leq 1 + 2^{\text{height}(t_{1})} \bbox[DarkSeaGreen]{-1} + 2^{\text{height}(t_{2})} \bbox[DarkSeaGreen]{-1} && \text{(by IH)} \\
&\leq 1 + \bigg( 2^{\text{max}\Big(\text{height}(t_{1}), \text{height}(t_{2})\Big)} \bbox[DarkSeaGreen]{-1} \bigg) \cdot 2 \\
&= \cancel{1} + 2^{\text{max}(\text{height}(t_{1}), \text{height}(t_{2}))+1} \bbox[DarkSeaGreen]{-1} \cancel{\bbox[DarkSeaGreen]{-1}} \\
&= 2^{\text{max}(\text{height}(t_{1}), \text{height}(t_{2}))+1} - 1
\end{align*}$$
