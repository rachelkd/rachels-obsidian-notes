---
dg-publish: false
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[4 - Critical Points and Optimization]]"
Week: 12
Lecture:
  - "30"
  - "31"
  - "32"
Chapter: "15.3"
Slides/Notes: 
Date: 2024-12-02
aliases: []
Date created: Tue., Jan. 7, 2025, 8:39:22 pm
Date modified: Sun., Feb. 9, 2025, 7:35:12 pm
---

# Constrained Optimization: Lagrange Multipliers

## Graphical Approach: Maximizing Production Subject to a Budget Constraint

Suppose we want to maximize production under a budget constraint.
Suppose production $f$ is a function of two variables, $x,y$, which are quantities of two raw materials, and that
$$
f(x, y) = x^{2/3}y^{1/3}.
$$
If $x,y$ are purchased at prices of $p_{1}$ and $p_{2}$ thousand of dollars per unit, what is the maximum production $f$ that can be obtained with a budget of $c$ thousand dollars?

- To maximize $f$ without regard to the budget:
    - Simply increase $x$ and $y$
- Budget constraint prevents us from increasing $x, y$ beyond a certain point
    - Amount spent on $x$ is $p_{1} x$
    - Amount spent on $y$ is $p_{2}y$

$$
g(x, y) = p_{1}x + p_{2}y \leq c
$$
where $g(x,y)$ is the total cost of the raw materials and $c$ is the budget in thousand of dollars.

Let $p_{1} = p_{2} = 1, c = 3.78$. Then,
$$
x + y \leq 3.78.
$$

![|304x129](https://i.imgur.com/y5QIm8x.png)

- & To maximize $f$, find the point which lies on the level curve with the largest possible value of $f$ *and* which lies within the budget
    - Point must lie on the budget constraint
        - Production is maximized when we spend all the money
- We can increase $f$ by moving in some direction along the line representing the budget constraint
    - Unless we are at a point where budget constraint is tangent to a contour of $f$
    - e.g., If we are on the line to the left of the point of tangency, moving right on the constraint will increase $f$
        - If we are on the line to the right of the point of tangency, moving left will increase $f$
- $\implies$ Maximum value of $f$ on budget constraint occurs at point where budget constraint is tangent to the contour $f = 2$

![|606](https://i.imgur.com/J6OkcIG.png)

## Analytical Solution: Lagrange Multipliers

> [!obs]+ Figure 15.26 suggests that max production occurs at the point where budget constraint is tangent to a level curve of the production function.
> - Method of **Lagrange multipliers** uses this fact in algebraic form

- Figure 15.27 shows:
    - At the optimum point $P$:
    - & ==Gradient of $f$== and ==normal== to the budget line $g(x, y) = x + y = 3.78$ are parallel
- At $P$, $\text{grad } f$ and $\text{grad } g$ are parallel

> [!def]+ Lagrange Multiplier
> For some scalar $\lambda$ called the **Lagrange multiplier**,
> $$
> \text{grad } f = \lambda \;\text{grad } g
> $$

Calculating the gradients,

$$
\left( \frac{2}{3}x^{-1/3} y^{1/3} \right) \vec{i} + \left( \frac{1}{3} x ^{2/3} y ^{-2/3} \right) \vec{j} = \lambda \left( \vec{i} + \vec{j} \right) .
$$
Equating the components gives
$$
\frac{2}{3}x ^{-1/3}y^{1/3} = \lambda \;\;\;\; \text{and} \;\;\;\; \frac{1}{3}x^{2/3} y^{-2/3} = \lambda
$$
Eliminating $\lambda$ gives
$$
\frac{2}{3}x^{-1/3}y^{1/3} = \frac{1}{3}x^{2/3}y^{-2/3} \implies 2y = x
$$
Since $x + y = 3.78$ must be satisfied, then $x = 2.52, y = 1.26$. Then,
$$
f(2.52, 1.26) = (2.52)^{2/3}(1.26)^{1/3} \approx 2
$$

## Lagrange Multipliers in General

Suppose we want to optimize an **objective function** $f(x, y)$ subject to a **constraint** $g(x, y) = c$.

- Look for extrema among the points which satisfy the constraint

> [!def]+ Local maximum subject to the constraint
> Suppose $P_{0}$ is a point satisfying the constraint $g(x, y) = c$.
> - $f$ has a **local maximum** at $P_{0}$ **subject to the constraint** if $f(P_{0}) \geq f(P)$ for all points $P$ near $P_{0}$ satisfying the constraint

> [!def]+ Global maximum subject to the constraint
> Suppose $P_{0}$ is a point satisfying the constraint $g(x, y) = c$.
> - $f$ has a **global maximum** at $P_{0}$ **subject to the constraint** if $f(P_{0}) \geq f(P)$ for all points $P$ satisfying the constraint

Local and global minima are defined similarly.

> [!thm]+ Optimizing $f$ subject to the constraint $g = c$
> If a smooth function $f$ has a maximum or minimum subject to a smooth constraint $g(x, y) = c$ at a point $P_{0}$, then either $P_{0}$ satisfies the equations
> $$
> \text{grad }f = \lambda \text{ grad }g \;\;\;\; \text{and} \;\;\;\; g(x, y) = c,
> $$
> or $P_{0}$ is an endpoint of the constraint, or $g(P_{0}) = \vec{0}$.
> To investigate whether $P_{0}$ is a global maximum or minimum, compare values of $f$ at the points satisfying these three conditions.

> [!tip]+ Strategy for optimizing $f(x, y)$ subject to the constraint $g(x, y) \leq c$
> - Find all points in the region $g(x, y) < c$ where $\text{grad }f$ is $\vec{0}$ or undefined
> - Use Lagrange multipliers to find the local extrema of $f$ on the boundary $g(x, y) =c$
> - Evaluate $f$ at the points found in the previous two steps and compare the values
