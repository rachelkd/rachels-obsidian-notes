---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 14
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "38"
  - "39"
Chapter: "16.4"
Slides/Notes: 
Date: 2025-01-15
Date created: Thu., Jan. 16, 2025, 8:00:20 pm
Date modified: Wed., Feb. 12, 2025, 7:23:07 pm
---

# Double Integrals in Polar Coordinates

## Integration in Polar Coordinates

Consider the following example:

- A biologist studying insect populations around a circular lake divides the area into the polar sectors of radii 2, 3, and 4 km.
- The approximate population density in each sector is shown in millions per square km.
- ? Estimate the total insect population around the lake.

![](https://i.imgur.com/iAXxIeQ.png)

- Intuition:
    - Multiply the population density in each sector by the area of that sector
    - Sectors in grid do not have the same area
        - Unlike rectangles in rectangular grid
    - Inner sectors have area
        $$
        \frac{1}{4}(\pi 3^{2} - \pi 2^{2}) = \frac{5\pi}{4} \approx 3.93 \text{ km}^{2}
        $$
    - Outer sectors have area
        $$
        \frac{1}{4}(\pi 4^{2} - \pi 3^{2}) = \frac{7\pi}{4} \approx 5.50 \text{ km}^{2}
        $$
$$
\begin{align}
\text{Population}  & \approx (3.93)(20 + 17 + 14 + 17) + (5.50)(13 + 10 + 8 + 10) \\
 & = 492.74 \text{ million insects}
\end{align}
$$

## What is $dA$ in Polar Coordinates?

- **Polar grid**
    - Built out of ==circles== and ==rays==
- $r = k$
    - A circle of radius $k$ centered at the origin
- $\theta = l$
    - A ray emanating from the origin at angle $l$ with the $x$-axis

Suppose we want to integrate $f(r, \theta)$ over the region $R$ in Figure 16.34.

![|460](https://i.imgur.com/F7TjWdn.png)

- Choosing $(r_{ij}, \theta_{ij})$ in the $ij$-th bent rectangle gives a *Riemann sum*
    $$
    \sum_{i,j} f(r_{ij}, \theta_{ij}) \Delta A
    $$
- Area $\Delta A$:
    - If $\Delta r, \Delta\theta$ are small, the shaded region is approximately a rectangle with sides $r \Delta\theta$ and $\Delta r$, so
        $$
        \Delta A \approx r\Delta \theta \Delta r
        $$
    ![|316](https://i.imgur.com/XpUiOkz.png)

> [!thm]+ Riemann sum
> $$
> \sum_{i, j} f(r_{ij}, \theta_{ij}) r_{ij} \Delta \theta \Delta r
> $$

If we take the limit as $\Delta r, \Delta \theta \to 0$, we obtain:

> [!def]+ Iterated integral in polar coordinates
> $$
> \int_{R} f \, dA = \int_{\alpha}^{\beta} \int_{a}^{b} f(r, \theta)r \, dr  \, d\theta 
> $$

> [!thm]+ From Cartesian coordinates to polar coordinates
> - $x = r \cos \theta$
> - $y = r\sin\theta$
> - $x^{2} + y^{2} = r^{2}$
> - $dA = r \, dr \, d\theta = r \, d\theta \, dr$

### Examples

> [!example]+ Compute the integral of $f(x, y) = \frac{1}{\left( x^{2} + y^{2} \right)^{3/2}}$ over the region $R$ shown below.
> ![|300](https://i.imgur.com/394pAgN.png)

- Region $R$ is described by the inequalities:
    - $1 \leq r \leq 2$
    - $0 \leq \theta \leq \frac{\pi}{4}$
- & In polar coordinates, $r = \sqrt{ x^{2} + y^{2} }$

Write $f$ as

$$
f(x, y) = \frac{1}{\left( x^{2} + y^{2} \right) ^{3/2}} = \frac{1}{(r^{2})^{3/2}} = \frac{1}{r^{3}}
$$

Then,

$$
\begin{align}
\int_{R} f \, dA & = \int_{0}^{\pi/4} \int_{1}^{2} \frac{1}{r^{3}} r \, dr  \, d\theta \\
& = \int_{0}^{\pi/4} \left( \int_{1}^{2} r^{-2} \, dr  \right)  \, d\theta \\
& = \int_{0}^{\pi/4} \left. -\frac{1}{r} \right| _{r = 1}^{r = 2} \, d\theta \\
& = \int_{0}^{\pi/4} \frac{1}{2} \, d\theta \\
& = \frac{\pi}{8}
\end{align} 
$$
