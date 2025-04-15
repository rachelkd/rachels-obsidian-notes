---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 19
Module:
  - "[[6 - Line Integrals and the Fundamental Theorem of Calculus]]"
Lecture:
  - "52"
  - "53"
  - "54"
Chapter: "18.4"
Slides/Notes: 
Date: 2025-02-24
Date created: Mon., Feb. 24, 2025, 3:36:44 pm
Date modified: Mon., Feb. 24, 2025, 11:20:26 pm
---

# Path-Dependent Vector Fields and Green’s Theorem

Suppose we are given a vector field but are not told whether it is path-independent.

> [!question]+ How can we tell if it has a potential function?
> - i.e., If it is a gradient field?

## How to Tell if a Vector Field is Path-dependent Using Line Integrals

- One way:
    - & Find two paths with the same endpoints such that the line integrals of the vector field along the two paths have different values

> [!example]+ Example 1
> Is the vector field $\vec{G}$ shown in Figure 18.41 path-independent?
> At any point, $\vec{G}$ has magnitude equal to the distance from the origin and direction perpendicular to the line joining the point to the origin.
>
> ![](https://i.imgur.com/syL0wac.png)
>
> > [!check]- Solution
> > - Choose $P = (1, 0), Q = (0, 1)$ and two paths between them:
> >     - $C_{1} :=$ Quarter circle of radius 1
> >     - $C_{2} :=$ Formed by parts of the $x$- and $y$-axes
> > - Along $C_{1}$:
> >     - Line integral $\int_{C_{1}} \vec{G} \cdot d\vec{r} > 0$
> >         - Since $\vec{G}$ points in the direction of the curve
> > - Along $C_{2}$:
> >     - $\int_{C_{2}} \vec{G} \cdot d\vec{r} = 0$
> >         - Since $\vec{G}$ is perpendicular to $C_{2}$ everywhere
> > - & $\vec{G}$ is *not* path-independent

## Path-Dependent Fields and Circulation

- Vector field in previous example has nonzero circulation around origin

<!-- break -->
- **Simple closed curve**
    - Closed curve that does not cross itself

> [!question] What can we say about the circulation of a general path-independent vector field $\vec{F}$ around a **closed curve** $C$?

Suppose $C$ is a *simple* closed curve.

If $P, Q$ are any two points on the path, then we can think of $C$ as made up of the path $C_{1}$ followed by $-C_{2}$.

![|184x180](https://i.imgur.com/NbtcyRA.png)
*Figure 18.42: A simple closed curve $C$ broken into two pieces, $C_{1}$ and $C_{2}$*

Since $\vec{F}$ is *path-independent*, we know:
$$\int_{C_{1}} \vec{F} \cdot d\vec{r} = \int_{C_{2}} \vec{F} \cdot d\vec{r}$$

& Circulation around $C$ is zero:
$$\int_{C} \vec{F} \cdot d\vec{r} = \int_{C_{1}} \vec{F} \cdot d\vec{r} + \int_{-C_{2}} \vec{F} \cdot d\vec{r} = \int_{C_{1}} \vec{F} \cdot d\vec{r} - \int_{C_{2}} \vec{F} \cdot d\vec{r} = 0$$

If closed curve $C$ does cross itself, we break it into simple closed curves and apply the same argument to each one.

![](https://i.imgur.com/zwoucVZ.png)
*Figure 18.43: A curve $C$ which crosses itself can be broken into simple closed curves*

Then, we have:

> [!thm]+ Path-independence and line integral
> A vector field is **path-independent** if and only if $\int_{C} \vec{F} \cdot d\vec{r} = 0$ for every closed curve $C$.

Implication:

> [!tip]+ How to see if a field is *path-dependent*
> - Look for a closed path with nonzero circulation

- Example 1:
    - Vector field has nonzero circulation around a circle around the origin
    - $\implies$ Path-dependent

## How to Tell if a Vector Field is Path-dependent Algebraically: the Curl

> [!example]+ Does the vector field $\vec{F} = 2xy \vec{i} + xy \vec{j}$ have a potential function? If so, find it.
>
> > [!check]- Solution
> > Suppose $\vec{F}$ does have a potential function $f$.
> >
> > Then, $\vec{F} = \text{grad }f$, which means that
> > $$\frac{ \partial f }{ \partial x } = 2xy \quad \text{and} \quad \frac{ \partial f }{ \partial y } = xy$$
> >
> > Integrating the expression for $\frac{ \partial f }{ \partial x }$ shows that we must have $$f(x, y) = x^{2}y + C(y)$$
> > where $C(y)$ is a function of $y$.
> >
> > Differentiating this expression for $f(x, y)$ with respect to $y$ and using the fact that $\frac{ \partial f }{ \partial y } = xy$, we get
> > $$\frac{ \partial f }{ \partial y }  = x^{2} + C'(y) = xy.$$
> >
> > This implies $$C'(y) = xy - x^{2}$$
> > But $C'(y)$ is a function of $y$ alone, so there is *no* potential function for the vector field $\vec{F}$

Look at a 2-dimensional vector field $\vec{F} = F_{1}\vec{i} + F_{2}\vec{j}$.

- If $\vec{F}$ is a gradient field, then there is a potential function $f$ such that $$\vec{F} = F_{1}\vec{i} + F_{2}\vec{j} = \frac{ \partial f }{ \partial x } \vec{i} + \frac{ \partial f }{ \partial y } \vec{j}$$
$$
F_{1} = \frac{ \partial f }{ \partial x } \quad \text{and} \quad F_{2} = \frac{ \partial f }{ \partial y } 
$$
Assume that $f$ has continuous second partial derivatives. By [[Second-Order Partial Derivatives|Equality of Mixed Partials]],
$$
\frac{ \partial F_{1} }{ \partial y } = \frac{ \partial^{2}f }{ \partial y \partial x } = \frac{ \partial^{2}f }{ \partial x \partial y } = \frac{ \partial F_{2} }{ \partial x }  
$$

> [!thm]+ Curl
> If $\vec{F}(x, y) = F_{1} \vec{i} + F_{2} \vec{j}$ is a gradient vector field with continuous partial derivatives then,
> $$\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } = 0$$
> If $F(x, y) = F_{1} \vec{i} + F_{2} \vec{j}$ is an arbitrary vector field, then we defined the 2-dimensional or scalar **curl** of the vector field $\vec{F}$ to be $$\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } $$

- & If $\vec{F}$ is a gradient field, then its curl is 0
    - ? Is the converse true?
    - i.e., If the curl is 0, does $\vec{F}$ have to be a gradient field?

> [!tip]+ Curl already enables us to show that a vector field is *not* a gradient field
> By contraposition,
> - & If $\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } \neq 0$, then $\vec{F} = F_{1} \vec{i} + F_{2}\vec{j}$ is *not* a gradient vector field

