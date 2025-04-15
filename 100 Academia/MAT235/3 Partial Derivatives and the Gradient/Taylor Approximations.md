---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 10
Lecture:
  - "27"
Chapter: "14.7"
Slides/Notes:
  - https://share.goodnotes.com/s/QbzzwLbyl19Sv6Gxew7Qze
Date: 2024-11-13
aliases: []
Date created: Mon., Nov. 11, 2024, 10:38:24 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Taylor Approximations

- **Taylor approximations** use derivatives to construct polynomial approximations of functions
- We can improve the accuracy of [[Local Linearity and the Differential|local linearization]] by including higher-order terms

## Single Variable Case

> [!info]+ Linear Approximation (Degree-1)
> For a function of one variable, the best linear approximation near $x=a$ is:
> $$f(x) \approx f(a) + f'(a)(x-a)$$
> This is also known as the ==local linearization==.

> [!info]+ Quadratic Approximation (Degree-2)
> A better approximation is given by the degree-2 Taylor polynomial:
> $$f(x) \approx f(a) + f'(a)(x-a) + \frac{f''(a)}{2}(x-a)^2$$

## Two Variable Case

> [!def]+ Local linearization for $(x, y)$ near $(a, b)$
> $$f(x,y) \approx L(x,y) = f(a,b) + f_x(a,b)(x-a) + f_y(a,b)(y-b)$$

