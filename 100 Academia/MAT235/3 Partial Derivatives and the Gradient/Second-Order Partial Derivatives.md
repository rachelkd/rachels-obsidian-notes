---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 10
Lecture:
  - "26"
Chapter: "14.7"
Slides/Notes:
  - https://share.goodnotes.com/s/iMkt2ETtjdRbUUaYIW8UOA
Date: 2024-11-11
aliases: [Clairaut's Theorem, Equality of Mixed Partials]
Date created: Mon., Nov. 11, 2024, 10:04:34 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Second-Order Partial Derivatives

> [!def]+ Second-Order Partial Derivatives
> Second-order partial derivatives are obtained by differentiating a function’s [[Partial Derivatives|first-order partial derivatives]].
>
> - First-order partial derivatives ($f_x$ and $f_y$) are themselves functions
> - These can be differentiated again to get second-order derivatives
> - A function $z = f(x,y)$ has ==four== second-order partial derivatives
>
> > [!info]+ Second-Order Partial Derivatives of $z = f(x, y)$
> > For a function $z = f(x, y)$,
> >
> > 1. $\frac{\partial^2z}{\partial x^2} = f_{xx} = (f_x)_x$ : Derivative with respect to $x$ twice
> > 2. $\frac{\partial^2z}{\partial x\partial y} = f_{xy} = (f_x)_y$ : Derivative with respect to $x$ then $y$
> > 3. $\frac{\partial^2z}{\partial y\partial x} = f_{yx} = (f_y)_x$ : Derivative with respect to $y$ then $x$
> > 4. $\frac{\partial^2z}{\partial y^2} = f_{yy} = (f_y)_y$ : Derivative with respect to $y$ twice

> [!note]+ Notation
>
> - Common to omit parentheses: write $f_{xy}$ instead of $(f_x)_y$
> - Notation $\frac{\partial^2z}{\partial x\partial y}$ can be written instead of $\frac{\partial}{\partial y}\left(\frac{\partial z}{\partial x}\right)$

> [!example]+ Given $f(x,y) = x^2y + 3y^2e^x$, find all second-order partial derivatives.
>
> 1. First find the first-order partial derivatives:
>    - $f_x(x,y) = 2xy + 3y^2e^x$
>    - $f_y(x,y) = x^2 + 6ye^x$
>
> 2. Then find all four second-order partial derivatives:
>    - $f_{xx}(x,y) = 2y + 3y^2e^x$ (differentiate $f_x$ with respect to $x$)
>    - $f_{xy}(x,y) = 2x + 6ye^x$ (differentiate $f_x$ with respect to $y$)
>    - $f_{yx}(x,y) = 2x + 6ye^x$ (differentiate $f_y$ with respect to $x$)
>    - $f_{yy}(x,y) = 6e^x$ (differentiate $f_y$ with respect to y)

- Notice that $f_{xy} = f_{yx}$

> [!thm]+ Equality of Mixed Partials
> If $f_{xy}$ and $f_{yx}$ are continuous at point $(a,b)$, an interior point of their domain, then:
> $$f_{xy}(a,b) = f_{yx}(a,b)$$

> [!question]+ Does there exist a function such that:
> $$f_x = 3y^2 + x \text{ and } f_y = 6xy + 3x$$
>
> 1. Find $f_{xy}$:
>    - $f_{xy} = \frac{\partial}{\partial y}(3y^2 + x) = 6y$
>
> 2. Find $f_{yx}$:
>    - $f_{yx} = \frac{\partial}{\partial x}(6xy + 3x) = 6y + 3$
>
> 3. Analysis:
>    - If $f$ exists, then $f_{xy} \neq f_{yx}$ (since $6y \neq 6y + 3$)
>    - But by **Equality of Mixed Partials**, if $f_{xy}$ and $f_{yx}$ are both continuous, then $f_{xy} = f_{yx}$
>    - This is a contradiction!
>
> Therefore, no such function can exist.

- Equality of Mixed Partials theorem can be used to prove that certain combinations of partial derivatives cannot come from a valid function.

> [!example]+ Estimating Mixed Partial Derivatives from a Table
> The values of $f(x,y)$ are given in the following table:
>
> | $x\backslash y$ | 2 | 4 | 6 | 8 |
> |-----------------|---|---|---|---|
> | 2               | 2 | 4 | 6 | 8 |
> | 4               | 3 | 6 | 9 | 12|
> | 6               | 4 | 8 | 12| 16|
> | 8               | 5 | 10| 15| 20|
>
> Using the table, estimate the values of $f_{xy}(4,4)$ and $f_{yx}(4,4)$
>
> **Solution:**
>
> 1. For $f_{xy}(4,4)$, we use:
> $$f_{xy}(4,4) \approx \frac{f_x(4,6) - f_x(4,2)}{4} \text{ where } \Delta y = 4$$
>
> First find $f_x(4,6)$:
> $$f_x(4,6) \approx \frac{f(6,6) - f(2,6)}{4} = \frac{12-6}{4} = \frac{3}{2}$$
>
> Then $f_x(4,2) \approx \frac{1}{2}$
>
> Therefore:
> $$f_{xy}(4,4) \approx \frac{\frac{3}{2} - \frac{1}{2}}{4} = \frac{1}{4}$$
>
> 1. Similarly, for $f_{yx}(4,4)$:
> $$f_{yx}(4,4) \approx \frac{1}{4}$$

- See [[Partial Derivatives#^c9139c|definition of partial derivatives]]
