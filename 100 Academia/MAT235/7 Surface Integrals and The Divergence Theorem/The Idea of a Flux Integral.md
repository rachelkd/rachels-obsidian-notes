---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 20
Module:
  - "[[7 - Surface Integrals and The Divergence Theorem]]"
Lecture:
  - "55"
  - "56"
  - "57"
Chapter: "19.1"
Slides/Notes: 
Date: 2025-03-03
Date created: Mon., Apr. 7, 2025, 11:13:18 pm
Date modified: Tue., Apr. 8, 2025, 3:18:09 am
---

# The Idea of a Flux Integral

## Flow through a Surface

- Imagine water flowing through a fishing net
    - Net stretched across a stream
- @ Suppose we want to measure the flow rate of water through the net
    - i.e., volume of fluid that passes through the surface per unit time

> [!example]+ Example 1
> A flat square surface of area $A$, in $m^{2}$, is immersed in a fluid. The fluid flows with constant velocity $\vec{v}$, in m/sec, perpendicular to the square.
> Write an expression for the rate of flow in $m^{3}$/sec.
>
> > [!check]+ Solution
> > - In one second:
> >     - A given particle of water moves a distance of $\| \vec{v} \|$ in direction perpendicular to square
> > - $\implies$ Entire body of water moving through square in one second is a *box*
> >     - of length $\| \vec{v} \|$, and
> >     - cross-sectional area $A$
> > - $\implies$ Box has volume $\| \vec{v} \| A \; \text{m}^{3}$
> > $$\text{Flow rate} = \| \vec{v} \| A \; \text{m}^{3}/\text{sec}$$

> [!def]+ Flux
> - *Flow rate* is called the **flux** of the fluid through the surface

- Can also compute *flux of **vector fields***
    - e.g., electric and magnetic fields
        - Where no flow is actually taking place

> [!tip]+ If vector field is constant and perpendicular to surface, and if surface is flat…
> - Flux is obtained by:
>     - & Multiplying speed by the area
> - Example 1

> [!goal]+ Next:
> - Find the flux of a constant vector field through a flat surface that is not perpendicular to the vector field
>     - Using *dot product*

- % Break a surface into small pieces which are approximately *flat* and where vector field is approximately *constant*
    - → Leads to a **flux integral**

### Orientation of a Surface

- Before computing flux of vector field through a surface:
    - @ Need to decide which direction of flow through the surface is the *positive* direction
        - Called **choosing an orientation**

> [!def]+ Choosing an orientation
> - At each point on a smooth surface, there are two unit normals
>     - One in each direction
> - **Choosing an orientation** means picking one of these normals at every point of the surface in a continuous way
> - *Unit normal vector* in the direction of the orientation
>     - Denoted by $\vec{n}$
> - For a *closed surface* i.e., boundary of a solid region:
>     - Choose the **outward orientation**
>         - Unless otherwise specified

