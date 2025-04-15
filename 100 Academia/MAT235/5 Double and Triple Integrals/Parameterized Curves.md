---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 16
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "43"
  - "44"
  - "45"
Chapter: "17.1"
Slides/Notes: 
Date: 2025-01-27
Date created: Wed., Jan. 29, 2025, 4:08:29 am
Date modified: Mon., Feb. 17, 2025, 7:04:33 pm
---

# Parameterized Curves

- A curve in the plane may be parameterized by a pair of equations of the form $x = f(t), y = g(t)$
- As parameter $t$ changes → Point $(x, y)$ traces out the curve

## Parametric Equations in Three Dimensions

- Describe motion in the *plane* by giving parametric equations for $x, y$ in terms of $t$
- To describe motion in 3-space parametrically;
    - Need a third equation giving $z$ in terms of $t$

> [!example]+ Find parametric equations for the curve $y = x^{2}$ in the $xy$-plane.
> A possible parametrization in two dimensions is:
>
> - $x = t$
> - $y = t^{2}$
> - $z$-coordinate is zero
>     - Since curve is in $xy$-plane
>
> A parameterization in three dimensions is
> $$
> x = t, \; y = t^{2}, \; z = 0
> $$

> [!example]+ Find parametric equations for a particle that starts at $(0, 3, 0)$ and moves around in a circle as shown in Figure 17.1.
> ![|400](https://i.imgur.com/k0eogCW.png)
> - Motion is in $yz$-plane
>     - $\implies x = 0$ at all times $t$
> - Looking at $yz$-plane from positive $x$-direction:
>     - Motion around a circle of radius 3 in the *clockwise* direction
>
> $$
> x = 0, \; y = 3 \cos t, \; z = -3 \sin t
> $$

> [!example]+ Describe in words the motion given parametrically by $$x = \cos t, \; y = \sin t, \; z = t.$$
> - Particle’s $x$ and $y$-coordinates give circular motion in the $xy$-plane
> - $z$-coordinate increases steadily
> - → Particle traces out a **helix**
>     - i.e., Rising spiral, coiled spring
>
> ![|505](https://i.imgur.com/lFUR8il.png)

### Parametric Equations of a Line

> [!thm]+ **Parametric Equations of a Line** through the point $(x_{0}, y_{0}, z_{0})$ and parallel to the vector $a \vec{i} + b \vec{j} + c \vec{k}$ are
> $$
> x = x_{0} + at, \; y = y_{0} + bt, \; z = z_{0} + ct
> $$

- Coordinates $x, y, z$ are linear functions of the parameter $t$

> [!example]+ Find parametric equations for the line parallel to the vector $2\vec{i} + 3\vec{j} + 4\vec{k}$ and through the point $(1, 5, 7)$.
> Imagine a particle at point $(1, 5, 7)$ at time $t = 0$, and moving through a displacement of $2\vec{i} + 3\vec{j} + 4\vec{k}$ for each unit of time $t$.
>
> When $t = 0$:
>
> - $x = 1$
> - $x$ increases by 2 units for every unit of time
> - & $\implies x = 1 + 2t$
> - $y$-coordinate starts at $y = 5$
> - $y$ increases at a rate of 3 units for every unit of time
> - & $\implies y = 5 + 3t$
> - $z$-coordinate starts at $z = 7$
> - $z$ increases at a rate of 4 units for every unit of time
> - & $\implies z = 7 + 4t$

## Vector Form of Parameterized Curves

- A point in the plane $(x, y)$ can be represented by the position vector $\vec{r} = x \vec{i} + y \vec{j}$
- In 3-space:
    - $\vec{r} = x \vec{i} + y \vec{j} + z \vec{k}$

![|400](https://i.imgur.com/MWHsEZd.png) ![|400](https://i.imgur.com/vnfpBRa.png)

> [!def]+ Parameterization
> We can write the parametric equations $x = f(t), y = g(t), z = h(t)$ as a single vector equation
> $$
> \vec{r}(t) = f(t) \vec{i} + g(t) \vec{j} + h(t) \vec{k}
> $$

- As parameter $t$ varies, point with position vector $\vec{r}(t)$ traces out a curve in 3-space
    - e.g., Circular motion in the plane $x = \cos t, y = \sin t$ can be written as:
        - $\vec{r} = (\cos t) \vec{i} + (\sin t) \vec{j}$
    - Helix in 3-space $x = \cos t, y = \sin t, z = t$ can be written as:
        - $\vec{r} = (\cos t) \vec{i} + (\sin t)\vec{j} + t\vec{k}$
        - ![](https://i.imgur.com/CEx3du9.png)

> [!example]+ Use vectors to give a parameterization for the circle of radius $\frac{1}{2}$ centred at point $(-1, 2)$.
> - Circle of radius 1 centred at origina is parameterized by vector-valued function $$\vec{r_{1}}(t) = \cos t \vec{i} + \sin t \vec{j}, \; 0 \leq t \leq 2\pi$$
> ![|503](https://i.imgur.com/GZnAq3l.png)
>
> - Point $(-1, 2)$ has position vector $\vec{r_{0}} = -\vec{i} + 2\vec{j}$
> - & Position vector, $\vec{r}(t)$, of a point on circle of radius $\frac{1}{2}$ centred at $(-1, 2)$ is found by adding $\frac{1}{2}\vec{r_{1}}$ to $\vec{r_{0}}$
>
> $$
> \vec{r}(t) = \vec{r_{0}} + \frac{1}{2} \vec{r_{1}}(t) = \left( -1 + \frac{1}{2} \cos t \right) \vec{i} + \left( 2 + \frac{1}{2} \sin t \right) \vec{j}.
> $$
> Equivalently,
> $$
> x = -1 + \frac{1}{2} \cos t, \;\; y = 2 + \frac{1}{2} \sin t, \;\; 0 \leq t \leq 2\pi
> $$
> ![|480](https://i.imgur.com/1eHvMMr.png)

### Parametric Equation of a Line

Consider a straight line in the direction of a vector $\vec{v}$ passing through the point $(x_{0}, y_{0}, z_{0})$ with position vector $\vec{r_{0}}$.

- We start at $\vec{r_{0}}$ and move up and down the line
    - Adding different multiples of $\vec{v}$ to $\vec{r_{0}}$

![|406](https://i.imgur.com/XoUXOtd.png)

- $ Every point on the line can be written as $\vec{r_{0}} + t\vec{v}$

> [!thm]+ Parametric Equation of a Line in Vector Form
> The line through the point with position vector $\vec{r_{0}} = x_{0}\vec{i} + y_{0} \vec{j} + z_{0}\vec{k}$ in the direction of vector $\vec{v} = a\vec{i} + b\vec{j} + c\vec{k}$ has parametric equation
> $$
> \vec{r}(t) = \vec{r_{0}} + t\vec{v}
> $$

## Intersection of Curves and Surfaces

- Parametric equations for a curve enable us to:
    - & Find where a curve intersects a surface

> [!example]+ Find the points at which the line $x = t, y = 2t, z = 1 + t$ pierces the sphere of radius 10 centered at the origin.
>
> - Equation for the sphere of radius 10 and centred at the origin:
> $$
> x^{2} + y^{2} + z^{2} = 100
> $$
> - To find the intersection points of the line and the sphere:
>     - & Substitute parametric equations of the line into equation of the sphere
>
> $$
> \begin{align}
> t^{2} + 4t^{2} + (1 + t)^{2}  & = 100 \\
> \implies 6t^{2} + 2t - 99  & = 0
> \end{align}
> $$
> - Equation has two solutions at approximately:
>     - $t = -4.23$
>     - $t = 3.90$
> - Using parametric equation for the line $(x, y, z) = (t, 2t, 1 + t)$:
>     - Line cuts sphere at two points:
>         - $(x, y, z) = (-4.23, 2(-4.23), 1 + (-4.23)) + (-4.23, -8.46, -3.23)$
>         - $(x, y, z) = (3.90, 7.80, 4.90)$

> [!tip] We can use parametric equations to find the intersection of ==two curves==.

> [!example]+ Two particles move through space, with equations $\vec{r_{1}}(t) = t\vec{i} + (1 + 2t)\vec{j} + (3-2t)\vec{k}$ and $\vec{r_{2}}(t) = (-2 -2t)\vec{i} + (1-2t)\vec{j} + (1+t)\vec{k}$. Do the particles ever collide? Do their paths cross?
>
> - If particles *collide*, they pass through same point at same time $t$
>     - % Must find a solution to vector equation $\vec{r_{1}}(t) = \vec{r_{2}}(t)$
> - Equivalent to finding a common solution to the three scalar equations:
>     - $t = -2-2t \implies t = -\frac{2}{3}$
>     - $1 + 2t = 1 - 2t \implies t = 0$
>     - $3-2t = 1 + t \implies t = \frac{2}{3}$
>     - → No common solution, so particles do not collide
> - If particle paths *cross*:
>     - % Find out if they pass through same point at two possibly different times $t_{1}, t_{2}$
> $$
> \begin{align}
> t_{1} = -2-2t_{2},  &  &  1 + 2t_{1} = 1 - 2t_{2},  &  &  3-2t_{1}=1+t_{2}
> \end{align}
> $$
> - Solve the first two equations simultaneously: $t_{1} = 2, t_{2} = -2$
> - These values also satisfy third equation
>     - → Paths cross
> - Position of first particle at time $t = 2$ is the same position of second particle at time $t = -2$
>     - At point $(2, 5, -1)$
