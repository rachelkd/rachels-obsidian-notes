---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
Module:
  - "[[5 - Double and Triple Integrals]]"
Week: 13
Lecture:
  - "35"
  - "36"
Chapter: "16.2"
Slides/Notes: 
Date: ""
aliases: []
Date created: Mon., Jan. 6, 2025, 12:22:58 am
Date modified: Tue., Jan. 14, 2025, 11:28:39 pm
---

# Iterated Integrals

## Expressing a Double Integral as an Iterated Integral

> [!thm]+ Writing a Double Integral as an Iterated Integral
> If $R$ is the rectangle $a \leq x b, c \leq y \leq d$ and $f$ is a continuous function on $R$, then the integral of $f$ over $R$ exists and is equal to the **iterated integral**
>
> $$
> \int_{R} f \, dA = \int_{y=c}^{y=d} \left( \int_{x=a}^{x=b} f(x, y) \, dx  \right)  \, dy,
> $$
>
> which can be written $\int_{c}^{d} \int_{a}^{b} f(x, y) \, dx \, dy$.

- To evaluate:
    - First perform the inside integral with respect to $x$, holding $y$ constant, then
    - Integrate result with respect to $y$

> [!example]+ A building is 8 meters wide and 16 meters long. It has a flat roof that is 12 meters high at one corner and 10 meters high at each of the adjacent corners. What is the volume of the building?
>
> - Put the high corner on the $z$-axis, the long side along the $y$-axis, and the short side along the $x$-axis
> - Roof is a plane with $z$-intercept 12 and $x$ slope $\frac{-2}{8}=-\frac{1}{4}$ and $y$ slope $\frac{-2}{16} = -\frac{1}{8}$
> $\implies$ Equation of the roof is
>
> $$
> z = 12 - \frac{1}{4}x - \frac{1}{8}y
> $$
> Volume is given by the double integral
>
> $$
> \int_{R} \left( 12 - \frac{1}{4}x - \frac{1}{8}y \right) \, dA
> $$
> where $R$ is the rectangle $0 \leq x \leq 8, 0 \leq y \leq 16$.
>
> Setting up an iterated integral, we get
> $$
> \text{Volume} = \int_{0}^{16} \int_{0}^{8} \left( 12 - \frac{1}{4}x - \frac{1}{8}y \right) \, dx  \, dy
> $$
> The inside integral evaluates to
> $$
> \int_{0}^{8} \left( 12 - \frac{1}{4}x - \frac{1}{8}y \right) \, dx = \left.\left( 12 - \frac{1}{8}x^{2} - \frac{1}{8}xy  \right)\right|_{x = 0}^{x = 8} = 88 - y.
> $$
> The outside integral then evaluates to
> $$
> \text{Volume} = \int_{0}^{16} (88 - y) \, dy = \left.\left( 88y - \frac{1}{2} y^{2} \right)\right|_{0}^{16} = 1280.
> $$
> So, the volume of the building is 1280 cubic meters.

