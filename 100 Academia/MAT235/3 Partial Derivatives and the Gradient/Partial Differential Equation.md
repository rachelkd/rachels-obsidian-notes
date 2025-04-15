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
Date created: Wed., Nov. 13, 2024, 10:59:54 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Partial Differential Equation

> [!def]+ Partial Differential Equation
> An equation that relates some partials of a multivariable function

- e.g., For $u(x, t)$, we have $u_{t} = u_{xx}$
    - Solution is a function $u(x, t)$

> [!thm]+ The Heat Equation
> $$u(x, t) = e^{-t}\sin x$$
> Finding some partial derivatives, we get
> $$\begin{align*}
> u_{t} &= -e^{-t}\sin x \tag{1} \\
> u_{x} &= e^{-t}\cos x \tag {2} \\
> u_{xx} &= -e^{-t} \sin x \tag {3}
> \end{align*}$$
> Then, $u_{t} = u_{x x}$ is a solution for the equation