> [!important]+ Positive and negative flux
> - Flux through a piece of surface is **positive** if flow is in the *direction of the orientation*
>     - **Negative** if it is in the opposite direction
>
> ![](https://i.imgur.com/CSSVWl1.png)
> *Figure 19.2: An oriented surface showing directions of positive and negative flow*

![](https://i.imgur.com/gvc7Uyk.png)

*Figure 19.3: Area vector $\vec{A} = \vec{n} A$ of flat surface with area $A$ and orientation $\vec{n}$*

### Area Vector

- Flux through a flat surface depends on both the *area of the surface* and its *orientation*
    - → Useful to represent its area by a vector
        - As shown in Figure 19.3

> [!def]+ Area vector
> - The **area vector** of a *flat*, *oriented* surface is a vector $\vec{A}$ such that:
>     - Magnitude of $\vec{A}$ is the area of the surface
>     - Direction of $\vec{A}$ is the direction of the orientation vector $\vec{n}$

### Flux of a Constant Vector Field through a Flat Surface

- Suppose:
    - Velocity vector field $\vec{v}$ of a fluid is constant, and
    - $\vec{A}$ is the area vector of a flat surface
- Flux through this surface:
    - Volume of fluid that flows through in one unit of time

![](https://i.imgur.com/ST44ovw.png)

*Figure 19.4: Flux of $\vec{v}$ through a surface with area vector $\vec{A}$ is the volume of this skewed box*

- Skewed box has:
    - Cross-sectional area $\| \vec{A} \|$
        - By definition of area vector
    - Height $\| \vec{v} \| \cos \theta$
- $\implies$ Volume is $\left( \| \vec{v} \| \cos \theta \right) \| \vec{A} \| = \vec{v} \cdot \vec{A}$

This gives us this following result:

> [!thm]+ Flux through a flat surface and constant velocity vector field
> If $\vec{v}$ is constant and $\vec{A}$ is the area of a flat surface, then
> $$\text{Flux through surface} = \vec{v} \cdot \vec{A}$$

> [!example]+ Example 2
> Water is slowing down a cylindrical pipe 2 cm in radius with a velocityof 3 cm/sec. Find the flux of the velocity vector field through the ellipse-shaped region shown in Figure 19.5.
> The normal to the ellipse makes an angle of $\theta$ with the direction of flow and the area of the ellipse is $4\pi/(\cos\theta)$ cm$^{2}$.
>
> ![](https://i.imgur.com/s6iYfTu.png)
> *Figure 19.5: Flux through ellipse-shaped region across a cylindrical pipe*
>
> > [!check]+ Solution
> > $$
> > \begin{align}
> > \text{Flux through ellipse} & = \vec{v} \cdot \vec{A} \\
> >  & = \|\vec{v}\| \| \vec{A} \| \cos \theta \\
> >  & = 3(\text{area of ellipse}) \cos\theta \\
> >  & = 3 \left( \frac{4\pi}{\cos\theta} \right) \cos\theta \\
> >  & = 12 \pi \; \text{cm}^{3}/\text{sec}
> > \end{align}
> > $$
> >
> > Another way is to notice that the flux through the ellipse is equal to the *flux through the circle perpendicular to the pipe* in Figure 19.5.
> > Since flux is the rate at which water is flowing down the pipe, we have:
> > $$
> > \begin{align}
> > \text{Flux through circle} & = \text{Velocity of water} \times \text{Area of circle} \\
> >  & = (3 \; \text{cm/sec})(\pi2^{2} \; \text{cm}^{2}) \\
> >  & = 12 \pi \; \text{cm}^{3}\text{/sec}
> > \end{align}
> > $$

## The Flux Integral

- If vector field $\vec{F}$ is *not* constant or the surface $S$ is *not* flat:
    - Divide the surface into a patchwork of small, almost flat pieces
    - See Figure 19.6
- For a particular patch with area $\Delta A$:
    - Pick a unit orientation vector $\vec{n}$ at a point on the patch
    - Define the area vector of the patch $\Delta \vec{A}$ as $$\Delta \vec{A} = \vec{n} \Delta A$$
    - & If the patches are small enough, we can assume that $\vec{F}$ is approximately constant on each piece

![](https://i.imgur.com/SRqp45R.png)

*Figure 19.6: Surface $S$ divided into small, almost flat pieces, showing a typical orientation vector $\vec{n}$ and area vector $\Delta \vec{A}$*

Then, we know that:
$$
\text{Flux through patch} \approx \vec{F} \cdot \Delta \vec{A},
$$
so, adding all the fluxes through all the small pieces, we have:
$$
\text{Flux through whole surface} \approx \sum \vec{F} \cdot \Delta \vec{A}.
$$
As each patch becomes smaller and $\| \Delta \vec{A} \| \to 0$, the approximation gets better and we get:
$$
\text{Flux through }S = \lim_{ \| \Delta \vec{A} \| \to 0 } \sum \vec{F} \cdot \Delta \vec{A}.
$$
Provided the limit exists, we make the following definition:

> [!def]+ Flux integral
> The **flux integral** of the vector field $\vec{F}$ through the oriented surface $S$ is
> $$\int_{S} \vec{F} \cdot d\vec{A} = \lim_{ \|\Delta \vec{A}\| \to 0 } \sum \vec{F} \cdot \Delta \vec{A}.$$
> If $S$ is a closed surface oriented outward, we describe the flux through $S$ as the flux *out of* $S$.

![](https://i.imgur.com/b5RKDjb.png)

*Figure 19.7: Flux of a vector field through a curved surface $S$*

### Flux and Fluid Flow

If $\vec{v}$ is the velocity vector field of a fluid, we have:
$$
\text{Rate fluid flows through surface }S = \text{Flux of } \vec{v} \text{through }S = \int_{S} \vec{v} \cdot d\vec{A}
$$
- % Rate of fluid flow is measured in units of *volume per unit time*

## Calculating Flux Integrals Using $d\vec{A} = \vec{n} dA$

> [!info]+ For a small patch of surface $\Delta S$ with unit normal $\vec{n}$…
> The area vector is $$\Delta \vec{A} = \vec{n} \Delta A$$
