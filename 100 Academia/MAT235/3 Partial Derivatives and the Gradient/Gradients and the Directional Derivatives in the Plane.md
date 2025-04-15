---
dg-publish: true
tags: [lecture, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 7
Lecture:
  - 21,22
Chapter: 14.4
Slides/Notes: 
Date: 2024-10-23
aliases: []
Date created: Sun., Nov. 3, 2024, 11:33:30 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Definition of Directional Derivative

> [!def]+ Directional derivative of $f$ at $(a, b)$ in the direction of a unit vector $\vec{u}$
> If $\vec{u} = u_{1}\vec{i} + u_{2}\vec{j}$ is a unit vector, we defined the directional derivative, $f_{\vec{u}}$ by
> $$\begin{align*} f_{\vec{u}}(a, b) &= \text{Rate of change of } f \text{ in the direction of } \vec{u} \text{ at } (a, b) \\ &= \lim_{ h \to 0 } \frac{f(a + hu_{1}, b + hu_{2}) - f(a, b)}{h} \end{align*}$$
> provided the limit exists. Note that the directional derivative is a *scalar*.

![|500](https://i.imgur.com/kAQdrWi.png)

- Notice that if $\vec{u} = \vec{i} \implies u_{1} = 1, u_{2} = 0$, then the directional derivative is $f_{x}$
    - $$f_{\vec{i}}(a, b) = \lim_{ h \to 0 } \frac{f(a + h, b) - f(a, b)}{h} = f_{x}(a, b)$$
- Similarly, if $\vec{u} = \vec{j}$, then the directional derivative $f_{\vec{u}}=f_{y}$

> [!warning]+ What if we do not have a unit vector?
> If $\vec{v}$ is not a unit vector and $\vec{v} \neq 0$, we ==construct a unit vector== $\vec{u} = \vec{v} / ||\vec{v}||$ in the same direction as $\vec{v}$ and define the rate of change of $f$ in the direction of $\vec{v}$ as $f_{\vec{u}}$

## Example. Calculate Directional Derivative

> [!example]+ Calculate the directional derivative of $f(x, y) = x^{2} + y^{2}$ at $(1, 0)$ in the direction of the vector $\vec{i} + \vec{j}$
>
> 1. Find the **unit vector** in the same direction as the vector $\vec{i} + \vec{j}$
>     - Since this vector has magnitude $\sqrt{ 2 }$, $$\vec{u} = \frac{1}{\sqrt{ 2 }}(\vec{i} + \vec{j}) = \frac{1}{\sqrt{ 2 }}\vec{i} + \frac{1}{\sqrt{ 2 }}\vec{j}$$
> 2. Calculate directional derivative
>    $$\begin{align*}
>    f_{\vec{u}}(1, 0) &= \lim_{ h \to 0 } \frac{f\left( 1 + \frac{h}{\sqrt{ 2 }}, \frac{h}{\sqrt{ 2 }} \right) - f(1, 0)}{h} \\
>    &= \lim_{ h \to 0 }  \frac{\left( 1 + \frac{h}{\sqrt{ 2 }} \right)^{2} + \left( \frac{h}{\sqrt{ 2 }} \right)^{2} - 1}{h} \\
>    &= \lim_{ h \to 0 } \frac{\sqrt{ 2h } + h^{2}}{h} \\
>    &= \lim_{ h \to 0 } \sqrt{ 2 }+ h \\
>    &= \sqrt{ 2 }
>    \end{align*}$$

# Computing Directional Derivatives from Partial Derivatives

> [!thm]+ Formula for Directional Derivative Using Partial Derivatives
> For a differentiable function $f$ and a unit vector $\vec{u} = u_1\vec{i} + u_2\vec{j}$, the directional derivative can be computed using:
> $$f_{\vec{u}}(a, b) = f_x(a, b)u_1 + f_y(a, b)u_2$$

## Derivation

1. Start with the definition:
   $$f_{\vec{u}}(a, b) = \lim_{h \to 0} \frac{f(a + hu_1, b + hu_2) - f(a, b)}{h} = \lim_{ h \to 0 } \frac{\Delta f}{h}$$
2. Let $\Delta f = f(a + hu_1, b + hu_2) - f(a, b)$ be the change in $f$
    - $\Delta x = hu_1$ (change in $x$)
    - $\Delta y = hu_2$ (change in $y$)
3. Using local linearity:
   $$\Delta f \approx f_x(a, b)\Delta x + f_y(a, b)\Delta y = f_x(a, b)hu_1 + f_y(a, b)hu_2$$
4. Dividing by $h$:
   $$\frac{\Delta f}{h} \approx \frac{f_x(a, b)hu_1 + f_y(a, b)hu_2}{h} = f_x(a, b)u_1 + f_y(a, b)u_2$$
5. As $h \to 0$, this ==approximation becomes exact==, giving us our formula.

> [!obs]+ Observation.
> This formula allows us to compute directional derivatives without taking limits, using only the partial derivatives and the components of the unit vector.

## Example. Using the Formula for Directional Derivatives

> [!example]+ Calculate $f_{\vec{u}}(1,0)$ for $f(x,y) = x^2 + y^2$ with $\vec{u} = \frac{1}{\sqrt{2}}\vec{i} + \frac{1}{\sqrt{2}}\vec{j}$ using the formula
>
> > [!note]- Step 1: Find the partial derivatives
> > $$\begin{align*}
> > f_x(x,y) &= 2x \\
> > f_y(x,y) &= 2y
> > \end{align*}$$
>
> > [!note]- Step 2: Identify components at point (1,0)
> > $$\begin{align*}
> > f_x(1,0) &= 2(1) = 2 \\
> > f_y(1,0) &= 2(0) = 0 \\
> > u_1 &= \frac{1}{\sqrt{2}} \\
> > u_2 &= \frac{1}{\sqrt{2}}
> > \end{align*}$$
>
> > [!note]- Step 3: Apply the formula $f_{\vec{u}}(a,b) = f_x(a,b)u_1 + f_y(a,b)u_2$
> > $$\begin{align*}
> > f_{\vec{u}}(1,0) &= f_x(1,0)u_1 + f_y(1,0)u_2 \\
> > &= (2)\left(\frac{1}{\sqrt{2}}\right) + (0)\left(\frac{1}{\sqrt{2}}\right) \\
> > &= \sqrt{2}
> > \end{align*}$$

# The Gradient Vector

> [!obs]+ Observation
> The directional derivative $f_{\vec{u}}(a,b)$ can be written as a dot product:
> $$f_{\vec{u}}(a,b) = f_x(a,b)u_1 + f_y(a,b)u_2 = (f_x(a,b)\vec{i} + f_y(a,b)\vec{j}) \cdot (u_1\vec{i} + u_{2}\vec{j})= \begin{pmatrix} f_x(a,b) \\ f_y(a,b) \end{pmatrix} \cdot \begin{pmatrix} u_1 \\ u_2 \end{pmatrix}$$

> [!def]+ The Gradient Vector
> For a differentiable function $f$ at the point $(a,b)$, the gradient vector is defined as:
> $$\text{grad }f(a,b) = f_x(a,b)\vec{i} + f_y(a,b)\vec{j}$$

## Relationship Between Directional Derivative and Gradient

> [!important]+ The Directional Derivative and the Gradient
> If $f$ is differentiable at $(a,b)$ and $\vec{u} = u_1\vec{i} + u_2\vec{j}$ is a unit vector, then:
> $$f_{\vec{u}}(a,b) = f_x(a,b)u_1 + f_y(a,b)u_2 = \text{grad }f(a,b) \cdot \vec{u}$$

> [!note]+ Local Linear Approximation
> For small changes $\Delta\vec{r} = \Delta x\vec{i} + \Delta y\vec{j}$, the change in $f$ can be approximated using the gradient:
> $$\Delta f \approx \text{grad }f \cdot \Delta\vec{r}$$

## Example. Finding the Gradient Vector

> [!example]+ Find the gradient vector of $f(x,y) = x + e^y$ at the point $(1, 1)$
>
> > [!note]- Step 1: Calculate partial derivatives
> > $$\begin{align*}
> > f_x &= 1 \\
> > f_y &= e^y
> > \end{align*}$$
>
> > [!note]- Step 2: Write gradient vector
> > $$\text{grad }f = f_x\vec{i} + f_y\vec{j} = \vec{i} + e^y\vec{j}$$
>
> > [!note]- Step 3: Evaluate at point $(1,1)$
> > $$\text{grad }f(1,1) = \vec{i} + e\vec{j}$$

## Alternative Notation for the Gradient

> [!def]+ The Del Operator
> The gradient can be written using the del operator $\nabla$
> $$\nabla = \frac{\partial}{\partial x}\vec{i} + \frac{\partial}{\partial y}\vec{j}$$
>
> This gives us the alternative notation:
> $$\text{grad }f = \nabla f$$
>
> For a function $z = f(x,y)$, we can write:
>
> - grad $z$
> - $\nabla z$
> - grad $f$
> - $\nabla f$

## What Does the Gradient Tell Us?

> [!important]+ Relationship Between Gradient and Directional Derivative
> Given that $f_{\vec{u}} = \text{grad }f \cdot \vec{u}$, at point $(a,b)$:
> $$f_{\vec{u}} = \text{grad }f \cdot \vec{u} = \|\text{grad }f\| \|\vec{u}\| \cos \theta = \|\text{grad }f\| \cos \theta$$
> where $\theta$ is the angle between grad $f$ and $\vec{u}$

> [!note]+ Key Properties
>
> 1. **Maximum Value**: Occurs when $\cos \theta = 1 \implies \theta = 0$
>    - $\vec{u}$ points in direction of grad $f$
>    - Maximum $f_{\vec{u}} = \|\text{grad }f\|$
>
> 2. **Minimum Value**: Occurs when $\cos \theta = -1 \implies \theta = \pi$
>    - $\vec{u}$ points opposite to grad $f$
>    - Minimum $f_{\vec{u}} = -\|\text{grad }f\|$
>
> 3. **Zero Value**: Occurs when $\theta = \frac{\pi}{2}$ or $\frac{3\pi}{2}$
>    - $\vec{u}$ is perpendicular to grad $f$
>    - $f_{\vec{u}} = 0$
>
> ![|center|500](https://i.imgur.com/Obv3ulF.png)