![|464](https://i.imgur.com/04Im0RQ.png)

![|500](https://i.imgur.com/ThxpQsI.png)

- Notice that the inner integral $\int_{0}^{8} \left( 12 - \frac{1}{4}x - \frac{1}{8}y \right) \, dx$ gives the ==area of the cross section== of the building perpendicular to the $y$-axis in Figure 16.12
- Iterated integral $\int_{0}^{16} \int_{0}^{8} \left( 12 - \frac{1}{4}x - \frac{1}{8}y \right) \, dx \, dy$ calculates the *volume* by adding the volumes of thin cross-sectional slabs

## Order of Integration

> [!obs]+ For any function we are likely to meet, it does not matter in which order we integrate over a rectangular region $R$
> $$
> \int_{R} f \, dA = \int_{c}^{d} \left( \int_{a}^{b} f(x,y) \, dx  \right) \, dy = \int_{a}^{b} \left( \int_{c}^{d} f(x, y) \, dy \right)  \, dx
> $$

## Iterated Integrals Over Non-Rectangular Regions

> [!thm]+ Theorem. Two cases
>
> > [!summary]+ Case I.
> > Suppose $R$ consists of the points $(x, y)$ such that $a \leq x \leq b$ and $g_{1}(x) \leq y \leq g_{2}(x)$ for $g_{1}, g_{2}$ continuous on $[a, b]$.
> > If $f$ is continuous on $R$, then
> > $$
> > \int \int_{R} f(x, y) \, dA = \int_{a}^{b} \int_{g_{1}(x)}^{g_{2}(x)} f(x,y) \, dy  \, dx 
> > $$
>
> > [!summary]+ Case II.
> > Suppose $R$ consists of the points $(x, y)$ such that $g_{1}(y) \leq x \leq g_{2}(y)$ and $c \leq y \leq d$ for $g_{1}, g_{2}$ continuous on $[c, d]$.
> > If $f$ is continuous on $R$, then
> > $$\int \int_{R} f(x, y) \, dA = \int_{c}^{d} \int_{g_{1}(y)}^{g_{2}(y)} f(x, y) \, dx  \, dy $$

> [!example]+ The density at the point $(x, y)$ of a triangular metal plate is $\delta(x, y)$. Express its mass as an iterated integral.
> ![|500](https://i.imgur.com/ybSNSez.png)
>
> - Approximate the triangular region using a grid of small rectangles of sides $\Delta x$ and $\Delta y$
>
> $$
> \text{Mass of rectangle} \approx \text{Density} \cdot \text{Area} \approx \delta(x, y) \Delta x \Delta y
> $$
>
> Summing over all rectangles gives a Riemann sum which approximates the double integral:
>
> $$
> \text{Mass} = \int_{R} \delta(x, y) \, dA,
> $$
> where $R$ is the triangle.
>
> > [!obs]+ Iterated integral for rectangles
> >
> > - Inside integral with respect to $y$ is along vertical strips which begin at the horizontal line $y = c$ and end at the line $y = d$
> > - One such strip for each $x$ between $x = a$ and $x = b$
> >
> > ![|500](https://i.imgur.com/AJKjBzh.png)
> >
> > - Idea is the same for triangular region
> > - Only difference:
> >     - Individual strips do not go from $y = c$ to $y = d$
> >     - Vertical strip that starts at the point $(x, 0)$ ends at the point $(x, 2 - 2x)$
> >         - Top edge of the triangle is the line $y = 2 - 2x$
> >
> > ![|500](https://i.imgur.com/Tmz2lG0.png)
>
> $\implies$ On this vertical strip, $y$ goes from $0$ to $2 - 2x$
> $\implies$ Inside integral is
>
> $$
> \int_{0}^{2 - 2x} \delta(x, y) \, dy
> $$
>
> - There is a vertical trip for each $x$ between 0 and 1
>     - $\implies$ Outside integral goes from $x = 0$ to $x = 1$
>
> Iterated integral is
>
> $$
> \text{Mass} = \int_{0}^{1} \int_{0}^{2 - 2x} \delta(x, y) \, dy  \, dx
> $$
>
> > [!attention]+ Integrating in opposite order: fix $y$ in the inner integral instead of $x$
> >
> > - Limits are formed by looking at *horizontal* strips instead of vertical
> > - Express $x$-values at the end points in terms of $y$
> >     - Use equation of the top edge of the triangle: $x = 1 - \frac{1}{2}y$
> > - $\implies$ Horizontal strip goes from $x = 0$ to $x = 1 - \frac{1}{2}y$
> > - Strip exists for every $y$ from 0 to 2
> >
> > ![|500](https://i.imgur.com/XycaLuU.png)
> >
> > So, the iterated integral is
> >
> > $$
> > \text{Mass} = \int_{0}^{2} \int_{0}^{1 - \frac{1}{2}y} \delta(x, y) \, dx  \, dy
> > $$

### Limits on Iterated Integrals

1. Limits on the outer integral must be constants
2. Limits on the inner integral can involve only the variable in the outer integral
    - e.g., If inner integral is with respect to $x$, its limits can be functions of $y$

> [!example]+ Find the mass $M$ of a metal plate $R$ bounded by $y = x$ and $y = x^{2}$ with density given by $\delta(x, y) = 1 + xy$ kg/meter$^{2}$.
> ![|500](https://i.imgur.com/nILetnL.png)
>
> The mass is given by
> $$
> M = \int_{R} \delta(x, y) \, dA.
> $$
>
> - Integrate along vertical strips → Do $y$ integral first
>     - Goes from the bottom boundary $y = x^{2}$ to top boundary $y = x$
>     - Left edge of the region is at $x = 0$
>     - Right edge is at intersection point of $y = x$ and $y = x^{2}$, which is $(1, 1)$
>     - $\implies$ $x$-coordinate of the vertical strips can vary from $x = 0$ to $x = 1$
>
> $$
> \begin{align}
> M = \int_{0}^{1} \int_{x^{2}}^{x} \delta(x, y) \, dy  \, dx &= \int_{0}^{1} \int_{x^{2}}^{x} (1 + xy) \, dy  \, dx  \\
>  & = \int_{0}^{1} \left.\left( y + x \frac{y^{2}}{2} \right)\right|_{y = x^{2}}^{y = x} \, dx  \\
>  & = \int_{0}^{1} \left( x - x^{2} + \frac{x^{3}}{2} - \frac{x^{5}}{2} \right) \, dx  \\
>  & = \left.\left( \frac{x^{2}}{2} - \frac{x^{3}}{3} + \frac{x^{4}}{8} - \frac{x^{6}}{12} \right)\right|_{0}^{1}  \\
>  & = \frac{5}{24} \\
>  & = 0.208 \text{ kg}
> \end{align}
> $$

> [!example]+ A semicircular city of radius 3 km borders the ocean on the straight side. Find the average distance from points in the city to the ocean.
>
> - Think of the ocean as everything below the x-axis in the xy-plane
> - City is the upper half of the circular disk of radius 3 bounded by $x^2 + y^2 = 9$
>
> ![|500](https://i.imgur.com/Kff6l8d.png)
>
> - Distance from any point $(x,y)$ in the city to the ocean is the vertical distance to the $x$-axis, namely $y$
> - Want to compute:
>
> $$
> \text{Average distance} = \frac{1}{\text{Area}(R)} \int_R y \, dA
> $$
>
> where $R$ is the region between the upper half of the circle $x^2 + y^2 = 9$ and the $x$-axis
>
> - Area of $R$ is $\frac{\pi 3^2}{2} = 9\pi/2$
>
> Computing with inner integral with respect to $y$:
>
> - Vertical strip goes from $y=0$ to $y=\sqrt{9-x^2}$
> - Strip exists for every $x$ from $-3$ to $3$
>
> $$
> \begin{align}
> \int_R y \, dA &= \int_{-3}^3 \int_0^{\sqrt{9-x^2}} y \, dy \, dx \\
> &= \int_{-3}^3 \frac{1}{2}(9-x^2) \, dx \\
> &= \frac{1}{2}\left(9x - \frac{x^3}{3}\right)\Bigg|_{-3}^3 \\
> &= \frac{1}{2}(18-(-18)) = 18
> \end{align}
> $$
>
> Therefore, average distance = $18/(9\pi/2) = 4/\pi = 1.273$ km
>
> Alternative approach using inner integral with respect to $x$:
>
> - Horizontal strips go from $x=-\sqrt{9-y^2}$ to $x=\sqrt{9-y^2}$
> - Strip exists for every $y$ from $0$ to $3$
>
> $$
> \begin{align}
> \int_R y \, dA &= \int_0^3 \int_{-\sqrt{9-y^2}}^{\sqrt{9-y^2}} y \, dx \, dy \\
> &= \int_0^3 2y\sqrt{9-y^2} \, dy \\
> &= -\frac{2}{3}(9-y^2)^{3/2}\big|_0^3 \\
> &= -\frac{2}{3}(0-27) = 18
> \end{align}
> $$
>
> We get the same result: average distance to the ocean is $4/\pi = 1.273$ km.

> [!example]+ Sketch the region of integration for the iterated integral $\int_0^6 \int_{x/3}^2 x\sqrt{y^2+1} \, dy \, dx$
>
> - Inner integral is with respect to $y$
> - Region built of vertical strips:
>     - Bottom of each strip is on line $y=x/3$
>     - Top is on horizontal line $y=2$
>     - Region contained between vertical lines $x=0$ and $x=6$
>     - Lines $y=2$ and $y=x/3$ meet at $x=6$
>
> ![|500](https://i.imgur.com/DBs03m0.png)

## Reversing the Order of Integration

> [!tip]+ Sometimes, it is helpful to reverse the order of integration in an iterated integral.
> - Integral which is difficult or impossible with the integration in one order can be straightforward in the other

> [!example]+ Evaluate $\int_{0}^{6} \int_{\frac{x}{3}}^{2} x\sqrt{ y^{3} + 1 } \, dy \, dx$ using the region sketched in Figure 16.19.
>
> - $\sqrt{ y^{3} + 1 }$ has no elementary antiderivative
>     - Cannot calculate inner integral symbolically
>     - → Try reversing order of integration
> - Horizontal strips go from $x = 0$ to $x = 3y$
> - Strip exists for every $y$ from 0 to 2
>
> $$
> \begin{align}
> \int_{0}^{6} \int_{\frac{x}{3}}^{2} x\sqrt{ y^{3} + 1 } \, dy \, dx & = \int_{0}^{2} \int_{0}^{3y} x \sqrt{ y^{3} + 1 } \, dx  \, dy  \\
>  & = \int_{0}^{2} \left.\left( \frac{x^{2}}{2} \sqrt{ y^{3} + 1 } \right)\right|_{x = 0}^{x = 3y} \, dy  \\
>  & = \int_{0}^{2} \frac{9y^{2}}{2}(y^{3} + 1)^{1/2} \, dy  \\
>  & = (y^{3} + 1)^{3/2}\big|_{0}^{2} \\
>  & = 27 - 1 = 26
> \end{align}
> $$

## Calculating the Volume in between Two Functions

> [!example]+ Set up an integral giving the volume between the plane $z = 4$ and the bowl $z = x^{2} + y^{2}$.
>
> Let $V$ be the volume of the solid bowl.
> $$
> \begin{align}
> V  & = \left( \text{volume under }z = 4 \text{ and above xy-plane} \right)  \\
>  & - \left( \text{volume under }z = x^{2}+y^{2} \text{ and above xy-plane} \right)  \\
>  & = \int \int _{R} 4 \, dA - \int \int_{R} x^{2}+y^{2} \, dA \\
>  & = \int \int_{R} 4 - (x^{2} + y^{2}) \, dA
> \end{align}
> $$
> ![|553](https://i.imgur.com/0akKxDs.png)
>
> Then,
> $$
> \implies \int_{-2}^{2} \int_{-\sqrt{ 4-x^{2} }}^{\sqrt{ 4-x^{2} }} 4 - (x^{2} + y^{2})  \, dy  \, dx 
> $$