> [!example]+ Show that $\vec{F} = 2xy \vec{i} + xy \vec{j}$ cannot be a gradient vector field.
>
> > [!check]- Solution
> > - $F_{1} = 2xy$
> >     - $\frac{ \partial F_{1} }{ \partial y } = 2x$
> > - $F_{2} = xy$
> >     - $\frac{ \partial F_{2} }{ \partial x } = y$
> >
> > $$\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } = 2x - y \neq 0$$
> > So, $\vec{F}$ cannot be a gradient field $\iff$ $\vec{F}$ is path-dependent.

## Green’s Theorem

- Two ways of seeing that a vector field $\vec{F}$ in the plane is path-dependent:
    1. Evaluate $\int_{C} F \cdot d\vec{r}$ for some closed curve $C$ and find that it is not zero
    2. Show that $\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } \neq 0$
- **Green’s Theorem** relates the line integral with the curl

> [!thm]+ Green’s Theorem
> Suppose $C$ is a piecewise smooth simple closed curve that is the boundary of a region $R$ in the plane and oriented so that the region is on the left as we move around the curve.
>
> ![](https://i.imgur.com/CIkg2iD.png)
> *Figure 18.44: Boundary $C$ oriented with $R$ on the left*
>
> Suppose $\vec{F} = F_{1} \vec{i} + F_{2} \vec{j}$ is a smooth vector field on a region containing $R$ and $C$. Then
> $$\int_{C} \vec{F} \cdot d\vec{r} = \int_{R} \left( \frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y }  \right) \, dx\,dy$$

### Proof of Green’s Theorem

