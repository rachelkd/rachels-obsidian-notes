---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 17
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "46"
  - "47"
  - "48"
Chapter: "17.3"
Slides/Notes: 
Date: 2025-02-03
Date created: Mon., Feb. 17, 2025, 7:05:21 pm
Date modified: Tue., Feb. 18, 2025, 2:38:16 pm
---

# Vector Fields

## Introduction to Vector Fields

> [!def]+ Vector field
> - **Function** that assigns a vector to each point in the plane or in 3-space

- Example of a vector field:
    - Gradient of a function $f(x, y)$
    - At each point $(x, y)$, the vector $\text{grad }f(x, y)$ points in the direction of maximum rate of increase of $f$

### Velocity Vector Fields

> [!example]+ Example of a **velocity vector field**: Flow of a part of the Gulf Stream
> - Each vector shows the velocity of the current at that point
> - Current is fastest where the velocity vectors are the longest in the middle of the stream
> - Beside the stream are eddies
>     - Where water flows round and round in circles
>
> ![](https://i.imgur.com/rEvseez.png)
> *Figure 17.18: The velocity vector field of the Gulf Stream*

### Force Fields

- **Force**
    - Another physical quantity represented by a vector
- When we experience a force:
    - Sometimes results from direct contact with the object that supplies the force
    - e.g., A push
- However, many forces can be felt at all points in space
    - e.g., Earth exerts a gravitational pull on all other masses
    - & Such forces can be represented by vector fields

> [!example]+ Gravitational force exerted by the earth on a mass of one kilogram at different points in space
> ![](https://i.imgur.com/zYusHdH.png)
> *Figure 17.19: The gravitational field of the earth*
>
> - Sketch of the vector field in 3-space
> - Vectors all point toward the earth
> - Vectors farther from the earth are smaller in magnitude

^de6489

## Definition of a Vector Field

> [!def]+ Vector field
> - A **vector field** in 2-space is a function $\vec{F}(x, y)$ whose value at a point $(x, y)$ is a 2-dimensional vector
> - Similarly, a vector field in *3-space* is a function $\vec{F}(x, y, z)$ whose values are 3-dimensional vectors

> [!note]+
> - Arrow over function $\vec{F}$ indicates that its value is a ==vector==
>     - Not a scalar
> - Often represent the point $(x, y)$ or $(x, y, z)$ by its position vector $\vec{r}$
>     - Write the vector field as $\vec{F}(\vec{r})$

### Visualizing a Vector Field Given by a Formula

- Vector field is a *function* that assigns a vector to each poiunt
- → Vector field can often be given by a ==formula==

> [!example]+ Sketch the vector field in 2-space given by $\vec{F}(x, y) = -y\vec{i} + x\vec{j}$
> - To plot the vector field:
>     - Plot $\vec{F}(x, y)$ with its tail at $(x, y)$
>     - Figure 17.20
>
> ![|378x234](https://i.imgur.com/1PbmNUj.png)
>
> - $ Magnitude of the vector at $(x, y)$ is the distance from $(x, y)$ to the origin
>     - $$\| \vec{F}(x, y) \| = \| -y\vec{i} + x\vec{j} \| = \sqrt{ x^{2} + y^{2} }$$
> - & $\therefore$  All vectors at a fixed distance from the origin — i.e., on a circle centred at the origin — have the same magnitude
>     - Magnitude gets larger as we move farther from origin
> - $ At each point $(x, y)$, the vector $\vec{F}(x, y)$ is perpendicular to the position vector $\vec{r} = x\vec{i} + y\vec{j}$
>     - Confirm this using dot product:
>         - $$\vec{r} \cdot \vec{F}(x, y) = (x\vec{i} + y\vec{j}) \cdot (-y\vec{i} + x\vec{j}) = 0$$
> - & Vectors are tangent to circles centred at the origin and get longer as we go out
>
> ![|157x157](https://i.imgur.com/Pa3stva.png)
> *Figure 17.20: The value $\vec{F}(x, y)$ is placed at the point $(x, y)$*
>
> ![](https://i.imgur.com/TintMw1.png)
> *Figure 17.21: The vector field $\vec{F}(x, y) = -y\vec{i} + x\vec{j}$, vectors scaled smaller to fit in diagram*

> [!example]+ Sketch the vector fields in 2-space given by (a) $\vec{F}(x, y) = x\vec{j}$ and (b) $\vec{G}(x, y) = x \vec{i}$
> (a)
> - Vector $x\vec{j}$ is parallel to the $y$-direction
>     - Points up when $x$ is positive, and down when $x$ is negative
> - The larger $|x|$ is, the longer the vector
> - Vectors in the field are constant along vertical lines since vector field does not depend on $y$
> ![](https://i.imgur.com/bdxvSIx.png)
> *Figure 17.22: The vector field $\vec{F}(x, y) = x\vec{j}$*
>
> (b)
> - Vector $x \vec{i}$ is parallel to the $x$-direction
>     - Points to right when $x$ is positive, and points to left when $x$ is negative
> - The larger $|x|$ is, the longer the vector
> - Vectors are constant along vertical lines
>     - $\because$ Vector field does not depend on $y$
>
> ![](https://i.imgur.com/VDbfwtX.png)
> *Figure 17.23: The vector field $\vec{F}(x, y) = x \vec{i}$*

> [!example]+ Describe the vector field in 3-space given by $\vec{F}(\vec{r}) = \vec{r}$, where $\vec{r} = x \vec{i} + y\vec{j} + z\vec{k}$
> - Notation $\vec{F}(\vec{r}) = \vec{r}$ means:
>     - Value of $\vec{F}$ at the point $(x, y, z)$ with position vector $\vec{r}$ is the vector $\vec{r}$ with its tail at $(x, y, z)$
> - → Vector field points outward
>
> ![](https://i.imgur.com/YML9OTW.png)
> *Figure 17.24: The vector field $\vec{F}(\vec{r}) = \vec{r}$*

### Finding a Formula for a Vector Field

> [!example]+ Newton’s Law of Gravitation states that the magnitude of the gravitational force exerted by an object of mass $M$ on an object of mass $m$ is proportional to $M$ and $m$ and inverse proportional to the square of the distance between them. Find a formula for the vector field $\vec{F}(\vec{r})$ that represents the gravitational force, assuming $M$ is located at the origin and $m$ is located at the point with position vector $\vec{r}$.
> - Magnitude of the force is given by
>     $$
>     \| \vec{F}(\vec{r}) \| = \frac{GMm}{\| \vec{r} \|^{2}},
>     $$
>     - where $G$ is the universal gravitational constant
>     - Since mass $m$ is located at $\vec{r}$
> - Unit vector in the direction of the force is $\frac{-\vec{r}}{\| \vec{r} \|}$
>     - Negative sign indicates that the direction of force is toward the origin
>         - Gravity is attractive
> - Take the product of the magnitude of the force and a unit vector in the direction of the force
>     - → Obtain an expression for the force vector field
>
> $$
> \vec{F}(\vec{r}) = \frac{GMm}{\| \vec{r} \|^{2}} \left( - \frac{\vec{r}}{\| r \| ^{2}} \right) = \frac{-GMm\vec{r}}{\|r^{3}\|}
> $$
>
> - Vector field is in [[#^de6489|Figure 17.19]]

## Gradient Vector Fields

- **Gradient field of $f$**
    - ==Vector field== of the gradient of a scalar function $f$
    - Recall:
        - Gradient is a function that assigns a vector to each point
        - → Is a vector field

> [!info] Many vector fields in physics are gradient fields.

![](https://i.imgur.com/dvyKKnr.png)
*Figure 17.26: The contour map of $f(x, y) = x^{2} + 2y^{2}$*

![](https://i.imgur.com/k2yQh5X.png)
*Figure 17.27: The contour map of $g(x, y) = 5 - x^{2} - 2y^{2}$*

![](https://i.imgur.com/QZXY9WZ.png)
*Figure 17.28: The contour map of $h(x, y) = x + 2y + 3$*

> [!example]+ Sketch the gradient field of the functions in Figures 17.26-28

- For a function $f(x, y)$:
    - Gradient vector of $f$ at a point is perpendicular to the contours in the direction of increasing $f$ and its magnitude is the rate of change in that direction
    - Rate of change is large when contours are close together
        - Small when contours are far apart
- $f(x, y) = x^{2} + 2y^{2}$:
    - Vectors all point outward, away from local minimum of $f$
    - ![](https://i.imgur.com/XTT6gvk.png)
- $g(x, y)$:
    - Vectors of $\text{grad }g$ all point inward, toward the local maximum of $g$
    - ![](https://i.imgur.com/H1HW14k.png)
- $h$:
    - Linear function
    - Gradient is constant $\implies$ $\text{grad }h$ is a constant vector field
    - ![](https://i.imgur.com/GIbDLA5.png)
