---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[5 - Double and Triple Integrals]]"
Week: 13
Lecture:
  - "34"
Chapter: "16.1"
Slides/Notes:
  - https://share.goodnotes.com/s/u2UysLCieR6ie1rL01kkMy
Date: 2025-01-06
aliases: []
Date created: Sun., Jan. 5, 2025, 9:55:38 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# The Definite Integral of a Function of Two Variables

Recall

> [!def]+ Definite integral of a continuous one-variable function, $f$
> - A limit of Riemann sums
>
> $$
> \int_{a}^{b} f(x) \, dx = \lim_{ \Delta x \to 0 } \sum_{i} f(x_{i}) \Delta x
> $$
>
> where $x_{i}$ is a point in the $i$-th subdivision of the interval $\left[ a, b \right]$.

## Definition of the Definite Integral

### Defining the Definite Integral for a Function $f$ of Two Variables on a Rectangular Region

Given a continuous function $f(x, y)$ defined on a region $a \leq x \leq b$ and $c \leq y \leq d$,

- We subdivide each of the intervals $a \leq x \leq b$ and $c \leq y \leq d$ into $n$ and $m$ equal subintervals respectively, giving
- $nm$ sub-rectangles

![|center|642](https://i.imgur.com/KSJWKAr.png)

- *Area* of each sub-rectangle is $\Delta A = \Delta x \, \Delta y$, where
    - $\Delta x = \frac{{(b-a)}}{n}$ is the *width* of each subdivision on the ==$x$-axis==
    - $\Delta y = \frac{(d-c)}{m}$ is the *width* of each subdivision on the ==$y$-axis==

> [!tip]+ To compute the Riemann sum
> - Multiply the *area* of each sub-rectangle by the value of the function at a point in the rectangle
> - Add the resulting numbers

> [!def]+ Upper sum
> $$
> \sum_{i, j} M_{ij} \Delta x \Delta y
> $$
>
> - Choose the maximum value $M_{ij}$ of the function on each rectangle
> - Add for all $i, j$

> [!def]+ Lower sum
>
> $$
> \sum_{i, j} L_{ij} \Delta x \Delta y
> $$
> - Take the minimum value on each sub-rectangle

> [!thm]+ Property of Riemann sums
> If $\left( u_{ij}, v_{ij} \right)$ is any point in the $ij$-th sub-rectangle, any other Riemann sums satisfies
>
> $$
> \sum_{i,j} L_{ij} \Delta x \Delta y \leq \sum_{i, j} f(u_{ij}, v_{ij}) \Delta x \Delta y \leq \sum_{i,j} M_{ij} \Delta x \Delta y
> $$

- We define the **definite integral** by taking the limit as the ==numbers of subdivisions, $n$ and $m$, tend to infinity==
- By comparing upper and lower sums, it can be shown that the ==limit exists when the function $f$ is *continuous*==
    - Get the same limit by letting $\Delta x$ and $\Delta y$ tend to 0

> [!def]+ Double integral; definite integral of $f$ over $R$
> Suppose $f$ is continuous on $R$, the rectangle $a \leq x \leq b, c \leq y \leq d$.
> If $(u_{ij}, v_{ij})$ is any point in the $ij$-th sub-rectangle,
>
> $$
> \int_{R} f \, dA = \lim_{ \Delta x, \Delta y \to 0 }  \sum_{i, j} f(u_{ij}, v_{ij}) \Delta x \Delta y
> $$
> Such an integral is called a **double integral**.

#### Case When $R$ is not Rectangular

- Sometimes think of $dA$ as being the area of an infinitesimal rectangle of length $dx$ and height $dy$
    - $dA = dx\,dy$

> [!def]+ Double integral when $R$ is not rectangular
> $$
> \int_{R} f \, dA = \int_{R} f(x, y)\,dx\,dy
> $$

## Interpretation of the Double Integral as Volume

- Double integral of a positive two-variable function can be interpreted as a *volume*
    - Similar to the definite integral of a positive one-variable function interpreted as an area
- In one-variable case:
    - Visualize the Riemann sums as the total area of rectangles above the subdivisions
- Two-variable case:
    - Visualize Riemann sums as ==solid bars== vs. rectangles
    - Number of subdivisions grows → Tops of the bars approximate surface better → Volume of the bars gets closer to the volume under the graph of the function

![](https://i.imgur.com/d0MTnf0.png)

> [!thm]+ Volume under graph of $f$ above region $R$
> If $x, y, z$ represent length and $f$ is positive, then
>
> $$
> \text{Volume under graph of } f \text{ above region } R = \int_{R} f \, dA
> $$

### Interpretation of the Double Integral as Area

> [!important]+ Special case:
> - $f(x, y) = 1$ for all points $(x, y)$ in the region $R$

- Each term in the Riemann sum is of the form $1 \cdot \Delta A = \Delta A$
- → Double integral gives the area of the region $R$

$$
\text{Area}(R) = \int_{R} 1 \, dA
$$

## Interpretation of the Double Integral as Average Value

> [!thm]+ Average value of $f$ on the region $R$
> $$
> \begin{align}
> &=\frac{1}{\text{Area of }R} \int_{R} f \, dA  \\
> \implies &\text{Average value} \times \text{Area of }R = \int_{R} f \, dA
> \end{align}
> $$

- Think of the average value of $f$ as the *height* of the box with the same volume that is on the same base

![](https://i.imgur.com/aPsLP1W.png)

## Integrals over Regions that Are Not Rectangles

- Extend definition to regions of other shapes, including triangles, circles, regions bounded by the graphs of piecewise continuous functions

### Approximate Definite Integral over $R$ Which is not Rectangular

- Use a grid of rectangles approximating the region
- Obtain grid by enclosing $R$ is a large rectangle and subdividing that rectangle
- Consider sub-rectangles which are inside $R$
- Pick a point $(u_{ij}, v_{ij})$ in each sub-rectangle and form a Riemann sum
    - $$\sum_{i, j} f(u_{ij}, v_{ij}) \Delta x \Delta y$$
- Sum over only those ==sub-rectangles within $R$==
    - As subdivisions become finer → Grid approximates region $R$ more closely

> [!def]+ Definite integral for a function $f$ continuous on $R$
> $$
> \lim_{ \Delta x, \Delta y \to 0 } \sum_{i, j} f(u_{ij}, v_{ij}) \Delta x \Delta y
> $$
> where the Riemann sum is taken over the sub-rectangles inside $R$

- ? Why can we omit rectangles which cover the edge of $R$?
    - For any region, area of sub-rectangles covering the edge tends to 0 as the grid becomes finer
    - → Does not affect limit

## Convergence of Upper and Lower Sums to Same Limit

> [!obs] If $f$ is continuous on the rectangle $R$, then the difference between upper and lower sums for $f$ converges to 0 as $\Delta x$ and $\Delta y$ approach 0.

> [!example]+ Let $f(x, y) = x^{2}y$ and let $R$ be the rectangle $0 \leq x \leq 1, 0 \leq y \leq 1$. Show that the difference between upper and lower Riemann sums for $f$ on $R$ converges to 0, as $\Delta x$ and $\Delta y$ approach 0.
>
> The difference between the sums is
> $$
> \sum M_{ij} \Delta x \Delta y - \sum L_{ij} \Delta x\Delta y = \sum \left( M_{ij} - L_{ij} \right) \Delta x \Delta y,
> $$
> where $M_{ij}$ and $L_{ij}$ are the maximum and minimum of $f$ on the $ij$-th sub-rectangle.
>
> - $f$ increases in both the $x$ and $y$ directions
>     - $\implies M_{ij}$ occurs at the corner of the sub-rectangle farthest from the origin
>     - $\implies L_{ij}$ occurs at the closest corner from the origin
> - Since slopes in $x$ and $y$ direction do not decrease as $x, y$ increase
>     - Difference $M_{ij} - L_{ij}$ is largest in the sub-rectangle $R_{nm}$ which is farthest from the origin, so
>
> $$
> \sum (M_{ij} - L_{ij}) \Delta x \Delta y \leq (M_{nm} - L_{nm}) \sum \Delta x \Delta y = (M_{nm} - L_{nm}) \text{Area}(R)
> $$
>
> - $\implies$ Difference converges to 0 as long as $(M_{nm} - L_{nm})$ does
> - Maximum $M_{nm}$ of $f$ on the $nm$-th sub-rectangle occurs at $(1, 1)$
>     - Sub-rectangle’s top right corner
> - Minimum $L_{nm}$ of $f$ occurs at opposite corner $\left( 1 - \frac{1}{n}, 1 - \frac{1}{m} \right)$
>
> Substituting into $f(x, y) = x^{2}y$,
> $$
> M_{nm} - L_{nm} = (1)^{2}(1) - \left( 1 - \frac{1}{n} \right) ^{2} \left( 1-\frac{1}{m} \right) = \frac{2}{n} - \frac{1}{n^{2}} + \frac{1}{m} - \frac{2}{nm} + \frac{1}{n^{2}m}.
> $$
> The right-hand side converges to 0 as $n, m \to \infty$ i.e., as $\Delta x, \Delta y \to 0$.
