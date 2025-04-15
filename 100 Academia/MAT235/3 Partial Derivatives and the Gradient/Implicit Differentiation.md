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
aliases: []
Date created: Mon., Nov. 11, 2024, 10:04:34 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Implicit Differentiation

> [!note] Implicit differentiation is not covered in TT2 or Final Exam

We have the equation $F(x, y, z) = 0$

Assume (implicitly) that $z = f(x, y)$

> [!question]+ How do we find $\frac{ \partial z }{ \partial x }$ and $\frac{ \partial z }{ \partial y }$?
> ![|center|400](https://i.imgur.com/EudLgGg.png)
> Take $\frac{ \partial }{ \partial x }$ with Chain Rule:
> $$\frac{ \partial F }{ \partial x } \frac{ \partial x }{ \partial x } + \frac{ \partial F }{ \partial y } \frac{ \partial y }{ \partial x } + \frac{ \partial F }{ \partial z } \frac{ \partial z }{ \partial x } = 0 $$
>
> Since $\frac{\partial x}{\partial x} = 1$ and $\frac{\partial y}{\partial x} = 0$:
> $$\frac{ \partial F }{ \partial x } + \frac{ \partial F }{ \partial z } \frac{ \partial z }{ \partial x } = 0 $$
>
> Solving for $\frac{\partial z}{\partial x}$:
> $$\frac{ \partial z }{ \partial x } = -\frac{\frac{\partial F}{\partial x}}{\frac{\partial F}{\partial z}} = - \frac{F_{x}}{F_{z}}$$
>
> Similarly, for $\frac{\partial z}{\partial y}$:
> $$\frac{ \partial z }{ \partial y } = -\frac{\frac{\partial F}{\partial y}}{\frac{\partial F}{\partial z}} = - \frac{F_{y}}{F_{z}}$$

> [!important]+ Remember
>
> - $\frac{\partial x}{\partial x} = 1$ (derivative of $x$ with respect to $x$)
> - $\frac{\partial y}{\partial x} = 0$ ($y$ is treated as a constant when taking partial derivative with respect to $x$)
> - $\frac{\partial x}{\partial y} = 0$ and $\frac{\partial y}{\partial y} = 1$ (for the second equation)
