---
dg-publish: false
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 18
Module:
  - "[[6 - Line Integrals and the Fundamental Theorem of Calculus]]"
Lecture:
  - "49"
  - "50"
  - "51"
Chapter: "18.2"
Slides/Notes: 
Date: 2025-02-10
Date created: Mon., Feb. 17, 2025, 7:05:21 pm
Date modified: Mon., Feb. 24, 2025, 2:12:18 pm
---

# Computing Line Integrals over Parameterized Curves

> [!goal] Use parameterization of a curve to convert a line integral into an ordinary one-variable integral

## Using a Parameterization to Evaluate a Line Integral

Recall the definition of the line integral:
$$
\int_{C} \vec{F} \cdot d\vec{r} = \lim_{ \| \Delta \vec{r}_{i} \| \to 0 } \sum \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i},
$$
where $\vec{r}_{i}$ are the *position vectors* of the points subdividing the curve into short pieces.

- Suppose we have a smooth parameterization, $\vec{r}(t)$, of $C$ for $a \leq t \leq b$
    - So that $\vec{r}(a)$ is the position vector of the starting point of the curve, and
    - $\vec{r}(b)$ is the position vector of the end
- & Then we can divide $C$ into $n$ pieces by dividing the interval $a \leq t \leq b$ into $n$ pieces
    - Each of size $\Delta t = (b-a) / n$

> [!goal]+ At each point $\vec{r}_{i} = \vec{r}(t_{i})$, we want to compute
> $$
> \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}
> $$

