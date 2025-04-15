---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 21
Module:
  - "[[7 - Surface Integrals and The Divergence Theorem]]"
Lecture:
  - "58"
  - "59"
  - "60"
Chapter: "21.1"
Slides/Notes: 
Date: 2025-03-10
Date created: Wed., Apr. 9, 2025, 2:09:38 pm
Date modified: Thu., Apr. 10, 2025, 4:14:03 am
---

# Flux Integrals Over Parameterized Surfaces

> [!goal]+ Compute the flux of a smooth vector field $\vec{F}$ through a smooth oriented surface, $S$, parameterized by $\vec{r} = \vec{r}(s, t)$ for $(s, t)$ in some region $R$ of the parameter space

Consider a parameter rectangle on the surface $S$ corresponding to a rectangular region with sides $\Delta s$ and $\Delta t$ in the parameter space.

![](https://i.imgur.com/kkzODot.png)

*Figure 21.21: Parameter rectangle on the surface $S$ corresponding to a small rectangular region in the parameter space, $R$*

If $\Delta s, \Delta t$ are small, the area vector, $\Delta \vec{A}$, of the patch is approximately the area vector of the parallelogram defined by the vectors
$$
\vec{r}(s + \Delta s, t) - \vec{r}(s, t) \approx \frac{ \partial \vec{r} }{ \partial s } \Delta s, \quad \text{and} \quad \vec{r}(s, t + \Delta t) - \vec{r}(s, t) \approx \frac{ \partial \vec{r} }{ \partial t } \Delta t
$$
Thus:
$$
\Delta \vec{A} \approx \frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t } \Delta s \, \Delta t
$$
- % Assume that the vector $\frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t }$ is never zero and points in the direction of the unit normal orientation vector $\vec{n}$
- If vector $\frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t }$ points in the opposite direction to $\vec{n}$:
    - Reverse the order of the cross product

Replacing $\Delta \vec{A}, \Delta s, \Delta t$ by $d\vec{A}, ds, dt$, we write:
$$
d\vec{A} = \left( \frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t }  \right) \, ds \, dt
$$

> [!thm]+ Flux of a vector field through a parameterized surface
> The flux of a smooth vector field $\vec{F}$ through a smooth oriented surface $S$ parameterized by $\vec{r} = \vec{r}(s, t)$, where $(s, t)$ varies in a parameter region $R$, is given by:
> $$\int_{S} \vec{F} \cdot d\vec{A} = \int_{R} \vec{F}\left( \vec{r}(s, t) \right) \cdot \left( \frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t }  \right) \, ds \, dt$$
> We choose the parameterization so that $\frac{ \partial \vec{r} }{ \partial s }\times \frac{ \partial \vec{r} }{ \partial t }$ is never zero and points in the direction of $\vec{n}$ everywhere.

## Area of a Parameterized Surface

The area $\Delta A$ of a small parameter rectangle is the magnitude of its area vector $\Delta \vec{A}$.
Therefore:
$$
\text{Area of }S = \sum\Delta A = \sum \left \| \Delta \vec{A} \right \| \approx \sum \left \| \frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t } \right \| \, \Delta s\,\Delta t
$$
Taking the limit as the area of the parameter rectangles tends to zero:

> [!thm]+ Area of a parameterized surface
> The area of a surface $S$ which is parameterized by $\vec{r} = \vec{r}(s, t)$, where $(s, t)$ varies in a parameter region $R$, is given by:
> $$\int_{S} dA = \int_{R} \left \| \frac{ \partial \vec{r} }{ \partial s } \times \frac{ \partial \vec{r} }{ \partial t } \right \| \, ds \, dt$$
