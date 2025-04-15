---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 15
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "41"
  - "42"
Chapter: "21.2"
Slides/Notes:
Date: 2025-01-22
Date created: Wed., Jan. 29, 2025, 4:08:29 am
Date modified: Sun., Feb. 9, 2025, 5:30:07 pm
---

# Change of Coordinates in a Multiple Integral

## Polar Change of Coordinates Revisited

Consider integral $\int_{R} (x + y)\,dA$ where $R$ is the region in the first quadrant bounded by the circle $x^{2} + y^{2} = 16$ and the $x$ and $y$-axes.

- In Cartesian coordinates:

$$
\int_{R} (x + y)\,dA = \int_{0}^{4} \int_{0}^{\sqrt{ 16 - x^{2} }} (x + y) \, dy  \, dx
$$

- In polar coordinates:

$$
\int_{0}^{\pi/2} \int_{0}^{4} (r \cos \theta + r \sin \theta) r\, dr  \, d\theta
$$
![](https://i.imgur.com/l6oFXrt.png)

- Integral in polar is over the rectangle in the $r\theta$-plane given by:
    - $0 \leq r \leq 4$
    - $0 \leq \theta \leq \frac{\pi}{2}$
- Conversion from polar to Cartesian coordinates changes rectangle into a quarter-disk
- Figure 21.18:
    - Shows how a typical rectangle (shaded) in $r\theta$-plane with sides of length $\Delta r$ and $\Delta\theta$ corresponds to a curved rectangle in the $xy$-plane with sides of length $\Delta r$ and $r\Delta\theta$
- & Extra $r$ is needed because the correspondence between $r, \theta, x, y$ curves the lines $r = 1, 2, \dots$ into circles and ==stretches== those lines around larger and larger circles

## General Change of Coordinates

Consider a general change of coordinates, where $x, y$ coordinates are related to $s, t$ coordinates by the differentiable functions.

$$
\begin{align}
x = x(s, t) && y = y(s, t)
\end{align}
$$

- A rectangular region, $\mathcal{T}$, in the $st$-plane corresponds to a region $R$ in the $xy$-plane
- Assume that change of coordinates is one-to-one
    - i.e., Each point in $R$ corresponds to only one point in $\mathcal{T}$

![](https://i.imgur.com/HY5Dn45.png)

- Divide $\mathcal{T}$ into small rectangles $\mathcal{T}_{ij}$
    - Sides of length $\Delta s, \Delta t$
- Corresponding piece $R_{ij}$ of $xy$-plane is a quadrilateral with curved sides
- If we choose $\Delta s, \Delta t$ small:
    - By [[Local Linearity and the Differential|local linearity]] of $x(s, t)$ and $y(s, t)$, $R_{ij}$ is approximately a ==parallelogram==
- Area of parallelogram with sides $\vec{a}$ and $\vec{b}$ is $\left\| \vec{a} \times \vec{b} \right\|$
    - → Need to find sides of $R_{ij}$ as ==vectors==

<!-- break -->
- Bottom side of $\mathcal{T}_{ij}$ has endpoints:
    - $\big( x(s, t), y(s, t) \big)$
    - $\big(x(s + \Delta s, t), y(s + \Delta s, t)\big)$
- Side in vector form is given by:
    $$
    \vec{a} = \big(x(s + \Delta s, t) - x(s, t) \big) \vec{i} + \big(y(s + \Delta s, t) - y(s, t)\big) \vec{j} \approx \left( \frac{ \partial x }{ \partial s } \Delta s \right) \vec{i} + \left( \frac{ \partial y }{ \partial s } \Delta s \right) \vec{j}
    $$
- Side of $R_{ij}$ corresponding to the left edge of $\mathcal{T}_{ij}$ is given by
    $$
    \vec{b} \approx \left( \frac{ \partial x }{ \partial t } \Delta t \right) \vec{i} + \left( \frac{ \partial y }{ \partial t } \Delta t \right) \vec{j}
    $$

$$
\begin{align}
\text{Area } R_{ij}  & \approx \left\| \vec{a} \times \vec{b} \right\| \approx \left|\left( \frac{ \partial x }{ \partial s } \Delta s \right) \left( \frac{ \partial y }{ \partial t } \Delta t \right) - \left( \frac{ \partial x }{ \partial t } \Delta t \right) \left( \frac{ \partial y }{ \partial s } \Delta s \right) \right| \\
 & = \left| \frac{ \partial x }{ \partial s } \cdot \frac{ \partial y }{ \partial t } - \frac{ \partial x }{ \partial t } \cdot \frac{ \partial y }{ \partial s }  \right| \Delta s\Delta t
\end{align}
$$

> [!def]+ Jacobian $\frac{ \partial (x, y) }{ \partial (s, t) }$
>
> $$
> \frac{ \partial (x, y) }{ \partial (s, t) } = \frac{ \partial x }{ \partial s } \cdot \frac{ \partial y }{ \partial t } - \frac{ \partial x }{ \partial t } \frac{ \partial y }{ \partial s } = \begin{vmatrix}
> \frac{ \partial x }{ \partial s }  & \frac{ \partial x }{ \partial t }  \\
> \frac{ \partial y }{ \partial s }  & \frac{ \partial y }{ \partial t }
> \end{vmatrix}
> $$

Then,
$$
\text{Area } R_{ij} \approx \left| \frac{ \partial (x, y) }{ \partial (s, t) }  \right| \Delta s \Delta t
$$
To compute $\int_{R} f(x, y) \, dA$ where $f$ is a continuous function:

- Look at Riemann sum obtained by dividing region $R$ into small curved regions $R_{ij}$

$$
\int_{R} f(x, y) \, dA \approx \sum\limits_{i, j} f(u _{ij}, v_{ij}) \cdot \text{Area of } R_{ij} \approx \sum_{i,j} f(u_{ij}, v_{ij}) \left| \frac{ \partial (x, y) }{ \partial (s, t) }  \right|  \Delta s \Delta t
$$

- Each point $(u_{ij}, v_{ij})$ in $R_{ij}$ corresponds to a point $(s_{ij}, t_{ij})$ in $\mathcal{T}_{ij}$
    - → Sum can be written in terms of $s, t$

$$
\sum_{i,j} f\big(x(s_{ij}, t_{ij}), y(s_{ij}, t_{ij})\big) \left| \frac{ \partial (x, y) }{ \partial (s, t) }  \right| \Delta s \Delta t
$$

### Change of Coordinates

> [!summary]+ Convert an integral from $x, y$ to $s, t$
>
> 1. Substitute for $x$ and $y$ in the integrand in terms of $s$ and $t$
> 2. Change the $xy$ region $R$ into an $st$ region $\mathcal{T}$
> 3. Change the area element by making the substitution $dx\,dy = \left| \frac{ \partial (x, y) }{ \partial (s, t) } \right|\,ds\,dt$

## Change of Coordinates in Triple Integrals

Suppose the differentiable functions
$$
x = x(s, t, u), \; y = y(s, t, u), \; z = z(s, t, u)
$$
define a one-to-one change of coordinates from a region $S$ in $stu$-space to a region $W$ in $xyz$-space.

Then, the *Jacobian* change of coordinates is given by the determinant
$$
\frac{ \partial(x, y, z) }{ \partial (s, t, u) } = \begin{vmatrix}
\frac{ \partial x }{ \partial s }  & \frac{ \partial x }{ \partial t }  & \frac{ \partial x }{ \partial u }  \\
\frac{ \partial y }{ \partial s }  & \frac{ \partial y }{ \partial t }  & \frac{ \partial y }{ \partial u }  \\
\frac{ \partial z }{ \partial s }  & \frac{ \partial z }{ \partial t }  & \frac{ \partial z }{ \partial u }
\end{vmatrix}.
$$
The Jacobian in three dimensions represents the change in the volume element.

> [!thm]+ Integrating integral with change of coordinates, three variables
> $$
> \int_{W} f(x, y, z)\,dx\,dy\,dz = \int_{S} f\Big(x(s, t, u), \, y(s, t, u), \, z(s, t, u)\Big) \, \begin{vmatrix}
> \frac{ \partial (x, y, z) }{ \partial (s, t, u) }
> \end{vmatrix}
> \,ds\,dt\,du
> $$
