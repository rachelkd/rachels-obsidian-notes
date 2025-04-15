---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 9
Lecture:
  - "23"
Chapter: "14.5"
Slides/Notes: 
Date: 2024-11-04
aliases: []
Date created: Mon., Nov. 4, 2024, 5:52:45 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

> [!goal]+ Objectives
>
> - Understand how to extend the idea of the gradient to functions of three variables.
> - Compute the directional derivative of a three-variable function.
> - Use the gradient vector to determine the equation of the tangent plane to a level surface at a given point.

# The Gradient Vector in Space

> [!def]+ The Gradient Vector in Space
> For a differentiable function $f(x,y,z)$, the gradient vector is defined as:
> $$\text{grad }f = f_x\vec{i} + f_y\vec{j} + f_z\vec{k}$$

# Directional Derivatives in Space

> [!def]+ Directional Derivative in Space
> For a differentiable function $f$ at point $(a,b,c)$ and a unit vector $\vec{u} = u_1\vec{i} + u_2\vec{j} + u_3\vec{k}$, the directional derivative is:
> $$f_{\vec{u}}(a,b,c) = f_x(a,b,c)u_1 + f_y(a,b,c)u_2 + f_z(a,b,c)u_3 = \text{grad }f(a,b,c) \cdot \vec{u}$$

## Relationship with the Gradient

> [!important]+ Gradient and Directional Derivative Relationship
> Since $f_{\vec{u}}(a,b,c) = \text{grad }f(a,b,c) \cdot \vec{u} = \|\text{grad }f(a,b,c)\| \cos \theta$, where $\theta$ is the angle between grad $f(a,b,c)$ and $\vec{u}$:
>
> - The directional derivative is largest when $\theta = 0$ (when $\vec{u}$ points in same direction as grad $f$)
> - The directional derivative is zero when $\theta = \pi/2$ (when $\vec{u}$ is perpendicular to grad $f$)

# Properties of the Gradient Vector in Space

> [!thm]+ Properties of Gradient Vector
> If $f$ is differentiable at $(a,b,c)$ and $\vec{u}$ is a unit vector, then:
>
> 1. $f_{\vec{u}}(a,b,c) = \text{grad }f(a,b,c) \cdot \vec{u}$
>
> If, in addition, grad $f(a,b,c) \neq \vec{0}$, then:
>
> 1. grad $f(a,b,c)$ is perpendicular to the level surface of $f$ at $(a,b,c)$
> 2. grad $f(a,b,c)$ points in the direction of greatest rate of increase of $f$
> 3. $\|\text{grad }f(a,b,c)\|$ is the maximum rate of change of $f$ at $(a,b,c)$

> [!example]+ Find the directional derivative of $f(x,y,z) = xy + z$ at the point $(-1,0,1)$ in the direction of $\vec{v} = 2\vec{i} + \vec{k}$
>
> > [!note]- Step 1: Find unit vector in direction of $\vec{v}$
> > The magnitude of $\vec{v}$ is:
> > $$\|\vec{v}\| = \sqrt{2^2 + 1} = \sqrt{5}$$
> > Therefore, the unit vector is:
> > $$\vec{u} = \frac{\vec{v}}{\|\vec{v}\|} = \frac{2}{\sqrt{5}}\vec{i} + 0\vec{j} + \frac{1}{\sqrt{5}}\vec{k}$$
>
> > [!note]- Step 2: Calculate partial derivatives
> > $$\begin{align*}
> > f_x(x,y,z) &= y \\
> > f_y(x,y,z) &= x \\
> > f_z(x,y,z) &= 1
> > \end{align*}$$
>
> > [!note]- Step 3: Apply the formula $f_{\vec{u}}(a,b,c) = f_x(a,b,c)u_1 + f_y(a,b,c)u_2 + f_z(a,b,c)u_3$
> > $$\begin{align*}
> > f_{\vec{u}}(-1,0,1) &= f_x(-1,0,1)u_1 + f_y(-1,0,1)u_2 + f_z(-1,0,1)u_3 \\
> > &= (0)\left(\frac{2}{\sqrt{5}}\right) + (-1)(0) + (1)\left(\frac{1}{\sqrt{5}}\right) \\
> > &= \frac{1}{\sqrt{5}}
> > \end{align*}$$

