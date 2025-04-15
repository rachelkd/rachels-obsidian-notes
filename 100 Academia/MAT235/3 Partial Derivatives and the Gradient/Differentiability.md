---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 10
Lecture:
  - "28"
Chapter: "14.8"
Slides/Notes: 
Date: 2024-11-14
aliases: []
Date created: Fri., Nov. 29, 2024, 10:06:41 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Differentiability

## Differentiability for Functions of Two Variables

- We define differentiability at a point in terms of the *error* and the distance from the point
- If the point is $(a, b)$ and a nearby point is $(a + h, b + h)$:
    - Distance between them is $\sqrt{ h^{2} + k^{2} }$

![|500](Differentiability-008.jpg)

> [!def]+ Differentiable at a point
> A function $f(x, y)$ is **differentiable at the point $(a, b)$** if there is a linear function $L(x, y) = f(a, b) + m(x-a) + n(y-b)$ such that if the *error* $E(x, y)$ is defined by
> $$
> f(x,y) = L(x, y) + E(x, y)
> $$
> and if $h = x-a, k = y-b$, then the *relative error* $\frac{E(a + h, b + k)}{\sqrt{ h^{2} + k^{2} }}$ satisfies
> $$
> \lim_{ {h \to 0, k \to 0} } \frac{E(a + h, b + k)}{\sqrt{ \sqrt{ h^{2} + k^{2} } }} = 0
> $$

## How Do We Know If a Function is Differentiable?

> [!thm]+ Theorem. Continuity of Partial Derivatives Implies Differentiability
> If the partial derivatives, $f_{x}, f_{y}$ of a function $f$ exist and are *continuous* on a small disk centered at the point $(a, b)$, then ==$f$ is *differentiable* at $(a, b)$==

- **Differentiability** is not the same as having partial derivatives
    - Function is differentiable at a point $\implies$ Both partial derivatives exist at that point
        - Converse is not true
    - Having both partial derivatives at a point does not guarantee that a function is differentiable there

> [!thm]+ Differentiability implies continuity
> If a function is *differentiable* at a point, it is ==also *continuous*== at that point

- A function $f(x, y)$ with ==both partial derivatives== at a point ==can fail to be continuous== at that point