![](https://i.imgur.com/sbUBtwQ.png)

- In the case $(a, b) = (0, 0)$, we have:

> [!def]+ Taylor Polynomial of Degree 1 Approximating $f(x, y)$ for $(x, y)$ Near $(0, 0)$
> If $f$ has ==continuous first-order derivatives==, then
> $$f(x,y) \approx L(x,y) = f(0,0) + f_x(0,0)x + f_y(0,0)y$$

> [!def]+ Quadratic Approximation (Taylor Polynomial of Degree-2) Near $(0, 0)$
> If $f$ has ==continuous second-order partial derivatives==:
> $$\begin{align*} f(x,y) &\approx Q(x,y) \\ &= f(0,0) + f_x(0,0)x + f_y(0,0)y + \frac{f_{xx}(0,0)}{2}x^{2} + f_{xy}(0,0)xy + \frac{f_{yy}(0,0)}{2}y^{2} \end{align*}$$

> [!def]+ Quadratic Approximation (Taylor Polynomial of Degree-2) Near $(a, b)$
> If $f$ has ==continuous second-order partial derivatives==:
> $$\begin{align*} f(x,y) &\approx Q(x,y) \\ &= L(x, y) + \frac{f_{xx}(a, b)}{2}(x-a)^{2} + f_{xy}(a, b)(x-a)(y-b) + \frac{f_{yy}(a, b)}{2}(y-b)^{2} \\ &= f(a,b) + f_x(a,b)(x-a) + f_y(a,b)(y-b) + \frac{f_{xx}(a, b)}{2}(x-a)^{2} + f_{xy}(a, b)(x-a)(y-b) + \frac{f_{yy}(a, b)}{2}(y-b)^{2} \end{align*}$$

Note: All the partials up to order-2 agree at $(a, b)$

$$\begin{align*}
f(a, b) &= Q(a, b) \\
f_{x}(a, b) &= Q_{x}(a, b) && f_{y}(a, b) = Q_{y}(a, b) \\
f_{xx}(a, b) &= Q_{xx}(a, b) && f_{xy}(a, b) = Q_{xy}(a, b) && f_{yy}(a, b) = Q_{yy}(a, b)
\end{align*}$$

## Example. Linear and Quadratic Approximation

Let $f(x, y) = (xy)^{-1}$ and $(a, b) = (1, 2)$.

> [!example]+ Find $L(x, y)$ and $Q(x, y)$. Use these to approximate $f(1.1, 1.9)$.
> 1. Find first-order partials
>    $$f_{x}(x, y) = -x^{-2}y^{-1} \quad f_{y}(x, y) = -x^{-1}y^{-2}$$
>    So, $f_{x}(1, 2) = -\frac{1}{2}$ and $f_{y}(1,2) = -\frac{1}{4}$
> 2. Substitute in $L(x, y)$
>    $$\begin{align*}
>    L(x, y) &= f(1, 2) + f_{x}(1,2)(x - 1) + f_{y}(1,2)(y-2) \\
>    &= \frac{1}{2} - \frac{1}{2}(x-1) - \frac{1}{4}(y-2)
>    \end{align*}$$
> 3. Find second-order partials
>    $$\begin{align*}
>    f_{xx}(x, y) &= 2x^{-3}y^{-1} \\
>    f_{xy}(x, y) = f_{yx}(x, y) &= x^{-2}y^{-2} \\
>    f_{yy}(x, y) &= 2x^{-1}y^{-3}
>    \end{align*}$$
>    At $(1,2)$:
>    $$f_{xx}(1,2) = 1, \quad f_{xy}(1,2) = \frac{1}{4}, \quad f_{yy}(1,2) = \frac{1}{4}$$
> 4. Form quadratic approximation $Q(x,y)$
>    $$\begin{align*}
>    Q(x,y) &= L(x,y) + \frac{f_{xx}(1,2)}{2}(x-1)^2 + f_{xy}(1,2)(x-1)(y-2) + \frac{f_{yy}(1,2)}{2}(y-2)^2 \\
>    &= \frac{1}{2} - \frac{1}{2}(x-1) - \frac{1}{4}(y-2) + \frac{1}{2}(x-1)^2 + \frac{1}{4}(x-1)(y-2) + \frac{1}{8}(y-2)^2
>    \end{align*}$$
> 5. Approximate $f(1.1, 1.9)$
>     - Using linear approximation:
>      $$\begin{align*}
>      L(1.1, 1.9) &= \frac{1}{2} - \frac{1}{2}(0.1) - \frac{1}{4}(-0.1) \\
>      &= 0.5 - 0.05 + 0.025 = 0.475
>      \end{align*}$$
>     - Using quadratic approximation:
>      $$\begin{align*}
>      Q(1.1, 1.9) &= 0.475 + \frac{1}{2}(0.1)^2 + \frac{1}{4}(0.1)(-0.1) + \frac{1}{8}(-0.1)^2 \\
>      &= 0.475 + 0.005 - 0.0025 + 0.00125 \\
>      &= 0.47875
>      \end{align*}$$
>     - Actual value: $f(1.1, 1.9) = \frac{1}{1.1 \cdot 1.9} \approx 0.47847$
>
>
>

The quadratic approximation gives a more accurate result than the linear approximation.

> [!example]+ Approximate$f(x,y) = x^2y$ at $(0.9, 0.2)$
> **Given:**
> - Function: $f(x,y) = x^2y$
> - Point to estimate: $(0.9, 0.2)$
>
> **Steps:**
>
> Choose $(a,b) = (1,0)$.
> 1. Find partial derivatives:
>    - First order:
>      - $f_x(x,y) = 2xy$
>      - $f_y(x,y) = x^2$
>    - Second order:  
>      - $f_{xx}(x,y) = 2y$
>      - $f_{xy}(x,y) = 2x$
>      - $f_{yy}(x,y) = 0$
>
> 2. Linear approximation:
>    $$L(x,y) = f(1,0) + f_x(1,0)(x-1) + f_y(1,0)y = y$$
>
> 3. Form quadratic approximation:
>    $$\begin{align*}
>    Q(x,y) &= L(x,y) + \overbrace{\frac{f_{xx}(1,0)}{2}}^{0/2}(x-1)^2 + \overbrace{f_{xy}(1,0)}^{2}(x-1)y + \overbrace{\frac{f_{yy}(1,0)}{2}}^{0/2}y^2 \\
>    &= y + 2(x-1)y
>    \end{align*}$$
>
> 4. Evaluate approximation:
>    $$Q(0.9, 0.2) = 0.2 + 2(-0.1)(0.2) = 0.16$$
>
> > [!obs]+ Accuracy Check
> > - Approximation: 0.16
> > - Actual value: $(0.9)^2(0.2) = 0.162$

- Choice of approximation point $(1,0)$ was strategic:
    - Close to target point $(0.9, 0.2)$
    - Makes calculations simpler

Related Topics:

- [[Partial Derivatives]]
- [[Local Linearity and the Differential]]
- [[Second-Order Partial Derivatives]]
