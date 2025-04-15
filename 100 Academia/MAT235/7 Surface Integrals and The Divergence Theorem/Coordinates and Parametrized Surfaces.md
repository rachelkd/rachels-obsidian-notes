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
Chapter: "21.1"
Slides/Notes: 
Date: 2025-03-03
Date created: Fri., Mar. 14, 2025, 12:16:44 am
Date modified: Wed., Apr. 9, 2025, 4:26:44 am
---

# Coordinates and Parametrized Surfaces

> [!abstract]+ Parameterization and coordinate systems are the same thing as polar, cylindrical, and spherical coordinates in different disguises.
> - Functions from one space to another

- Have already seen this with parameterized curves
    - A function from an interval $a \leq t \leq b$ to a curve in $xyz$-space

![](https://i.imgur.com/wqsUYmx.png)

*Figure 21.1: The parameterization is a function from the interval, $a ≤ t ≤ b$, to 3-space, whose image is the curve, $C$*

## Polar, Cylindrical, and Spherical Coordinates Revisited

- Equations for [[Double Integrals in Polar Coordinates|polar coordinates]] can also be viewed as defining a function from the $r\theta$-plane into the $xy$-plane
    - $x = r \cos \theta, \quad y = r \sin \theta$
    - Function transforms rectangle on the left into the quarter disk on the right
        - Figure 21.2
    - Need *two* parameters to describe this disk
        - Because disk is two-dimensional object

![](https://i.imgur.com/AbGoiGe.png)

*Figure 21.2: A grid in the rθ-plane and the corresponding curved grid in the xy-plane*

### Polar Coordinates as Families of Parameterized Curves

- Polar coordinates
    - Give *two* families of parameterized curves
    - → Form the polar coordinate grid
- Lines $r = \text{constant}$ in $r\theta$-plane
    - Correspond to *circles* in the $xy$-plane
    - Each circle parameterized by $\theta$
- Lines $\theta = \text{constant}$
    - Correspond to *rays* in the $xy$-plane
    - Each ray parameterized by $r$

### Cylindrical and Spherical Coordinates

- Cylindrical and spherical coordinates may be viewed as *functions* from 3-space to 3-space
- Cylindrical coordinates take rectangular boxes in $r\theta z$-space
    - Map them to *cylindrical* regions in $xyz$-space
- Spherical coordinates take rectangular boxes in $\rho \phi\theta$-space
    - Map to *spherical* regions in $xyz$-space

### General Parameterizations

> [!info]+ A parameterization or coordinate system provides a way of representing a curved object
> - By means of a simple *region* in the *parameter space*
>     - e.g., an interval, rectangle, or rectangular box
> - Along with a *function* mapping that region into the curved object

- We use this idea to parameterize curved surfaces in 3-space

## How Do We Parameterize a Surface?

In [[Double Integrals in Polar Coordinates|Section 17.1]]:

- Parameterized a circle in 2-space using the equations $$x=\cos t, \quad y = \sin t$$
- 3-space:
    - Same circle in the $xy$-plane has parametric equations $$x = \cos t, \quad y = \sin t, \quad z = 0$$
    - Add $z = 0$ equation to specify that circle is in the $xy$-plane
    - ? What if we want circle in the plane $z = 3$?
        - Use equations $$x = \cos t,\quad y = \sin t,\quad z = 3$$
    - ? What if we let $z$ vary freely, as well as $t$?
        - & Circles in every horizontal plane
        - Forms a cylinder
            - Left of Figure 21.3
- & Need two parameters $t, z$ to parameterize the cylinder

![](https://i.imgur.com/SYMEe1N.png)

*Figure 21.3: The cylinder $x = \cos t, y = \sin t, z = z$*

> [!info]+ Curves versus surfaces
> - Curve
>     - & One-dimensional
>         - Though it may live in two or three dimensions
>     - If we move along it, we can only move backward and forward in one direction
>     - & Only requires one parameter to trace out a curve
> - Surface
>     - & Two-dimensional
>     - At any given point, there are two *independent* directions we can move
>     - e.g., Cylinder:
>         - Can move vertically, or circle around the $z$-axis horizontally
>             - Need *two* parameters to describe it

- Can think of parameters as map coordinates
    - Like longitude and latitude on the surface of the earth
- Parameters for a surface give a *grid* on the surface
    - Figure 21.3; right image
    - Just as polar coordinates give a polar grid on a circular region
- Cylinder case:
    - ? What are the parameters?
        - $t, z$
        - $x = \cos t,\quad y = \sin t,\quad z = z$
        - $0 \leq t \leq 2\pi$
        - $-\infty < z < \infty$
    - Last equation $z = z$
        - Reminds us that we are in three dimensions
        - $z$-coordinate on surface is allowed to vary freely

> [!thm]+ In general
> - Express the coordinates $(x, y, z)$ of a point on a surface $S$ in terms of *two* parameters
>     - $s, t$
>
> $$x = f_{1}(s, t),\quad y = f_{2}(s, t), \quad z = f_{3}(s, t)$$

- As values of $s, t$ vary:
    - Corresponding point $(x, y, z)$ sweeps out the surface $S$
        - Figure 21.4
- **Parameterization of the surface**
    - Function which sends the point $(s, t)$ to the point $(x, y, z)$

![](https://i.imgur.com/4VWKaG1.png)

*Figure 21.4: The parameterization sends each point $(s, t)$ in the parameter region, $R$, to a point $(x, y, z) = (f_{1}(s, t), f_{2}(s, t), f_{3}(s, t))$ in the surface, $S$*

## Using Position Vectors

> [!tip] We can use the position vector $\vec{r} = x \vec{i} + y \vec{j} + z \vec{k}$ to combine the three parametric equations for a surface into a single vector equation.

> [!example]+ The parameterization of the cylinder $x = \cos t, y = \sin t, z = z$
> $$\vec{r}(t, z) = \cos t \vec{i} + \sin t \vec{j} + z \vec{k} \quad 0 \leq t \leq 2\pi, -\infty < z < \infty$$

> [!thm]+ General parameterized surface $S$
> $$\vec{r} (s, t) = f_{1}(s, t) \vec{i} + f_{2}(s, t) \vec{j} + f_{3}(s, t) \vec{k}$$

## Parameterizing a Surface of the Form $z = f(x, y)$

> [!thm]+ Surface of the form $z = f(x, y)$
> Can be parameterized by
> $$x = s, \quad y = t, \quad z = f(s, t)$$

- Think of $x, y$ as *parameters*
    - Rather than introduce new parameters $s, t$
    - & → May write $x = x, y = y, z = f(x, y)$

> [!example]+ Give a parametric description of the lower hemisphere of the sphere $x^{2} + y^{2} + z^{2} = 1$
> - Surface is the graph of the function $z = -\sqrt{ 1 - x^{2} - y^{2} }$
>     - Over the region $x^{2} + y^{2} \leq 1$ in the plane
> - Then, parametric equations are:
>     - $x = s$,
>     - $y = t$,
>     - $z = - \sqrt{ 1 - s^{2} - t^{2} }$
>     - Parameters $s, t$ vary inside and on the *unit circle*

## Parameterizing *Planes*

Consider a plane containing two non-parallel vectors $\vec{v_{1}}$, $\vec{v_{2}}$ and a point $P_{0}$ with position vector $\vec{r_{0}}$.

- Can get to any point on the plane by starting at $P_{0}$ and moving parallel to $\vec{v_{1}}$ or $\vec{v_{2}}$
    - Adding multiples of them to $\vec{r_{0}}$

![](https://i.imgur.com/xvJvmRs.png)

*Figure 21.5: The plane $\vec{r}(s, t) = \vec{r}_{0} + s\vec{v}_{1} + t\vec{v}_{2}$ and some points corresponding to various choices of $s$ and $t$*

- $s\vec{v}_{1}$ is parallel to $\vec{v}_{1}$
- $t\vec{v}_{2}$ is parallel to $\vec{v}_{2}$

This gives us the following result:

> [!thm]+ Parameterizing a Plane
> The plane through the point with position vector $\vec{r}_{0}$ and containing the two nonparallel vectors $\vec{v}_{1}$ and $\vec{v}_{2}$ has parameterization
> $$\vec{r}(s, t) = \vec{r}_{0} + s\vec{v}_{1} + t\vec{v}_{2}$$

> [!thm]+ Parametric equations of a plane
> If:
> - $\vec{r}_{0} = x_{0} \vec{i} + y_{0} \vec{j} + z_{0} \vec{k}$
> - $\vec{v}_{1} = a_{1} \vec{i} + a_{2} \vec{j} + a_{3} \vec{k}$
> - $\vec{v}_{2} = b_{1} \vec{i} + b_{2} \vec{j} + b_{3} \vec{k}$
>
> Then the parameterization of the plane can be expressed with the *parametric equations*
> $$\begin{align} x & = x_{0} + sa_{1} + tb_{1},  \\
> y & = y_{0} + sa_{2} + tb_{2}, \\
> z & = z_{0} + sa_{3} + tb_{3}
> \end{align}$$

- $ Parameterization of the plane expresses coordinates $x, y, z$ as *linear functions* of the parameters $s, t$

> [!example]+ Write a parameterization for the plane through the point $(2, -1, 3)$ and containing the vectors $\vec{v}_{1} = 2 \vec{i} + 3 \vec{j} - \vec{k}$ and $\vec{v}_{2} = \vec{i} - 4 \vec{j} + 5 \vec{k}$.
> $$
> \begin{align}
> \vec{r} (s, t) & = \vec{r}_{0} + s\vec{v}_{1} + t\vec{v}_{2} \\
>  & = 2\vec{i} -\vec{j} + 3\vec{k} + s(2\vec{i} + 3\vec{j} - \vec{k}) + t(\vec{i} - 4 \vec{j} + 5\vec{k}) \\
>  & = (2 + 2s + t) \vec{i} + (-1 + 3s - 4t) \vec{j} + (3 - s + 5t) \vec{k}
> \end{align}
> $$
> Equivalently,
> $$x = 2 + 2s + t, \quad y = -1 + 3s - 4t, \quad z = 3 - s + 5t$$

## Parameterizations Using Spherical Coordinates

Recall the [[Integrals in Cylindrical and Spherical Coordinates|spherical coordinates]] $\rho, \phi, \theta$ introduced in Chapter 16.

- Sphere of radius $\rho$:
    - Can use $\phi, \theta$ as coordinates
    - Similar to latitude and longitude on the surface of the earth
    - Figure 21.6
    - $\phi$ is measured from the north pole
        - Whereas latitude is measured from the equator
- If positive $x$-axis passes through the Greenwich meridian:
    - Longitude and $\theta$ are equal for $0 \leq \theta \leq \pi$

![](https://i.imgur.com/qzsLyB9.png)

*Figure 21.6: Parameterizing the sphere by $\phi$ and $θ$*

### Parameterizing a Sphere Using Spherical Coordinates

> [!info]+ Sphere with radius 1 centered at the origin is parameterized by
> $$x = \sin \phi \cos \theta, \quad y = \sin \phi \sin \theta, \quad z = \cos \phi,$$
> where $0 \leq \theta \leq 2\pi$ and $0 \leq \phi \leq \pi$
>
> ![](https://i.imgur.com/X7zi5DC.png)
>
> *Figure 21.8: The relationship between $x, y, z$ and $ϕ, θ$ on a sphere of radius $1$*
>
> > [!note]+ We can also write these equations in vector form
> > $$\vec{r}(\theta, \phi) = \sin \phi \cos \theta \vec{i} + \sin \phi \sin \theta \vec{j} + \cos \phi \vec{k}$$

Then,
$$
\begin{align}
x^{2} + y^{2} + z ^{2}  & = \sin ^{2}\phi(\cos ^{2}\theta + \sin ^{2}\theta) + \cos ^{2}\phi \\
 & = \sin ^{2}\phi + \cos ^{2}\phi \\
 & = 1
\end{align}
$$
- $ Verifies that the point with position vector $\vec{r}(\theta,\phi)$ lies on the sphere of radius 1
- $ Notice that $z$-coordinate depends only on the parameter $\phi$
    - → All points on the same *latitude* have the same $z$-coordinate

> [!example]+ Example 5
> Find parametric equations for the following spheres:
> 1. Center at origin and radius 2
> 2. Center at point with Cartesian coordinates $(2, -1, 3)$ and radius 2
>
> > [!check]+ Solution
> >
> > > [!note]+ 1
> > > - Must scale distance from origin by 2
> > >
> > > $$x = 2 \sin \phi \cos \theta, \quad y = 2 \sin \phi \sin \theta, z = 2 \cos \phi,$$
> > > where $0\leq 2 \leq 2\pi$ and $0 \leq \phi \leq \pi$.
> > > In vector form:
> > > $$\vec{r}(\theta, \phi) = 2 \sin \phi \cos \theta \vec{i} + 2 \sin \phi \sin \theta \vec{j} + 2 \cos \phi \vec{k}$$
> >
> > > [!note]+ 2
> > > - To shift center of sphere from origin to point $(2, -1, 3)$:
> > >     - Add vector parameterization we found in part 1. to position vector of $(2, -1, 3)$
> > >
> > > $$
> > > \begin{align}
> > > \vec{r}(\theta, \phi) & = 2 \vec{i} - \vec{j} + 3\vec{k} + (2 \sin \phi \cos \theta \vec{i} + 2 \sin \phi \sin \theta \vec{j} + 2 \cos \phi \vec{k}) \\
> > > & =(2 + 2 \sin \phi \cos \theta) \vec{i} + (-1 + 2 \sin \phi \sin \theta) \vec{k} + (3 + 2 \cos \phi) \vec{k},
> > >\end{align}
> > > $$
> > > where $0 \leq \theta \leq 2\pi$ and $0 \leq \phi \leq \pi$.
> > > Alternatively,
> > > $$x = 2 + 2 \sin \phi \cos \theta, \quad y = - 1 + 2 \sin \phi \sin \theta, \quad z = 3 + 2 \cos \phi.$$
> > >
> > > ![](https://i.imgur.com/zFkxoen.png)
> > > *Figure 21.9: Sphere with center at the point $(2, –1, 3)$ and radius 2*

## Parameterizing Surfaces of Revolution

- Many surfaces have an axis of rotational symmetry and circular cross sections perpendicular to that axis
    - & Surfaces are referred to as **surfaces of revolution**

> [!example]+ Example 6
> Find a parameterization of the cone whose base is the circle $x^{2} + y^{2} = a^{2}$ in the $xy$-plane and whose vertex is at height $h$ above the $xy$-plane.
>
> ![](https://i.imgur.com/Qs0qcFw.png)
>
> *Figure 21.10: The cone whose base is the circle $x^{2} + y^{2} = a^{2}$ in the $xy$-plane and whose vertex is at the point $(0, 0, h)$ and the vertical cross section through the cone*
>
> > [!check]+ Solution
> > - Use cylindrical coordinates $r, \theta, z$
> > - In $xy$-plane:
> >     - Radius vector $\vec{r}_{0}$ from $z$-axis to a point on the cone in the $xy$-plane is $$\vec{r}_{0} = a \cos \theta \vec{i} + a \sin \theta \vec{j}$$
> > - Above $xy$-plane:
> >     - Radius of the circular cross section $r$ decreases linearly from $r = a$ when $z = 0$ to $r = 0$ when $z = h$
> >     - By similar triangles: $$\frac{a}{h} = \frac{r}{h - z}$$
> > - Solving for $r$: $$r = \left( 1 - \frac{z}{h} \right) a$$
> > - Horizontal radius vector $\vec{r}_{1}$ at height $z$ has components similar to $\vec{r}_{0}$
> >     - ! But with $a$ replaced by $r$
> > $$\vec{r}_{1} = r \cos \theta \vec{i} + r \sin \theta \vec{j} = \left( 1 - \frac{z}{h} \right) a \cos \theta \vec{i} + \left( 1 - \frac{z}{h} \right) a \sin \theta \vec{j}$$
> > - As $\theta$ goes from $0 \to 2\pi$:
> >     - $\vec{r}_{1}$ traces out the horizontal circle in Figure 21.10
> >     - → Get the position vector $\vec{r}$ of a point on the cone by adding the vector $z\vec{k}$
> >
> > Then, we get:
> > $$\vec{r} = \vec{r}_{1} + z\vec{k} = a \left( 1 - \frac{z}{h} \right) \cos \theta \vec{i} + a \left( 1 - \frac{z}{h} \right) \sin \theta \vec{j} + z\vec{k},$$
> > for $0 \leq z \leq h$ and $0 \leq \theta \leq 2\pi$
> > These equations can be written as:
> > $$x = \left( 1 - \frac{z}{h} \right) a \cos \theta, \quad y = \left( 1 - \frac{z}{h} \right) a \sin \theta, \quad z = z$$
> > The parameters are $\theta$ and $z$.

> [!example]+ Example 7
> Consider the bell of a trumpet. A model for the radius $z = f(x)$ of the horn (in cm) at a distance $x$ cm from the large open end is given by the function
> $$f(x) = \frac{6}{(x + 1)^{0.7}}.$$
> The bell is obtained by rotating the graph of $f$ about the $x$-axis. Find a parameterization for the first 24 cm of the bell.
>
> ![](https://i.imgur.com/efnPsk6.png)
> *Figure 21.11: The bell of a trumpet obtained by rotating the graph of $z = f(x)$ about the $x$-axis*
>
> > [!check]+ Solution
> > - At distance $x$ from large open end of horn:
> >     - Cross section parallel to the $yz$-plane is a *circle* of radius $f(x)$ with center on $x$-axis
> >     - Such a circle can be parameterized by $$y = f(x) \cos \theta, \quad z = f(x) \sin \theta$$
> >
> > Thus, we have the parameterization
> > $$x = x, \quad y = \left( \frac{6}{(x + 1)^{0.7}} \right) \cos \theta, \quad z = \left( \frac{6}{(x + 1)^{0.7}} \right) \sin \theta, \quad 0 \leq x \leq 24, \quad 0 \leq \theta \leq 2\pi.$$
> > The parameters are $x$ and $\theta$.

## Parameter Curves

> [!def]+ Parameter cruves
> On a *parameterized surface*, the curve obtained by setting one of the parameters equal to a *constant* and letting the other vary is called a **parameter curve**.

> [!info]+ Families of parameterized curves
> If surface is parameterized by:
> $$
> \vec{r}(s, t) = f_{1}(s, t) \vec{i} + f_{2}(s, t) \vec{j} + f_{3}(s, t) \vec{k},
> $$
> there are *two* families of parameter curves on the surface: one family with $t$ constant and the other with $s$ constant.

> [!example]+ Example 8
> Consider the vertical cylinder:
> $$x = \cos t, \quad y = \sin t, \quad z = z.$$
>
> 1. Describe the two parameter curves through the point $(0, 1, 1)$.
> 2. Describe the family for parameter curves with $t$ constant and the family with $z$ constant
>
> > [!success]+ 1
> > - Point $(0, 1, 1)$ corresponds to the parameter values $t = \frac{\pi}{2}$ and $z = 1$
> > - → There are two parameter curves
> >     - One with $t = \frac{\pi}{2}$
> >     - One with $z = 1$
> > - Parameter curve with $t = \frac{\pi}{2}$ has the parametric equations $$x = \cos\left( \frac{\pi}{2} \right) = 0, \quad y = \sin \left( \frac{\pi}{2} \right) = 1, z = z,$$with parameter $z$
> >     - $ Line through the point $(0, 1, 1)$ parallel to the $z$-axis
> > - Parameter curve with $z = 1$ has parametric equations $$x = \cos t, \quad y = \sin t, \quad z = 1,$$with parameter $t$
> >     - $ Unit circle parallel to and one unit above the $xy$-plane centered on the $z$-axis
>
> > [!check]+ 2
> > - Fix $t = t_{0}$ for $t$ and let $z$ vary
> >     - Curves parameterized by $z$ have equations $$x = \cos t_{0}, \quad y = \sin t_{0}, z=z.$$
> >         - $ Vertical lines on the cylinder parallel to the $z$-axis
> >             - ![](https://i.imgur.com/7b5hkPA.png)
> >             *Figure 21.12: The family of parameter curves with $t = t_{0}$ for the cylinder $x = \cos t, y = \sin t, z = z$*
> > - Fix $z = z_{0}$ and let $t$ vary
> >     - Curves in this family are parameterized by $t$ and have equations $$x = \cos t, \quad y = \sin t, \quad z = z_{0}$$
> >         - $ Circles of radius 1 parallel to the $xy$-plane centered on the $z$-axis
> >             - ![](https://i.imgur.com/lUhYy9N.png)
> >               *Figure 21.13: The family of parameter curves with $z = z_{0}$ for the cylinder $x = \cos t, y = \sin t, z = z$*

> [!example]+ Example 9
> Describe the families of parameter curves with $\theta = \theta_{0}$ and $\phi = \phi_{0}$ for the sphere
> $$x = \sin \phi \cos \theta, \quad y = \sin \phi \sin \theta, \quad z = \cos \phi,$$
> where $0 \leq \theta \leq 2\pi, 0 \leq \phi \leq \pi$.
>
> > [!check]+ Solution
> > - $\phi$ measures latitude
> >     - → Family with $\phi$ constant consists of circles of constant latitude
> >         - ![](https://i.imgur.com/ZVpytPc.png)
> >           *Figure 21.14: The family of parameter curves with $ϕ = \phi_{0}$ for the sphere parameterized by $(θ, ϕ)$*
> > - Family with $\theta$ constant consists of the meridians (*semicircles*) running between the north and south poles
> >     - ![](https://i.imgur.com/kO3w71S.png)
> >       *Figure 21.15: The family of parameter curves with $θ = \theta_{0}$ for the sphere parameterized by $(θ, ϕ)$*

## Summary

- **Parameterized surfaces**
    - In general:
        - Express coordinates $(x, y, z)$ of a point on a *surface* $S$ in terms of *two* parameters, $s$ and $t$:$$x = f_{1}(s, t), \quad y = f_{2}(s, t), \quad z = f_{3}(s, t)$$
- Using the **position vector**:
- Can express a parameterization for a general surface $S$ as:$$\vec{r}(s, t) = f_{1}(s, t) \vec{i} + f_{2}(s, t) \vec{j} + f_{3}(s, t) \vec{k}$$

> [!example]+ Examples of parameterized surfaces
> - **Surface of the form $z = f(x, y)$**
>     - Can be parameterized by $x = s, y = t, z = f(s, t)$
> - **Plane** through the point with position vector $\vec{r}_{0}$ and containing the two nonparallel vectors $\vec{v}_{1}, \vec{v}_{2}$
>     - Can be parameterized by $\vec{r}(s, t) = \vec{r}_{0} + s\vec{v}_{1} + t\vec{v}_{2}$
> - **Cylinder** with radius 1 centered around the origin
>     - Can be parameterized by $x = \cos t, y = \sin t, z = z$
>         - $0 \leq t \leq 2\pi$ and $-\infty < z < \infty$
> - **Sphere** with radius 1 centered at origin
>     - Can be parameterized by $x = \sin \phi \cos \theta, y = \sin \phi \sin \theta, z = \cos \phi$
>         - $0 \leq \theta \leq 2\pi$ and $0 \leq \phi \leq \pi$

- **Parameter curves**
    - Curve obtained by setting one of the parameters equal to a constant and letting other vary
