---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 9
Lecture:
  - "24"
Chapter: "14.5"
Slides/Notes:
  - https://share.goodnotes.com/s/8oHkJH7bPyjF7d5oGBcB9W
Date: 2024-11-06
aliases: []
Date created: Wed., Nov. 6, 2024, 5:12:28 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# $z = f(x, y)$

- Graph of $f$ lives in 3-dim space
- Domain of $f$ lives in 2-dim space

## Facts about $\nabla f(x, y)$

- Gradient lives in 2-dim space
- Gradient points in the direction that causes the fastest increase in the value of $z$
- $\| \nabla f(x,y) \|$ gives the **magnitude** of the fastest increase in $z$
- $\nabla f(x, y)$ is **orthogonal** to the level curves of $f$
- If $\vec{u} = u_{1}\vec{i} + u_{2}\vec{j}$ is a *unit vector*, then $$D_{\vec{u}}f(a, b) = \nabla  f(a, b) \cdot \vec{u}$$

# $w = g(x, Y, z)$

- The graph of $g$ lives in 4-dim space
- The domain of $g$ lives in 3-dim space

## Facts about $\nabla f(x, y)$

- Gradient lives in 3-dim space
    - Components are the partial derivatives for every variable
- Gradient points in the direction that causes the fastest increase in the value of $w$
- $\| \nabla g(x, y, z) \|$ gives the magnitude of the fastest increase in $w$
- $\nabla g(x,y,z)$ is normal to the **level surfaces** of $g$
- If $\vec{u} = u_{1}\vec{i} + u_{2}\vec{j} + u_{3}\vec{k}$ is a unit vector in 3 space, then $$D_{\vec{u}}g(a, b, c) = (\nabla g)(a, b, c) \cdot \vec{u}$$

# Example. Temperature

$$T(x, y, z) = 2x^{2}yz$$

- $T$ → degrees Fahrenheit
- $x, y, z$ → measured in meters

> [!question]+ How fast does $T$ change as you move away from $P = (1, 1, 2)$ in the direction of the origin?
> $$\vec{P0} = \begin{pmatrix} 0 - 1 \\ 0 - 1 \\ 0 - 2 \end{pmatrix} = \begin{pmatrix} -1 \\ -1 \\ -2 \end{pmatrix}$$
> $\implies$ The unit vector $\vec{u} = \frac{1}{\sqrt{ 6 }}\begin{pmatrix}-1 \\ -1 \\ -2\end{pmatrix}$
> Then, $$ \begin{align*} \nabla T (1, 1, 2) &= \left. \begin{pmatrix} 4xyz \\ 2x^{2}z \\ 2x^{2}y \end{pmatrix} \right|_{(x,y,z) = (1,1,2)} \\ &= \begin{pmatrix}
8 \\ 4 \\ 2
\end{pmatrix} \end{align*} $$
> $$\begin{align*}

\implies T_{\vec{u}}(1,1,2) &= (\nabla T)(1, 1, 2) \cdot \vec{u} \\
&= \begin{pmatrix}
8 \\ 4 \\ 2
\end{pmatrix} \cdot \begin{pmatrix}
-\frac{1}{\sqrt{6}} \\ -\frac{1}{\sqrt{6}} \\ -\frac{2}{\sqrt{6}}
\end{pmatrix} \\
&= -\frac{8}{\sqrt{6}} - \frac{4}{\sqrt{6}} - \frac{4}{\sqrt{6}} \\
&= -\frac{16}{\sqrt{6}} \approx -6.53 \text{ °F/m}
\end{align*}$$

# Example. Normal Vectors to a Surface

$$f(x, y) = xy^{2} - \frac{1}{2} x^{2}y^{2}$$

> [!question]+ Is the vector $\nabla f(x, y)$ normal to the graph of $f$? Explain
> No
> Interpret $(\nabla f)(x, y)$ as $\begin{pmatrix}f_{x}(x, y) \\ f_{y}(x, y) \\ 0 \end{pmatrix}$
> $$f_{x}(a, b)(xa) + f_{y}(a, b)(y-b) + f(a, b) = z$$
>
> - The graph of $f$ is given by $z = f(x, y) \iff 0 = f(x, y) = z$, which is the level surface $g(x, y, z) = 0$ for $g(x,y,z) = f(x,y) - z$.
> - So the tangent plane to the graph at $(x, y, z)$ has normal vector $$(\nabla g)(x, y, z) = \begin{pmatrix} f_{x}(x, y) \\ f_{y}(x, y) \\ -1 \end{pmatrix}$$
> - However, $\nabla f(x,y)$ is perpendicular to the *level curves*/*contours* of $f$ through $(x, y)$

> [!question]+ Find two vectors: one that is normal to the graph of $f$ at $(1, 0)$ and one that is normal to the graph of $f$ at $(1, 1)$
> First, let’s find $f_x$ and $f_y$:
> $$f_x = y^2 - xy^2 \quad \text{and} \quad f_y = 2xy - x^2y$$
>
> At $(1,0)$:
> $$\begin{pmatrix}
> f_{x}(1, 0) \\
> f_{y}(1, 0) \\
> -1
> \end{pmatrix} = \begin{pmatrix}
> 0 \\
> 0 \\
> -1
> \end{pmatrix}$$
>
> At $(1,1)$:
> $$\begin{pmatrix}
> f_{x}(1, 1) \\
> f_{y}(1, 1) \\
> -1
> \end{pmatrix} = \begin{pmatrix}
> 0 \\
> 1 \\
> -1
> \end{pmatrix}$$
