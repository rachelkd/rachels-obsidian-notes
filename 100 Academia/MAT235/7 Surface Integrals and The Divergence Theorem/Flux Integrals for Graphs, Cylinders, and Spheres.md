---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 21
Module:
  - "[[7 - Surface Integrals and The Divergence Theorem]]"
Lecture:
  - "58"
  - "59"
  - "60"
Chapter: "19.2"
Slides/Notes:
Date: 2025-03-10
Date created: Wed., Apr. 9, 2025, 2:09:38 pm
Date modified: Thu., Apr. 10, 2025, 4:28:16 am
---

# Flux Integrals for Graphs, Cylinders, and Spheres

- Recall [[The Idea of a Flux Integral]]:
    - Computed flux integrals in certain simple cases

> [!goal]+ This section
>
> - Compute flux through surfaces that are graphs of functions, through cylinders, and through spheres

## Flux of a Vector Field through the Graph of $z = f(x, y)$

- Suppose:
    - $S$ is the graph of differentiable function $z = f(x, y)$
        - Oriented upward
    - $\vec{F}$ is a smooth vector field
- In [[The Idea of a Flux Integral|Section 19.1]]:
    - Subdivided surface into small pieces with area vector $\Delta \vec{A}$
    - Defined flux of $\vec{F}$ through $S$ as follows:$$\int_{S} \vec{F} \cdot d\vec{A} = \lim_{ \| \Delta \vec{A} \| \to 0 } \sum \vec{F} \cdot \Delta \vec{A}$$

> [!question]+ How do we divide $S$ into small pieces?
>
> - Use cross sections of $f$ with $x$ or $y$ constant
> - Take the patches in a wire frame representation of the surface
> - → Must calculate the area vector of one of these areas
>     - Which is approximately a parallelogram

### Area Vector of a Coordinate Patch

