---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 6
Lecture:
  - "17"
Chapter: 14.2
Slides/Notes:
  - https://share.goodnotes.com/s/0hBJMHRTZ8bFNSdv4Mt4Pt
Date: 2024-10-10
aliases: []
Date created: Thu., Oct. 10, 2024, 5:22:42 pm
Date modified: Tue., Feb. 18, 2025, 4:24:51 pm
---

> [!note]+ To compute $f_{x}(x, y, z)$,
>
> - Pretend $y$ and $z$ are constant
> - Differentiate as function of 1 variable $x$
> - $f_{y}(x, y, z)$: Pretend $x,z$ are constant; differentiate as function of 1 variable $y$
> - $f_{z}(x, y, z)$: Pretend $x, y$ are constant; differentiate as function of 1 variable $z$

# Examples in Lecture

Calculate the partial derivatives of:

> [!question]- Calculate the partial derivatives of $z = x^{2}y + y^{2}x$
>
> - $\frac{ \partial z }{ \partial x } = 2xy + y^2$
> - $\frac{ \partial z }{ \partial y } = x^{2} + 2xy$

> [!question]- Calculate the partial derivatives of $f(x, y, z) = z\ln (y \cos x)$
> $$\begin{align*}
> \frac{ \partial f }{ \partial x }(x,y,z) &= z \frac{ \partial }{ \partial x } (y \cos x) \times \frac{1}{y \cos x}\\
> &= -zy \sin x \times \frac{1}{y \cos x} \\
> &= -z \tan x
> \end{align*}$$
>
> $$\begin{align*}
> f_{y}(x,y,z) &= z \frac{1}{y \cos x} \times \frac{ \partial  }{ \partial y } (y \cos x) \\
> &= \frac{z}{y}
> \end{align*}$$
>
> $$f_{z}(x,y,z) = \ln (y \cos x)$$
