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
Chapter: "19.4"
Slides/Notes: 
Date: 2025-03-17
Date created: Sun., Apr. 20, 2025, 6:22:19 pm
Date modified: Sun., Apr. 20, 2025, 6:26:45 pm
---

# The Divergence Theorem

- The **boundary** of a solid region is the skin between the interior of the region and the space around it

> [!def]+ The Divergence Theorem
> If $W$ is a solid region with boundary $S$ given the outward orientation, and if $\vec{F}$ is a smooth vector field on a solid region containing $W$ and $S$, then
> $$\int_{S} \vec{F} \cdot d\vec{A} = \int_{W} \text{div }\vec{F} \, dV$$
> where $S$ is the given outward orientation.

> [!thm]+ Divergence-free
> If $\vec{F}$ is a **divergence-free** vector field, and if $S$ is the boundary of solid region $W$, then by the Divergence Theorem,
> $$\int_{S} \vec{F} \cdot d\vec{A} = \int_{W} \text{div }\vec{F}\, dV = 0$$
