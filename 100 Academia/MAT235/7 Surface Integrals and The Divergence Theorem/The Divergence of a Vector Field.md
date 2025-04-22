---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 22
Module:
  - "[[7 - Surface Integrals and The Divergence Theorem]]"
Lecture:
  - "61"
  - "62"
  - "63"
Chapter: "19.3"
Slides/Notes: 
Date: 2025-03-17
Date created: Sun., Apr. 20, 2025, 5:31:42 pm
Date modified: Sun., Apr. 20, 2025, 6:22:19 pm
---

# The Divergence of a Vector Field

![](https://i.imgur.com/BmOKUd0.png)

*Figure 19.29: Vector field showing a **source***

![](https://i.imgur.com/1RGCMLj.png)

*Figure 19.30: Vector field showing a **sink***

> [!goal]+ Use the flux out of a closed surface surrounding a point to measure the *outflow per unit volume* there, also called the **divergence**, or **flux density**

## Definition of Divergence

- To measure outflow per unit volume of a vector field at a point:
    - Calculate flux out of a small sphere centred at the point
    - Divide by the volume enclosed by the sphere
    - Take the limit of this flux-to-volume ratio as the sphere contracts around the point

> [!def]+ Geometric definition of divergence
> The **divergence**, or **flux density** of a smooth vector field $\vec{F}$, written ==$\text{div} \vec{F}$==, is a scalar-valued function defined by
> $$\text{div} \vec{F}(x, y, z) = \lim_{ \text{Volume} \to 0 } \frac{\left( \int_{S} \vec{F} \cdot d\vec{A} \right)}{\text{Volume of } S} $$
> Here, $S$ is a sphere centred at $(x, y, z)$, oriented outward, that contracts down to $(x, y, z)$ in the limit.
> The limit can be computed using other shapes as well.

In Cartesian coordinates, the divergence can also be calculated using the following formula:

> [!def]+ Cartesian coordinate definition of divergence
> If $\vec{F} = \langle F_{1}, F_{2}, F_{3} \rangle$, then
> $$\text{div} \vec{F} = \frac{ \partial F_{1} }{ \partial x }  + \frac{ \partial F_{2} }{ \partial y } + \frac{ \partial F_{3} }{ \partial z } $$

> [!tip]+ The dot product formula gives an easy way to remember the Cartesian coordinate definition and suggests another common notation for $\text{div} \vec{F}$, namely $\nabla \cdot \vec{F}$.
> Using $\nabla = \frac{ \partial }{ \partial x } \vec{i} + \frac{ \partial }{ \partial y } \vec{j} + \frac{ \partial }{ \partial z } \vec{k}$, we can write:
> $$\text{div } \vec{F} = \nabla \cdot \vec{F} = \left\langle  \frac{ \partial }{ \partial x } , \frac{ \partial }{ \partial y } , \frac{ \partial  }{ \partial z }   \right\rangle \cdot \langle F_{1}, F_{2}, F_{3} \rangle = \frac{ \partial F_{1} }{ \partial x }  + \frac{ \partial F_{2} }{ \partial y } + \frac{ \partial F_{3} }{ \partial z }$$

A vector field $\vec{F}$ is said to be **divergence-free** if $\text{div }\vec{F} = 0$ everywhere that $\vec{F}$ is defined.
