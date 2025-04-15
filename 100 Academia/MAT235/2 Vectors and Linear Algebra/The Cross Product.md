---
dg-publish: true
tags: ["lecture", "note", math, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[2 - Vectors and Linear Algebra]]"
Week: 5
Chapter: 13.4
Date: 2024-09-30
aliases: []
Date created: Mon., Sep. 30, 2024, 11:15:42 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Area of a Parallelogram

![](https://i.imgur.com/oFAWQlx.png)

$$\text{Area of parallelogram} = \text{Base} \cdot \text{Height} = ||\vec{v}|| ||\vec{w}|| \sin \theta$$

- Computing given $\vec{v} = v_{1} \vec{i} + v_{2} \vec{j} + v_{3} \vec{k}$, $\vec{w} = w_{1} \vec{i} + w_{2} \vec{j} + w_{3} \vec{k}$:
    - $\vec{v}$, $\vec{w}$ on $xy$-plane $\implies v_{3} = w_{3} = 0$
    - $\implies \text{Area of parallelogram} = |v_{1}w_{2} - v_{2}w_{1}|$

# Definition of the Cross Product

> [!def]+ Definition of the cross product $\vec{v} \times \vec{w}$
>
> - $\vec{v} \times \vec{w}$ is a vector perpendicular to both $\vec{v}$ and $\vec{w}$
> - **Geometric**:
>     - If $\vec{v}$ and $\vec{w}$ are not parallel, then
>     - $\vec{v} \times \vec{w} = \bigg( \text{Area of parallelogram with edges } \vec{v} \text{ and } \vec{w} \bigg) \vec{n} = (||\vec{n}|| ||\vec{w}|| \sin \theta) \vec{n}$
>     - where $0 \leq \theta \leq \pi$
> - **Algebraic**:
>     - $$\vec{v} \times \vec{w} = (v_{2}w_{3} - v_{3}w_{2}) \vec{i} + (v_{3}w_{1} - v_{1}w_{3}) \vec{j} + (v_{1}w_{2} - v_{2}w_{1}) \vec{k}$$
>     - where $\vec{v} = v_{1} \vec{j} + v_{2} \vec{j} + v_{3} \vec{k}$ and $\vec{w} = w_{1} \vec{i} + w_{2} \vec{j} + w_{3} \vec{k}$
>     - $$\vec{v} \times \vec{w} = \begin{pmatrix} \hat{i} & \hat{j} & \hat{k} \\ v_1 & v_2 & v_3 \\ w_1 & w_2 & w_3 \end{pmatrix} = \left( v_2 w_3 - v_3 w_2 \right) \hat{i} + \left( v_3 w_1 - v_1 w_3 \right) \hat{j} + \left( v_1 w_2 - v_2 w_1 \right) \hat{k}$$