![](https://i.imgur.com/3q7q0rh.png)

If $R$ is not a rectangle, we subdivide it into small rectangular pieces as shown in Figure 18.46.
The contribution to the integral of the non-rectangular pieces can be made as small as we like by making the subdivision fine enough.

The double integrals over each piece add up to the double integral over the whole region $R$.
Figure 18.47 shows how the circulations around adjacent pieces cancel along the common edge, so the circulations around all the pieces add up to the circulation around the boundary $C$.

Since Green’s Theorem holds for the rectangular pieces, it holds for the whole region $R$.

![|566x555](https://i.imgur.com/RWfpxMe.png)

> [!example]+ Use Green’s Theorem to evaluate $\int_{C} \left( y^{2} \vec{i} + x\vec{j} \right) \cdot d\vec{r}$ where $C$ is the counterclockwise path around the perimeter of the rectangle $0 ≤ x ≤ 2, 0 ≤ y ≤ 3$.
>
> > [!check]- Solution
> > - $F_{1} = y^{2}$
> > - $F_{2} = x$
> >
> > By Green’s Theorem,
> > $$\int_{C} \left( y^{2}\vec{i} + x \vec{j} \right) \cdot d\vec{r} = \int_{R} \left( \frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } \,dx\,dy \right) = \int_{0}^{3} \int_{0}^{2} (1-2y) \, dx  \, dy = -12$$

## The Curl Test for Vector Fields in the Plane

- & If $\vec{F} = F_{1} \vec{i} + F_{2} \vec{j}$ is a gradient field with continuous partial derivatives, then $$\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } = 0.$$

> [!goal]+ Show that the converse is true if the domain of $\vec{F}$ has no holes in it
> Assume that
> $$\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y }  = 0$$
>
> - @ Show that $\vec{F}$ is path-independent

If $C$ is any oriented simple closed curve in the domain of $\vec{F}$ and $R$ is the region inside $C$, then
$$
\int_{R} \left( \frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y } \right) \,dx\,dy = 0
$$
since the integrand is identically 0.
By Green’s Theorem,
$$
\int_{C} \vec{F} \cdot d\vec{r} = \int_{R} \left( \frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y }  \right) \,dx\,dy = 0
$$
Thus, ==$\vec{F}$ is path-independent== and therefore a ==gradient field==.

- Argument is valid for every closed curve, $C$
    - Provided the region $R$ is entirely in the domain of $\vec{F}$

> [!thm]+ The Curl Test for Vector Fields in 2-Space
> Suppose $\vec{F} = F_{1} \vec{i} + F_{2}\vec{j}$ is a vector field with continuous partial derivatives such that:
> 1. Domain of $\vec{F}$ has the property that every closed curve in it encircles a region that lies entirely within the domain
>     - In particular, domain of $\vec{F}$ has no holes
> 2. $\frac{ \partial F_{2} }{ \partial x }- \frac{ \partial F_{1} }{ \partial y } = 0$
>
> Then $\vec{F}$ is path-independent, so $\vec{F}$ is a gradient field and has a potential function.

## The Curl Test for Vector Fields in 3-Space

> [!def]+ Curl of a 3-dimensional vector field
> If $\vec{F}(x, y, z) = F_{1} \vec{i} + F_{2} \vec{j} + F_{3}\vec{k}$, we define the **curl** vector field by
> $$\text{curl }\vec{F} = \left( \frac{ \partial F_{3} }{ \partial y } - \frac{ \partial F_{2} }{ \partial z }  \right) \vec{i} + \left( \frac{ \partial F_{1} }{ \partial z } - \frac{ \partial F_{3} }{ \partial x }  \right) \vec{j} + \left( \frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y }  \right) \vec{k}$$

> [!thm]+ The curl test for vector fields in 3-space
> The vector field $\vec{F} = F_{1}\vec{i} + F_{2}\vec{j} + F_{3}\vec{k}$ is a path-independent vector field if both
>
> 1. Domain of $\vec{F}$ has the property that every closed curve in it can be contracted to a point in a smooth way, staying at all times within the domain, and
> 2. $\text{curl }\vec{F} = \vec{0}$

---

### PCA Example

Let $\vec{F}$ be the vector field given by $\vec{F}(x, y) = \frac{-y\vec{i} + x\vec{j}}{x^{2} + y^{2}}$.

1. Calculate $\frac{ \partial F_{2} }{ \partial x } - \frac{ \partial F_{1} }{ \partial y }$. Does the curl test imply that $\vec{F}$ is path-independent?
2. Calculate $\int_{C} \vec{F} \cdot d\vec{r}$, where $C$ is the unit circle centred at the origin and oriented counterclockwise. Is $\vec{F}$ a path-independent vector field?
3. Explain why the pasts to parts 1. and 2. do not contradict Green’s Theorem

![](https://i.imgur.com/NFg62jI.png)