- Recall geometric definition of the [[The Cross Product#Definition of the Cross Product|cross product]]:
    - Vector $\vec{v} \times \vec{w}$ has magnitude equal to area of the parallelogram formed by $\vec{v}$ and $\vec{w}$, and
    - Direction perpendicular to this parallelogram
        - Determined by right-hand rule
            - Given $\vec{a} \times \vec{b} = \vec{c}$:
                - $\vec{a}$ is index finger
                - $\vec{b}$ is middle finger
                - $\vec{c}$ is thumb

$$
\text{Area vector of parallelogram} = \vec{A} = \vec{v} \times \vec{w}
$$

![](https://i.imgur.com/cK9m01G.png)

*Figure 19.18: Surface showing coordinate patch and tangent vectors $\vec{r}_{x}$ and $\vec{r}_{y}$*

![](https://i.imgur.com/7Dw7pUT.png)

*Figure 19.19: Parallelogram-shaped patch in the tangent plane to the surface*

- Consider the patch of surface above the rectangular region with sides $\Delta x, \Delta y$ in the $xy$-plane
    - (Figure 19.18)
    - & We approximate area vector $\Delta \vec{A}$ of this patch by the area vector of the corresponding patch on the tangent plane to the surface
        - (Figure 19.19)
    - Patch is the parallelogram determined by the vectors $\vec{v}_{x}$ and $\vec{v}_{y}$
        - & → Area vector is given by$$\Delta \vec{A} \approx \vec{v}_{x} \times \vec{v}_{y}$$
- To find $\vec{v}_{x}, \vec{v}_{y}$:
    - $ Notice a point on the surface has position vector $\vec{r} = x\vec{i} + y\vec{j} + f(x, y) \vec{k}$

> [!thm]+ Cross section of $S$ with $y$ constant has tangent vector
> $$\vec{r}_{x} = \frac{ \partial \vec{r} }{ \partial x } = \vec{i} + f_{x} \vec{k}$$

> [!thm]+ Cross section of $S$ with $x$ constant has tangent vector
> $$\vec{r}_{y} = \frac{ \partial \vec{r} }{ \partial y } = \vec{j} + f_{y} \vec{k}$$

- Vectors $\vec{r}_{x}$ and $\vec{v}_{x}$ are *parallel*
    - $\because$ Both are tangent to the surface and parallel to the $xz$-plane
- $x$-component of $\vec{r}_{x}$ is $\vec{i}$; $x$-component of $\vec{v}_{x}$ is $(\Delta x)\vec{i}$
    - → $\vec{v}_{x} = (\Delta x) \vec{r}_{x}$
- Similarly: $\vec{v}_{y} = (\Delta y)\vec{r}_{y}$

So, the upward-pointing area vector of the parallelogram is:
$$\Delta \vec{A} \approx \vec{v}_{x} \times \vec{v}_{y} = \left( \vec{r}_{x} \times \vec{r}_{y} \right) \Delta x \Delta y = \left( -f_{x}\vec{i} - f_{y} \vec{j} + \vec{k} \right) \Delta x \Delta y$$

- % This is our *approximation* for the area vector $\Delta \vec{A}$ on the surface

Replacing $\Delta \vec{A}, \Delta x, \Delta y$ by $d\vec{A}, dx, dy$:
$$
d\vec{A} = \left( -f_{x} \vec{i} - f_{y} \vec{j} + \vec{k} \right) \,dx\,dy
$$

> [!thm]+ Flux of $\vec{F}$ through a surface given by a graph of $z = f(x, y)$
> Suppose the surface $S$ is part of the graph $z = f(x, y)$ above a region $R$ in the $xy$-plane, and suppose $S$ is oriented upward.
> The **flux of $\vec{F}$ through $S$** is
> $$\int_{S} \vec{F} \cdot d\vec{A} = \int_{R} \vec{F}\left(x, y, f(x, y)\right) \cdot (-f_{x} \vec{i} - f_{y} \vec{j} + \vec{k}) \, dx \, dy$$

> [!example]+ Example 1
> Compute $\int_{S} \vec{F} \cdot d\vec{A}$ where $\vec{F}(x, y, z) = z \vec{k}$ and $S$ is the rectangular plate with corners $(0, 0, 0), (1, 0, 0), (0, 1, 3), (1, 1, 3)$, oriented upward.
> ![](https://i.imgur.com/fvoSWo9.png)
> *Figure 19.20: The vector field $\vec{F} = z\vec{k}$ on the rectangular surface $S$*
>
> > [!check]+ Solution
> > We find the equation for the plane $S$ in the form $z = f(x, y)$.
> > Since $f$ is linear, with $x$-slope equal to 0 and $y$-slope 3, and $f(0, 0) = 0$, we have:
> > $$z = f(x, y) = 0 + 0x + 3y = 3y$$
> > Then,
> > $$d\vec{A} = \left( -f_{x} \vec{i} - f_{y} \vec{j} + \vec{k} \right) \, dx \, dy = (0 \vec{i} - 3 \vec{j} + \vec{k})\,dx\,dy = (-3 \vec{j} + \vec{k})\, dx \, dy$$
> > The flux integral is therefore
> > $$
> > \begin{align}
> > \int_{S} \vec{F} \cdot d\vec{A} & = \int_{0}^{1} \int_{0}^{1} 3y \vec{k} \cdot \left( -3 \vec{j} + \vec{k} \right)  \, dx  \, dy  \\
> >  & = \int_{0}^{1} \int_{0}^{1} 3y \, dx  \, dy \\
> >  & = 1.5
> > \end{align}
> > $$

### Surface Area of a Graph

> [!tip]+ We can find area of a surface by integrating the magnitude $\| d\vec{A} \|$
>
> - Since magnitude of an area vector is *area*

If a surface is the graph of a function $z = f(x, y)$, we have:
$$
\| d\vec{A} \| = \left\| -f_{x} \vec{i} - f_{y} \vec{j} + \vec{k} \right\|\, dx\, dy = \sqrt{ \left( f_{x} \right)^{2} + (f_{y})^{2} + 1  } \, dx\, dy
$$
Then:

> [!thm]+ Surface area of a graph $z = f(x, y)$
> Suppose a surface $S$ is part of the graph $z = f(x, y)$ where $(x, y)$ is a region $R$ in the $xy$-plane.
> Then
> $$\text{Area of }S = \int_{R} \sqrt{ \left( f_{x} \right) ^{2} + \left( f_{y} \right) ^{2} + 1 } \, dx \, dy$$

> [!example]+ Example 2
> Find the area of the surface $z = f(x, y)$ where $0 \leq x \leq 4, 0 \leq y \leq 5$ when:
>
> 1. $f(x, y) = 2x + 3y + 4$
> 2. $f(x, y) = x^{2} + y^{2}$
>
> > [!check]+ 1
> > Since $f_{x} = 2$ and $f_{y} = 3$, we have:
> > $$\text{Area} = \int_{0}^{5} \int_{0}^{4} \sqrt{ 2^{2} + 3^{2} + 1 } \, dx  \, dy = 20 \sqrt{ 14 }$$
>
> > [!check]+ 2
> > Since $f_{x} = 2x$ and $f_{y} = 2y$, we have:
> > $$\text{Area} = \int_{0}^{5} \int_{0}^{4} \sqrt{ 4x^{2} + 4y^{2} + 1 } \, dx  \, dy = 140.089$$
>
> Surface area integrals can often only be evaluated numerically.

## Flux of a Vector Field through a Cylindrical Surface

Consider the cylinder of radius $R$ centered on the $z$-axis illustrated in Figure 19.21 and oriented away from $z$-axis.

![](https://i.imgur.com/6C7C9oz.png)

*Figure 19.21: Outward-oriented cylinder*

The coordinate patch in Figure 19.22 has surface area given by:
$$
\Delta A \approx R \Delta \theta \Delta z
$$
![](https://i.imgur.com/tlgwsOr.png)

*Figure 19.22: Coordinate patch with area $\Delta \vec{A}$ on surface of a cylinder*

The outward unit normal $\vec{n}$ points in the direction of $x \vec{i} + y \vec{j}$, so:
$$
\vec{n} = \frac{x \vec{i} + y \vec{j}}{\left\| x \vec{i} + y \vec{j} \right\|} = \frac{R \cos \theta \vec{i} + R \sin \theta \vec{j}}{R} = \cos \theta \vec{i} + \sin \theta \vec{j}
$$
Therefore, area vector of coordinate patch is approximated by:
$$
\Delta \vec{A} = \vec{n} \Delta A \approx \left( \cos \theta \vec{i} + \sin \theta \vec{j} \right) R\Delta\theta\Delta\theta,
$$
which gives us:
$$
d\vec{A} = \left( \cos \theta \vec{i} + \sin \theta \vec{j} \right) R\, dz\, d\theta.
$$
This gives us the following result:

> [!thm]+ Flux of a vector field through a cylinder
> The flux of $\vec{F}$ through the cylindrical surface $S$, of radius $R$ and oriented away from the $z$-axis, is given by:
> $$\int_{S} \vec{F} \cdot d\vec{A} = \int_{T} \vec{F}(R, \theta, z) \cdot \left( \cos \theta \vec{i} + \sin \theta \vec{j} \right) R \, dz\,d\theta,$$
> where $T$ is the $\theta z$-region corresponding to $S$.

> [!example]+ Example 3
> Compute $\int_S \vec{F} \cdot d\vec{A}$ where $\vec{F}(x,y,z) = y\vec{j}$ and $S$ is the part of the cylinder of radius 2 centered on the $z$-axis with $x \geq 0$, $y \geq 0$, and $0 \leq z \leq 3$. The surface is oriented toward the $z$-axis.
>
> ![](https://i.imgur.com/GrUWFl9.png)
>
> *Figure 19.23: The vector field $\vec{F} = y \vec{j}$ on the surface $S$*
>
> > [!check]+ Solution
> > First, note that the orientation is toward the $z$-axis (inward), which is opposite to our standard formula. This means we need to negate our area vector:
> >
> > $$d\vec{A} = -(\cos\theta\vec{i} + \sin\theta\vec{j})R\,dz\,d\theta$$
> >
> > In cylindrical coordinates, we have $R = 2$ and $\vec{F} = y\vec{j} = 2\sin\theta\vec{j}$ (since $y = R\sin\theta = 2\sin\theta$).
> >
> > Since the surface lies in the first quadrant ($x \geq 0$, $y \geq 0$), we have $0 \leq \theta \leq \pi/2$.
> >
> > Now we can compute the flux:
> >
> > $$\begin{align}
> > \int_S \vec{F} \cdot d\vec{A} &= \int_T \vec{F}(R,\theta,z) \cdot (-\cos\theta\vec{i} - \sin\theta\vec{j})R\,dz\,d\theta \\
> > &= \int_0^{3} \int_0^{\pi/2} (2\sin\theta\vec{j}) \cdot (-\cos\theta\vec{i} - \sin\theta\vec{j})(2)\,d\theta\,dz \\
> > &= \int_0^{3} \int_0^{\pi/2} -2\sin\theta\vec{j} \cdot \sin\theta\vec{j} \cdot 2\,d\theta\,dz \\
> > &= \int_0^{3} \int_0^{\pi/2} -4\sin^2\theta\,d\theta\,dz \\
> > &= -4 \int_0^{3} \int_0^{\pi/2} \sin^2\theta\,d\theta\,dz
> > \end{align}$$
> >
> > We know that $\int_0^{\pi/2} \sin^2\theta\,d\theta = \pi/4$, so:
> >
> > $$\begin{align}
> > \int_S \vec{F} \cdot d\vec{A} &= -4 \int_0^{3} \frac{\pi}{4}\,dz \\
> > &= -4 \cdot \frac{\pi}{4} \cdot 3 \\
> > &= -3\pi
> > \end{align}$$

## Flux of a Vector Field through a Spherical Surface

Consider the piece of the sphere of radius $R$ centered at the origin, oriented outward, as illustrated in Figure 19.24.

![](https://i.imgur.com/rNs54VM.png)

*Figure 19.24: Coordinate patch with area $\Delta \vec{A}$ on surface of a sphere*

The coordinate patch in Figure 19.24 has surface area given by
$$
\Delta A \approx R^{2} \sin \phi \Delta \phi \Delta \theta.
$$
The outward unit normal $\vec{n}$ points in the direction of $\vec{r} = x \vec{i} + y \vec{j} + z \vec{k}$, so:
$$
\vec{n} = \frac{\vec{r}}{\left \| \vec{r} \right \|} = \sin \phi \cos \theta \vec{i} + \sin \phi \sin \theta \vec{j} + \cos \phi \vec{k}.
$$
Therefore, area vector of the coordinate patch is approximated by:
$$
\Delta \vec{A} \approx \vec{n} \Delta A = \frac{\vec{r}}{\left\| \vec{r} \right\|}\Delta A = \left( \sin \phi \cos \theta \vec{i} + \sin \phi \sin \theta \vec{j} + \cos \phi \vec{k} \right) R^{2} \sin \phi \Delta \phi \Delta \theta.
$$
Therefore:
$$
d\vec{A} = \frac{\vec{r}}{\left\| \vec{r} \right\|}dA = \left( \sin \phi \cos \theta \vec{i} + \sin \phi \sin \theta \vec{j} + \cos \phi \vec{k} \right) R^{2} \sin \phi \, d\phi \, d\theta.
$$

> [!thm]+ Flux of a vector field through a sphere
> The flux of $F$ through the spherical surface $S$, with radius $R$ and oriented away from the origin, is given by
> $$\begin{align} \int_{S} \vec{F} \cdot d\vec{A}  & = \int_{S} \vec{F} \cdot \frac{\vec{r}}{\left\| \vec{r} \right \|}dA \\  & = \int_{T} \vec{F}(R, \theta, \phi) \cdot \left( \sin \phi \cos \theta \vec{i} + \sin \phi \sin \theta \vec{j} + \cos \phi \vec{k} \right) R^{2} \sin \phi \, d\phi, d\theta, \end{align}$$
> where $T$ is the $\theta \phi$-region corresponding to $S$.

> [!example]+ Example 4
> Find the flux of $\vec{F} = z\vec{k}$ through $S$, the upper hemisphere of radius 2 centered at the origin, oriented outward.
>
> > [!check]+ Solution
> > The hemisphere $S$ is parameterized by spherical coordinates $\theta$ and $\phi$, with $0 \leq \theta \leq 2\pi$ and $0 \leq \phi \leq \pi/2$ (where $\phi$ is the angle from the positive z-axis).
> >
> > Since $R = 2$ and $\vec{F} = z\vec{k} = 2\cos\phi\vec{k}$ (because $z = R\cos\phi = 2\cos\phi$ in spherical coordinates), the flux is:
> >
> > $$\begin{align}
> > \int_S \vec{F} \cdot d\vec{A} &= \int_S 2\cos\phi\vec{k} \cdot (\sin\phi\cos\theta\vec{i} + \sin\phi\sin\theta\vec{j} + \cos\phi\vec{k})4\sin\phi\,d\phi\,d\theta \\
> > &= \int_0^{2\pi}\int_0^{\pi/2} 2\cos\phi\vec{k} \cdot \cos\phi\vec{k} \cdot 4\sin\phi\,d\phi\,d\theta \\
> > &= \int_0^{2\pi}\int_0^{\pi/2} 8\cos^2\phi\sin\phi\,d\phi\,d\theta
> > \end{align}$$
> >
> > Then:
> >
> > $$\begin{align}
> > \int_0^{\pi/2} 8\cos^2\phi\sin\phi\,d\phi &= 8\int_0^{\pi/2} \cos^2\phi\sin\phi\,d\phi
> > \end{align}$$
> >
> > Using the substitution $u = \cos\phi$ (which gives $du = -\sin\phi\,d\phi$), we get:
> >
> > $$\begin{align}
> > 8\int_0^{\pi/2} \cos^2\phi\sin\phi\,d\phi &= 8\int_1^0 \cos^2\phi \cdot (-du) \\
> > &= 8\int_0^1 u^2\,du \\
> > &= 8\left[\frac{u^3}{3}\right]_0^1 \\
> > &= 8 \cdot \frac{1}{3} = \frac{8}{3}
> > \end{align}$$
> >
> > Now, integrating with respect to $\theta$:
> >
> > $$\begin{align}
> > \int_0^{2\pi}\int_0^{\pi/2} 8\cos^2\phi\sin\phi\,d\phi\,d\theta &= \int_0^{2\pi} \frac{8}{3}\,d\theta \\
> > &= \frac{8}{3} \cdot 2\pi \\
> > &= \frac{16\pi}{3}
> > \end{align}$$
> >
> > Therefore, the flux of $\vec{F} = z\vec{k}$ through the upper hemisphere is $\frac{16\pi}{3}$.

> [!example]+ Example 2
> Compute the surface area of a sphere of radius $a$.
>
> > [!check]+ Solution
> > We take the sphere $S$ of a radius $a$ centered at the origin and parameterize it with the spherical coordinates $\phi$ and $\theta$.
> > The parameterization is:
> > $$x = a \sin \phi \cos \theta, \quad y = a \sin \phi \sin \theta, \quad z = a \cos \phi, \quad \text{for } 0 \leq \theta \leq 2\pi, 0 \leq \phi \leq \phi$$
> > We compute:
> >
> > $$
> > \begin{align}
> > \frac{ \partial \vec{r} }{ \partial \phi } \times \frac{ \partial \vec{r} }{ \partial \theta }  & = \left( a \cos \phi \cos \theta \vec{i} + a \cos \phi \sin \theta \vec{j} - a \sin \phi \vec{k} \right) \times \left( -a \sin \phi \sin \theta \vec{i} + a \sin \phi \cos \theta \vec{j} \right)  \\
> >  & = a^{2} \left( \sin ^{2}\phi \cos \theta \vec{i} + \sin ^{2} \phi \sin \theta \vec{j} + \sin \phi \cos \phi \vec{k} \right) 
> > \end{align}
> > $$
> > and so
> > $$
> > \left \| \frac{ \partial \vec{r} }{ \partial \phi } \times \frac{ \partial \vec{r} }{ \partial \theta } \right \| = a^{2} \sin \phi
> > $$
> > Thus, we see that the *surface area* of the sphere $S$ is given by
> > $$
> > \text{Surface area} = \int_{S}dA = \int_{R} \left \| \frac{ \partial \vec{r} }{ \partial \phi }  \times \frac{ \partial \vec{r} }{ \partial \theta } \right \| \, d\phi d \theta = \int_{\phi = 0}^{\pi} \int_{\theta = 0}^{2\pi} a^{2} \sin \phi \, d\theta \, d\phi = 4\pi a^{2}
> > $$