# Tangent Plane to a Level Surface

> [!def]+ Tangent Plane to a Level Surface
> If $f(x,y,z)$ is differentiable at $(a,b,c)$, then the equation of the tangent plane to the level surface of $f$ at the point $(a,b,c)$ is:
> $$f_x(a,b,c)(x-a) + f_y(a,b,c)(y-b) + f_z(a,b,c)(z-c) = 0$$

> [!note]+ Connection to Gradient
> The equation of the tangent plane can be written using the gradient:
>
> 1. The gradient vector $\nabla f(a,b,c)$ is normal to the tangent plane
> 2. The equation can be rewritten as: $\nabla f(a,b,c) \cdot \begin{pmatrix} x-a \\ y-b \\ z-c \end{pmatrix} = 0$
> 3. This shows that any vector in the tangent plane is perpendicular to the gradient

> [!important]+ Geometric Interpretation
> The vector $\begin{pmatrix} x-a \\ y-b \\ z-c \end{pmatrix}$ is a displacement vector from the point $(a,b,c)$ to any point $(x,y,z)$ in the tangent plane. The equation states that:
>
> - The gradient vector at $(a,b,c)$ is perpendicular to every such displacement vector
> - This is why the gradient is normal to the tangent plane
> - Any point $(x,y,z)$ satisfying the tangent plane equation represents a tip of a displacement vector that is perpendicular to the gradient

> [!example]+ Find the equation of the tangent plane to the sphere $x^2 + y^2 + z^2 = 14$ at the point $(1,2,3)$
> ![|center|100](https://i.imgur.com/pe69llq.png)
>
> - If you look at a normal vector to the *sphere*, it is also normal to the tangent plane at $(1, 2, 3)$
> - ? How do you find a normal vector to a surface that is not a function?
>
> > [!note]- Step 1: Write surface as a function of three variables
> > Let $w = f(x,y,z) = x^2 + y^2 + z^2 - 14$
> > Then our sphere is the level surface $f(x,y,z) = 0$
>
> > [!note]- Step 2: Calculate partial derivatives
> > $$\begin{align*}
> > f_x(x,y,z) &= 2x \\
> > f_y(x,y,z) &= 2y \\
> > f_z(x,y,z) &= 2z
> > \end{align*}$$
>
> > [!note]- Step 3: Evaluate partial derivatives at $(1,2,3)$
> > $$\begin{align*}
> > f_x(1,2,3) &= 2(1) = 2 \\
> > f_y(1,2,3) &= 2(2) = 4 \\
> > f_z(1,2,3) &= 2(3) = 6
> > \end{align*}$$
> >
> > Therefore:
> > $$\text{grad }f = \begin{pmatrix} f_x \\ f_y \\ f_z \end{pmatrix} = \begin{pmatrix} 2x \\ 2y \\ 2z \end{pmatrix}$$
> > $$\text{grad }f(1,2,3) = \begin{pmatrix} f_x(1,2,3) \\ f_y(1,2,3) \\ f_z(1,2,3) \end{pmatrix} = \begin{pmatrix} 2 \\ 4 \\ 6 \end{pmatrix}$$
> > is a normal vector to the tangent plane at $(1,2,3)$
>
> > [!note]- Step 4: Use the tangent plane equation
> > $$f_x(a,b,c)(x-a) + f_y(a,b,c)(y-b) + f_z(a,b,c)(z-c) = 0$$
> > Substituting our values:
> > $$2(x-1) + 4(y-2) + 6(z-3) = 0$$
> > $$2x - 2 + 4y - 8 + 6z - 18 = 0$$
> > $$2x + 4y + 6z = 28$$
> > $$x + 2y + 3z = 14$$
