---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 8
Lecture:
  - "20"
Chapter: 14.3
Slides/Notes:
  - https://share.goodnotes.com/s/2ogzPjs5IPnJfUAN1BR4ZT
  - https://share.goodnotes.com/s/Lco8sI0ZkTwUcGSWu2R9jq
Date: 2024-10-22
aliases: []
Date created: Tue., Oct. 22, 2024, 12:22:00 am
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Local Linearity

- For a function of one-variable:
    - As we zoom in on graph → Looks like a straight line
- For a graph of a two-variable function:
    - As we zoom in → Looks like a plane
- ![|500](https://i.imgur.com/eaW37yu.png)
- ![](https://i.imgur.com/pmU0jlI.png)
    - As we zoom in → Contours look more like equally parallel lines (contours of a linear function)
- [*] Effect can also be seen *numerically* (zooming in with table of values
    - ![](https://i.imgur.com/G5Y4T3d.png)

> [!obs]+ Seeing a plane when we zoom in at a point tells us that $f(x, y)$ is closely approximated near that point by a linear function, $L(x, y)$
> $$f(x, y) \approx L(x, y)$$

# The Tangent Plane

- Graph of the function $z = L(x, y)$ is the **tangent plane** at that point
- [*] We say that $f(x, y)$ is *differentiable* at the point

> [!question]+ What is the equation of the tangent plane?
> At the point $(a, b)$,
>
> - $x$-slope of graph of $f$ is the partial derivative $f_{y}(a, b)$
> - $y$-slope is $f_{y}(a, b)$

> [!def]+ Tangent plane to the surface $z = f(x, y)$ at the point $(a, b)$
> Assuming $f$ is differentiable at $(a, b)$, the equation of the tangent plane is
> $$z = f(a, b) + f_{x}(a, b)(x - a) + f_{y}(a, b)(y - b)$$
> ![|center|500](https://i.imgur.com/09ZTcIw.png)

- Deriving this equation:
    - We know: $$\begin{align*} z &= f_{x}(a, b)x + f_{y}(a, b)y + c \\ f(a, b) &= f_{x}(a,b)a + f_{y}(a, b)b + c \\ \implies c &= f(a, b) - f_{x}(a, b)a - f_{y}(a, b)b \\ \implies z &= f(a, b) + f_{x}(a, b)(x - a) + f_{y}(a, b)(y - b) \end{align*}$$

# Local Linearization

- Tangent plane lies close to the surface near the point at which they meet $\implies$ $z$-values on tangent plane are close to values of $f(x, y)$ for points near $(a, b)$
- Replace $z$ by $f(x, y)$ in equation of tangent plane gives us an *approximation*

> [!def]+ Tangent plane approximation to $f(x, y)$ for $(x, y)$ near the point $(a, b)$
> Provided $f$ is differentiable at $(a, b)$,
> $$f(x, y) \approx f(a, b) + f_{x}(a, b)(x - a) + f_{y}(a, b)(y - b)$$
> ![|center|500](https://i.imgur.com/pgIGbhq.png)

- Thinking of $a$ and $b$ as fixed → Expression on right side is linear in $x$ and $y$
- Right side of approximation gives the **local linearization** of $f$ near $x = a, y = b$

# The Differential

> [!info]+ We are often interested in change in the value of the function as we move from point $(a, b)$ to a nearby point $(x, y)$
>
> - Rewrite tangent plane approximation as $$\begin{align*} \underbrace{f(x, y) - f(a, b)}_{\Delta f} &\approx f_{x}(a, b)\underbrace{(x-a)}_{\Delta x} + f_{y}(a, b)\underbrace{(y-b)}_{\Delta y} \end{align*}$$
> $$\implies \Delta f \approx f_{x}(a, b) \Delta x + f_{y}(a, b) \Delta y$$

- If $a, b$ are fixed, $\Delta f \approx f_{x}(a, b) \Delta x + f_{y}(a, b) \Delta y$ is a linear function of $\Delta x, \Delta y$ that can be used to estimate $\Delta f$ for small $\Delta x$ and $\Delta y$

> [!def]+ Differential of a function $z = f(x, y)$
> The **differential**, $df$ or $dz$, at a point $(a, b)$ is the linear function of $dx$ and $dy$ given by
> $$df = f_{x}(a, b)dx + f_{y}(a, b)dy$$
> Differential at a general point is often written $df = f_{x}dx + f_{y}dy$