![](https://i.imgur.com/kZRaPVK.png)
*Figure 18.19: Subdivision of the interval $a \leq t \leq b$*

![](https://i.imgur.com/JrbpjTs.png)
*Figure 18.20: Corresponding subdivision of the parameterized path $C$*

- & Since $t_{i + 1} = t_{i} + \Delta t$, the displacement vectors $\Delta \vec{r}_{i}$ are given by: $$\begin{align} \Delta \vec{r}_{i} & = \vec{r}(t_{i + 1}) - \vec{r}(t_{i}) \\ & = \vec{r}(t_{i} + \Delta t) - \vec{r}(t_{i}) \\ & = \frac{\vec{r}(t_{i} + \Delta t) - \vec{r}(t_{i})}{\Delta t} \cdot \Delta t \\ & \approx \vec{r'}(t_{i}) \Delta t \end{align}$$
    - Use the facts that $\Delta t$ is small and that $\vec{r}(t)$ is differentiable to obtain the last approximation

> [!obs]+ Notice that
> $$
> \begin{align}
> \int_{C} \vec{F} \cdot d\vec{r} & \approx \sum \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i} \\
>  & = \sum \vec{F}(\vec{r}(t_{i})) \cdot \Delta \vec{r}_{i} \\
>  & \approx \sum \vec{F}\left(\vec{r}(t_{i})\right) \cdot \vec{r'}(t_{i})\Delta t
> \end{align}
> $$

- $ $\vec{F} \left( \vec{r}‘(t) \right) \cdot \vec{r}’(t_{i})$ is the value at $t_{i}$ of a *one-variable function of $t$*
    - Last sum is a one-variable Riemann sum
- & In the limit as $\Delta t \to 0$, we get a definite integral $$\lim_{ \Delta t \to 0 } \sum \vec{F}\left( \vec{r}(t_{i}) \right) \cdot \vec{r}'(t_{i})\Delta t = \int_{a}^{b} \vec{F}\left( \vec{r}(t) \right) \cdot \vec{r}'(t) \, dt $$

> [!thm]+ Using a parameterization to evaluate a line integral
> If $\vec{r}(t)$, for $a \leq t \leq b$, is a smooth parameterization of an oriented curve $C$ and $\vec{F}$ is a vector field which is continuous on $C$, then
> $$
> \int_{C} \vec{F} \cdot d\vec{r} = \int_{a}^{b} \vec{F}\left( \vec{r}(t) \right) \cdot \vec{r}'(t) \, dt 
> $$

- In words:
    - To compute the line integral of $\vec{F}$ over $C$:
        - Take the ==dot product of $\vec{F}$ evaluated on $C$ with velocity vector==, $\vec{r}'(t)$, of the parameterization of $C$
        - Then integrate along the curve
- % Even though we assumed that $C$ is smooth, we can use the same formula to compute line integrals over curves which are *piecewise smooth*
    - e.g., Boundary of a rectangle
    - & If $C$ is piecewise smooth, we apply the formula to each one of the smooth pieces and add the results by applying property 4 from [[The Idea of a Line Integral#Properties of Line Integrals|Properties of Line Integrals]]
    - ![[The Idea of a Line Integral#^ee3617]]

### Examples

> [!example]+ Example 1
> Compute $\int_{C} \vec{F} \cdot d\vec{r}$ where $\vec{F} = (x + y)\vec{i} + y\vec{j}$ and $C$ is the quarter unit circle, oriented counter clockwise as shown in Figure 18.21
>
> ![](https://i.imgur.com/tKFoWwV.png)
> *Figure 18.21: The vector field $\vec{F} = (x + y)\vec{i} + y\vec{j}$ and the quarter circle $C$*
>
> > [!check]- Solution
> > - Most of vectors in $\vec{F}$ along $C$ generally point in a direction *opposite* to the orientation of $C$
> >     - $\implies$ Expect our answer to be negative
> > - First step is to ==parameterize $C$==
> >     - $$\vec{r}(t) = x(t)\vec{i} + y(t)\vec{j} = \cos t \vec{i} + \sin t \vec{j}, \qquad 0 \leq t \leq \frac{\pi}{2}$$
> > - Substitute parameterization into $\vec{F}$
> >     - $\vec{F}\left( x(t), y(t) \right) = \left( \cos t + \sin t \right) \vec{i} + \sin t \vec{j}$
> > - Compute velocity vector for $\vec{r}'(t)$
> >     - $\vec{r}'(t) = x'(t) \vec{i} + y'(t) \vec{j} = -\sin t \vec{i} + \cos t\vec{j}$
> > - Then,
> > $$
> > \begin{align}
> > \int_{C} \vec{F} \cdot d\vec{r} & = \int_{0}^{\pi/2} \left( \left( \cos t + \sin t \right) \vec{i} + \sin t\vec{j} \right) \cdot \left( -\sin t\vec{i} + \cos t\vec{j} \right)  \, dt  \\
> >  & = \int_{0}^{\pi/2} \left( -\cos t \sin t -\sin ^{2}t + \sin t\cos t \right)  \, dt \\
> >  & = \int_{0}^{\pi/2} -\sin ^{2}t \, dt  \\
> >  & =-\frac{\pi}{4} \\
> >  & \approx -0.7854
> > \end{align}
> > $$

> [!example]+ Example 2
> Consider the vector field $\vec{F} = x\vec{i} + y\vec{j}$.
> 1. Suppose $C_{1}$ is the line segment joining $(1, 0)$ to $(0, 2)$ and $C_{2}$ is part of a parabola with its vertex at $(0, 2)$, joining the same points in the same order.
> ![](https://i.imgur.com/sMmnQd2.png)
> Verify that $$\int_{C_{1}} \vec{F} \cdot d\vec{r} = \int_{C_{2}} \vec{F} \cdot d\vec{r}$$
> 2. If $C$ is the triangle shown below, show that $\int_{C} \vec{F} \cdot d\vec{r} = 0$
> ![|180x166](https://i.imgur.com/2EeftfL.png)
>
> > [!check]- Solution
> > ![](https://i.imgur.com/0RbZvrU.png)

> [!example]+ Example 3
> Let $C$ be the closed curve consisting of the upper half-circle of radius 1 and the line forming its diameter along the $x$-axis, oriented counterclockwise. See Figure 18.24.
>
> ![](https://i.imgur.com/cqIKzuf.png)
> *Figure 18.24: The curve $C = C_{1} + C_{2}$*
>
> Find $\int_{C} \vec{F} \cdot d\vec{r}$ where $\vec{F}(x, y) = -y\vec{i} + x\vec{j}$
>
> > [!check]- Solution
> > - Write $C = C_{1} + C_{2}$, where
> >     - $C_{1}$ is the half-circle
> >     - $C_{2}$ is the line
> > - Compute $\int_{C_{1}} \vec{F} \cdot d\vec{r}$ and $\int_{C_{2}} \vec{F} \cdot d\vec{r}$ separately
> > - Parameterize $C_{1}$ by:
> >     - & $\vec{r}(t) = \cos t \vec{i} + \sin t \vec{j}$ with $0 \leq t \leq \pi$
> >
> > Then,
> > $$
> > \begin{align}
> > \int_{C_{1}} \vec{F} \cdot d\vec{r} & = \int_{0}^{\pi} (-\sin t \vec{i} + \cos t \vec{j}) \cdot (-\sin t \vec{i} + \cos t\vec{j}) \, dt  \\
> >  & = \int_{0}^{\pi} (\sin ^{2}t + \cos ^{2}t) \, dt  \\
> >  & = \int_{0}^{\pi} 1 \, dt  \\
> >  & = \pi
> > \end{align}
> > $$
> >
> > - $ For $C_{2}$, we have $\int_{C_{2}} \vec{F} \cdot d\vec{r} = 0$
> >     - Vector field $\vec{F}$ has no $\vec{i}$ component along the $x$-axis (where $y = 0$)
> >     - $\implies$ Perpendicular to $C_{2}$ at all points
> >
> > Finally,
> > $$
> > \int_{C} F \cdot d\vec{r} = \int_{C_{1}} \vec{F} \cdot d\vec{r} + \int_{C_{2}} \vec{F} \cdot d\vec{r} = \pi + 0 = \pi
> > $$

> [!example]+ Example 4: Computation of a line integral over a path in 3-space
> A particle travels along helix $C$ given by $\vec{r}(t) = \cos t \vec{i} + \sin t\vec{j} + 2tk$ and is subject to a force $\vec{F} = x \vec{i} + z\vec{j} - xy\vec{k}$.
> Find the total work done on the particle by the force for $0 \leq t \leq 3\pi$.
>
> > [!check]- Solution
> > $$
> > \begin{align}
> > \text{Work done} = \int_{C} \vec{F} \cdot d\vec{r} & = \int_{0}^{3\pi} \vec{F}\left( \vec{r}(t) \right) \cdot \vec{r'}(t) \, dt  \\
> >  & = \int_{0}^{3\pi} \left( \cos t \vec{i} + 2t\vec{j} - \cos t \sin t \vec{k} \right) \cdot \left( -\sin t\vec{i} + \cos tj + 2\vec{k} \right)  \, dt  \\
> >  & = \int_{0}^{3\pi} \left( -\cos t \sin t + 2t \cos t - 2\cos t \sin t \right) \, dt  \\
> >  & = \int_{0}^{3\pi} (-3\cos t\sin t + 2t\cos t) \, dt  \\
> >  & = -4
> > \end{align}
> > $$

## The Differential Notation $\int_{C} P\,dx + Q\,dy + R\,dz$x

There is an alternative notation for line integrals that is often useful.

> [!def]+ Differential notation
> For the vector field $\vec{F} = P(x, y, z)\vec{i} + Q(x, y, z)\vec{j} + R(x, y, z)\vec{k}$ and an oriented curve $C$, we have
> $$
> \int_{C} \vec{F} \cdot d\vec{r} = \int_{C} P(x, y, z)\,dx + Q(x, y, z)\,dy + R(x, y, z)\,dz
> $$

- Write $d\vec{r} = dx\vec{i} + dy\vec{j} + dz\vec{k}$

> [!tip]+ Line integrals can be expressed either using *vectors* or *differentials*
> - If independent variables are *distances*:
>     - & Visualizing a line integral in terms of ==dot product== can be useful
> - However, if independent variables are *temperature* and *volume*, then the dot product does not have any physical meaning
>     - & ==Differentials== are more natural

> [!example]+ Example 5
> Evaluate $\int_{C} xydx - y^{2}dy$ where $C$ is the line segment from $(0, 0)$ to $(2, 6)$
>
> > [!success]- Solution
> > We parameterize $C$ by $x = t, \; y = 3t, \; 0 \leq t \leq 2$.
> > Thus, $dx = dt, \; dy = 3dt$, so
> > $$\int_{C} xydx - y^{2}dy = \int_{0}^{2} t(3t)\,dt - (3t)^{2}(3dt) = \int_{0}^{2} (-24t^{2}) \, dt = -64 $$

## Independence of Parameterization

- There are many different ways of parameterizing a given oriented curve
- ? What happens to the value of a given line integral if you choose another parameterization?
    - Choice of parameterization makes no difference
- Initially defined the line integral without reference to any particular parameterization
    - This is exactly what we expect
- & Value of a line integral is **independent** of the parameterization of the path

![](https://i.imgur.com/jKIl7vh.png)
