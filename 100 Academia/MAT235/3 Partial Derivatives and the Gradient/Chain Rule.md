---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[3 - Partial Derivatives and the Gradient]]"
Week: 9
Lecture:
  - "25"
Chapter: "14.6"
Slides/Notes: 
Date: 2024-11-07
aliases: []
Date created: Mon., Nov. 4, 2024, 7:03:11 pm
Date modified: Thu., Jan. 9, 2025, 7:06:03 pm
---

# Composition of Functions and Rates of Change

> [!def]+ Composite Functions
> A composite function occurs when we substitute one or more functions into another function. For a function $z = f(x,y)$:
>
> 1. If we substitute $x = g(t)$ and $y = h(t)$, then $z$ becomes a function of $t$:
> $$z = f(g(t), h(t))$$
> 2. If we substitute $x = g(u,v)$ and $y = h(u,v)$, then $z$ becomes a function of $u$ and $v$:
> $$z = f(g(u,v), h(u,v))$$

> [!example]+ Corn Production and Global Warming
> Let’s analyze how corn production changes with respect to time due to global warming effects on rainfall and temperature.
>
> **Given:**
>
> - Corn production $C = f(R, T)$ depends on:
>     - Annual rainfall $(R)$
>     - Average temperature $(T)$
> - Current partial derivatives:
>     - $f_R = 3.3$ (rate of change with respect to rainfall)
>     - $f_T = -5$ (rate of change with respect to temperature)
> - Global warming model predicts:
>     - $\frac{dR}{dt} = -0.2$ cm/year (rainfall decreasing)
>     - $\frac{dT}{dt} = 0.1$ °C/year (temperature increasing)
>
> > [!note]- Step 1: Apply Chain Rule
> > By [[Local Linearity and the Differential|local linearity]], changes in $R$ and $T$ cause a change in $C$:
> > $$\Delta C \approx f_R\Delta R + f_T\Delta T = 3.3\Delta R - 5\Delta T$$
>
> > [!note]- Step 2: Express changes in terms of time
> > From the global warming model:
> > $$\Delta R \approx -0.2\Delta t \text{ and } \Delta T \approx 0.1\Delta t$$
>
> > [!note]- Step 3: Substitute to find rate of change
> > $$\begin{align*}
> > \Delta C &\approx 3.3(-0.2\Delta t) - 5(0.1\Delta t) \\
> > &= -0.66\Delta t - 0.5\Delta t \\
> > &= -1.16\Delta t
> > \end{align*}$$
>
> > [!note]- Step 4: Conclude
> > Therefore:
> > $$\frac{\Delta C}{\Delta t}= \frac{dC}{dt} \approx -1.16$$
>
> This means corn production is decreasing at a rate of approximately 1.16 units per year under the given global warming model.

# The Chain Rule for Single Parameter Functions

> [!thm]+ Chain Rule Theorem
> If $f$, $g$, and $h$ are differentiable and if:
>
> - $z = f(x,y)$
> - $x = g(t)$
> - $y = h(t)$
>
> Then:
> $$\frac{dz}{dt} = \frac{\partial z}{\partial x}\frac{dx}{dt} + \frac{\partial z}{\partial y}\frac{dy}{dt}$$

> [!note]+ Derivation Using Local Linearization
> We can derive this by considering small changes:
>
> 1. For changes in $x$ and $y$ with respect to $t$:
> $$\Delta x \approx \frac{dx}{dt}\Delta t \text{ and } \Delta y \approx \frac{dy}{dt}\Delta t$$
>
> 2. The local linearization for $z$ gives:
> $$\Delta z \approx \frac{\partial z}{\partial x}\Delta x + \frac{\partial z}{\partial y}\Delta y$$
>
> 3. Substituting the expressions from step 1:
> $$\Delta z \approx \frac{\partial z}{\partial x}\frac{dx}{dt}\Delta t + \frac{\partial z}{\partial y}\frac{dy}{dt}\Delta t$$
> $$= \left(\frac{\partial z}{\partial x}\frac{dx}{dt} + \frac{\partial z}{\partial y}\frac{dy}{dt}\right)\Delta t$$
>
> 4. Therefore:
> $$\frac{\Delta z}{\Delta t} \approx \frac{\partial z}{\partial x}\frac{dx}{dt} + \frac{\partial z}{\partial y}\frac{dy}{dt}$$
>
> 5. Taking the limit as $\Delta t \to 0$ gives us the chain rule.

## Visualizing the Chain Rule with a Dragram

![](https://i.imgur.com/W7bwAJW.png)

- Shows the *chain of dependence*:
    - $z$ depends on $x$ and $y$
    - which in turn depend on $t$
