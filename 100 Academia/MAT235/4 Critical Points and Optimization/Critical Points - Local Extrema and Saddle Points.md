---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[4 - Critical Points and Optimization]]"
Week: 11
Lecture:
  - "28"
  - "29"
Chapter: "15.1"
Slides/Notes: 
Date: 2024-11-18
aliases: []
Date created: Mon., Nov. 18, 2024, 11:56:04 pm
Date modified: Thu., Jan. 9, 2025, 7:05:19 pm
---

# Critical Points: Local Extrema and Saddle Points

> [!def]+ Local Maximum
> A function $f$ has a **local maximum** at point $P_0$ if:
>
> - $f(P_0) \geq f(P)$ for all points $P$ near $P_0$
> - i.e., the value at $P_0$ is greater than or equal to all nearby points
> - This creates a “peak” in the function’s graph

> [!def]+ Local Minimum
> A function $f$ has a **local minimum** at point $P_0$ if:
>
> - $f(P_0) \leq f(P)$ for all points $P$ near $P_0$
> - i.e., the value at $P_0$ is less than or equal to all nearby points
> - This creates a “valley” in the function’s graph

## Detecting Local Maxima/Minima

Recall:

- [[Gradients and Directional Derivatives in Space|Gradient vector]] points in direction of function increase when defined and nonzero
- At local max/min points ($P_0$), gradient must be $\vec{0}$ or undefined if not on boundary

### Reasoning for Local Maximum

- If $\text{grad}f(P_0)$ were defined and nonzero at local max:
    - Could increase $f$ by moving in direction of $\text{grad}f(P_0)$
    - But this contradicts definition of local max
    - Therefore $\text{grad}f(P_0) = \vec{0}$

### Reasoning for Local Minimum

- If $\text{grad}f(P_0)$ were defined and nonzero at local min:
    - Could decrease $f$ by moving opposite to $\text{grad}f(P_0)$
    - But this contradicts definition of local min
    - Therefore $\text{grad}f(P_0) = \vec{0}$

### Visual Interpretation (2 Variables)

- At local maximum:
    - Gradient vectors point inward around maximum
    - Vectors perpendicular to contours
    - At maximum point: gradient $=\vec{0}$ or undefined
- Similar behaviour at local minimum

![](https://i.imgur.com/b5HnXEK.png)

![](https://i.imgur.com/IO70OwQ.png)

## Finding Critical Points

> [!def]+ Critical Points
> Points where either:
>
> - The gradient vector $= \vec{0}$, or
> - The gradient is undefined
>
> Key properties:
>
> - All local maxima/minima (not on boundary) are critical points
> - Not all critical points are local maxima/minima

> [!info]+ To find critical points of $f$
>
> - Set $\text{grad}f = f_{x}\vec{i} + f_{y}\vec{j} + f_{x}\vec{k} = \vec{0}$
>     - Equivalent to ==setting all partial derivatives of $f$ to zero==
> - Also look for points where one or more of the partial derivatives is ==undefined==

> [!example]+ Find and analyze the critical points of $f(x,y) = x^2 - 2x + y^2 - 4y + 5$
> **Step 1:** Set partial derivatives to zero
>
> - $f_x(x,y) = 2x - 2 = 0$
> - $f_y(x,y) = 2y - 4 = 0$
>
> **Step 2:** Solve system of equations
>
> - From $f_x$: $x = 1$
> - From $f_y$: $y = 2$
> - Critical point: $(1,2)$
>
> **Step 3:** Analyze behavior
> ![|center|400](https://i.imgur.com/9J6pohY.png)
>
> - Table of values around $(1,2)$ shows all surrounding values are higher
> - Complete the square: $$f(x,y) = x^2 - 2x + y^2 - 4y + 5 = (x-1)^2 + (y-2)^2$$
>     - Vertex at $(1, 2)$
>
> ![|center|400](https://i.imgur.com/DKN2PhZ.png)
>
> **Step 4:** Conclusion
>
> - Point $(1,2)$ is a local minimum and global minimum
> - Function forms a paraboloid with vertex at $(1,2,0)$

> [!example]+ Negative Distance Function
> Given $f(x,y) = -\sqrt{x^2 + y^2}$
>
> **Step 1:** Find partial derivatives
>
> - $f_x(x,y) = -\frac{x}{\sqrt{x^2 + y^2}}$
> - $f_y(x,y) = -\frac{y}{\sqrt{x^2 + y^2}}$
>
> **Step 2:** Analyze derivatives
>
> - Partial derivatives are never simultaneously zero, but
> - Both undefined at $(0,0)$ (denominator = 0)
> - Therefore, $(0,0)$ is a critical point
>
> **Step 3:** Geometric interpretation
> ![|center|400](https://i.imgur.com/QqJaUE8.png)
>
> - Graph forms an inverted cone
> - Vertex at $(0,0)$
> - Function represents negative distance from origin
>
> **Step 4:** Conclusion
>
> - Point $(0,0)$ is both:
>     - Local maximum
>     - Global maximum
> - Value at maximum: $f(0,0) = 0$

> [!example]+ Saddle Point
> Given $g(x,y) = x^2 - y^2$
>
> **Step 1:** Find partial derivatives and set to zero
>
> - $g_x(x,y) = 2x = 0$
> - $g_y(x,y) = -2y = 0$
>
> **Step 2:** Solve system
>
> - From $g_x$: $x = 0$
> - From $g_y$: $y = 0$
> - Critical point: $(0,0)$
>
> **Step 3:** Analyze behavior
>
> - At origin: $g(0,0) = 0$
> - Along x-axis: $g(x,0) = x^2$ (opens upward)
> - Along y-axis: $g(0,y) = -y^2$ (opens downward)
> - Takes both positive and negative values near origin
>
> **Step 4:** Conclusion
>
> - Point $(0,0)$ is a critical point but:
>     - Not a local maximum
>     - Not a local minimum
>     - Forms a ==saddle point==
>     - Graph resembles a horse saddle or potato chip shape

![](https://i.imgur.com/iM4K6tC.png)

![](https://i.imgur.com/jxluZYA.png)

![](https://i.imgur.com/xqbopsH.png)

![|center|400](https://i.imgur.com/o29xR0z.png)

# Classifying Critical Points of a Function

> [!thm]+ Second-Derivative Test for Functions of Two Variables
> Suppose $(x_{0},y_{0})$ is a point where $\text{grad}f(x_{0}, y_{0}) = \vec{0}$.
> Then, the discriminant $D$ is
> $$
> D=f_{xx}(x_{0}, y_{0}) f_{yy}(x_{0}, y_{0}) - \left( f_{xy}(x_{0}, y_{0}) \right) ^{2}
> $$
> or more easily,
> $$D = f_{xx}f_{yy} - f_{xy}^{2}$$
>
> - If $D > 0$, and:
>     - $f_{xx}(x_{0}, y_{0}) > 0$:
>         - $f$ has a **local minimum** at $(x_{0}, y_{0})$
>     - $f_{xx}(x_{0}, y_{0}) < 0$:
>         - $f$ has a **local maximum** at $(x_{0}, y_{0})$
> - If $D < 0$:
>     - $f$ has a **saddle point** at $(x_{0}, y_{0})$
> - If $D = 0$:
>     - Anything can happen
>     - $f$ can have a local maximum, local minimum, saddle point, or *none* of these at $(x_{0}, y_{0})$
