---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 15
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "40"
Chapter: "16.5"
Slides/Notes: 
Date: 2025-01-20
Date created: Thu., Jan. 16, 2025, 8:00:20 pm
Date modified: Wed., Feb. 12, 2025, 9:34:08 pm
---

# Integrals in Cylindrical and Spherical Coordinates

## Cylindrical Coordinates

- Cylindrical coordinates of a point $(x,y,z)$ in 3-space are obtained by:
    - Representing the ==$x, y$ coordinates== in ==polar== coordinates
    - Letting the ==$z$-coordinate== be the $z$-coordinate of the ==Cartesian== coordinate system

> [!thm]+ Relation between Cartesian and cylindrical coordinates
> Each point in 3-space is represented using:
>
> - $0 \leq r < \infty$
> - $0 \leq \theta \leq 2\pi$
> - $-\infty < z < \infty$
>
> $$
> \begin{align}
> x & = r \cos \theta, \\
> y & = r \sin \theta, \\
> z & = z
> \end{align}
> $$
> - $x^{2} + y^{2} = r^{2}$
>     - As with [[Double Integrals in Polar Coordinates|polar coordinates]]
>
> ![|center|300](https://i.imgur.com/eC0yCu1.png)

### Fundamental Surfaces

To visualize cylindrical coordinates, sketch the surfaces obtained by setting one of the coordinates equal to a constant.

- Setting $r = c$ gives a ==cylinder== around the $z$-axis whose radius is $c$.
    - ![|300](https://i.imgur.com/YSoiBbx.png)
- Setting $\theta = c$ gives a ==half-plane== perpendicular to the $xy$-plane, with one edge along the $z$-axis, making an angle $c$ with the axis.
    - ![|300](https://i.imgur.com/R5H06Bn.png)
- Setting $z = c$ gives a horizontal plane $|c|$ units from the $xy$-plane.
    - ![|300](https://i.imgur.com/e3o5ILa.png)

> [!def] These are **fundamental surfaces**.

- Regions that can most easily be described in cylindrical coordinates are:
    - Regions whose boundaries are such fundamental surfaces
    - e.g., Vertical cylinders, or wedge-shaped parts of vertical cylinders

### Integration in Cylindrical Coordinates

- Recall in polar coordinates:
    - To evaluate $\int_{R} f \, dA$:
        - Had to express area element $dA$ in terms of polar coordinates
        - $dA = r\,dr\,d\theta$
- To evaluate triple integral $\int_{W} f \, dV$ in cylindrical coordinates:
    - Need to express volume element $dV$ in cylindrical coordinates

![|300](https://i.imgur.com/WZ2g4Hj.png) ![|300](https://i.imgur.com/L598Pwr.png)

- Area of the base is $\Delta A \approx r \, \Delta r \, \Delta\theta$
- Height is $\Delta z$
- $\implies$ Volume is given by $\Delta V \approx r \, \Delta r \Delta \theta \, \Delta z$

> [!tip]+ Computing integrals in cylindrical coordinates
> $$dV = r\,dr\,d\theta\,dz$$

- Other orders of integration are also possible

### Examples

> [!example]+ Find the mass of the wedge of cheese cut from a cylinder 4 cm high and 6 cm in radius; this wedge subtends an angle of $\frac{\pi}{6}$ at the center. Its density is 1.2 grams/cm$^{3}$.
> Wedge is described by the following inequalities:
> - $0 \leq r \leq 6$
> - $0 \leq z \leq 4$
> - $0 \leq \theta \leq \frac{\pi}{6}$
> If wedge is $W$, its mass is
> $$
> \int_{W} 1.2 \, dV.
> $$
> In cylindrical coordinates:
> $$
> \begin{align}
> \int_{W} 1.2 \, dV  & = \int_{0}^{4} \int_{0}^{\pi/6} \int_{0}^{6} 1.2 r \, dr  \, d\theta  \, dz \\
>  & = \int_{0}^{4} \int_{0}^{\pi/6} \left. 0.6r^{2} \right| _{0}^{6} \, d\theta  \, dz \\
>  & = 21.6 \int_{0}^{4} \int_{0}^{\pi/6}  \, d\theta  \, dz \\
>  & = 21.6\left( \frac{\pi}{6} \right)4 \\
>  & = 45.239 \text{ grams}
> \end{align}
> $$

> [!example]+ A water tank in the shape of a hemisphere has radius $a$; its base is its plane face. Find the volume $V$ of water in the tank as a function of $h$, the depth of the water.
>
> - In Cartesian coordinates:
>     - Sphere of radius $a$ has equation $x^{2} + y^{2} + z^{2} = a^{2}$
>     - $r^{2} = x^{2} + y^{2} \implies \bbox[ pink ]{ r^{2} + z^{2} = a^{2} }$
> - If we want to describe the amount of water in the tank in cylindrical coordinates:
>     - $0 \leq r \leq \sqrt{ a^{2} - z^{2} }$
>     - $0 \leq \theta \leq 2\pi$
>     - $0 \leq z \leq h$
>
> $$
> \begin{align}
> \text{Volume of water} = \int_{W} 1\,dV  & = \int_{0}^{2\pi} \int_{0}^{h} \int_{0}^{\sqrt{ a^{2} - z^{2} }} r \, dr  \, dz \, d\theta  \\
>  & = \int_{0}^{2\pi} \int_{0}^{h} \left. \frac{r^{2}}{2} \right|_{r = 0}^{r = \sqrt{ a^{2} - z^{2} }}  \, dz  \, d\theta \\
>  & = \int_{0}^{2\pi} \int_{0}^{h} \frac{1}{2}\left( a^{2} - z^{2} \right)  \, dz  \, d\theta \\
>  & = \int_{0}^{2\pi} \left. \frac{1}{2}\left( a^{2}z - \frac{z^{3}}{3}\right) \right| _{z = 0}^{x=h} \, d\theta \\
>  & = \int_{0}^{2\pi} \frac{1}{2}\left( a^{2}h - \frac{h^{3}}{3} \right) \, d\theta \\
>  & = \pi \left( a^{2}h - \frac{h^{3}}{3} \right)  
> \end{align}
> $$
>
> ![](https://i.imgur.com/FAWHxTI.png)

## Spherical Coordinates

![|400](https://i.imgur.com/mHqMOYr.png)

- Cartesian coordinates: $P = (x, y , z)$

> [!thm]+ We define spherical coordinates $(\rho, \phi, \theta)$ for $P$ as follows:
> - $\rho = \sqrt{ x^{2} + y^{2} + z^{2} }$
>     - Distance of $P$ from the origin
> - $\phi$
>     - Angle between the positive $z$-axis and the ==positive $z$-axis== and the line through the origin and the point $P$
> - $\theta$
>     - Angle of the positive $x$-axis in the direction of the positive $y$-axis
>     - Same in cylindrical coordinates

Recall cylindrical coordinates:
$$
\begin{align}
x = r \cos \theta, && y = r \sin \theta, && z = z
\end{align}
$$
From Figure 16.47, we derive the following:
$$
\begin{align}
z = \rho \cos \phi, && r = \rho \sin \phi
\end{align}
$$

> [!thm]+ Relation Between Cartesian and Spherical Coordinates
> Each point in 3-space is represented using:
> - $0 \leq \rho < \infty$
> - $0 \leq \phi \leq \pi$
> - $0 \leq \theta \leq 2\pi$
>
> $$
> \begin{align}
> x & = \rho \sin \phi \cos \theta \\
> y & = \rho \sin \phi \sin \theta \\
> z & = \rho \cos \phi \\
> \rho^{2} & = x^{2} + y^{2} + z^{2}
> \end{align}
> $$

> [!tip]+ Spherical coordinates are useful when there is spherical symmetry with respect to the origin
> - Either in the region of integration or in the integrand

- **Fundamental surfaces**:
    - $\rho = k$
        - Sphere of radius $k$ centred at the origin
        - ![|300](https://i.imgur.com/eWhS5SA.png)
    - $\theta = k$
        - Half-plane with its edge along the $z$-axis
        - ![|300](https://i.imgur.com/Zv3v0n7.png)
    - $\phi = k$
        - Cone if $k \neq \frac{\pi}{2}$
        - $xy$-plane if $k = \frac{\pi}{2}$
        - ![|300](https://i.imgur.com/2NYZpj4.png)

### Integration in Spherical Coordinates

> [!goal]+ We need to express the volume element, $dV$, in spherical coordinates
> - To use spherical coordinates in triple integrals

- $ Volume element can be approximated by a ==box with curved edges==
    - One edge has length $\Delta p$
    - Edge parallel to the $xy$-plane is an arc of a circle made from rotating the cylindrical radius $r = \rho \sin \phi$ through an angle $\Delta\theta$
        - Edge has length $\rho \sin \phi \Delta\theta$
    - Remaining edge comes from rotating the radius $\rho$ through an angle $\Delta \phi$
        - Has length $\rho \Delta \phi$
- $\implies \Delta V \approx \Delta \rho(\rho\Delta \phi)(\rho \sin \phi \Delta \theta) = \rho^{2} \sin \phi \Delta \rho \Delta \phi \Delta \theta$

![|404](https://i.imgur.com/KTtqrJL.png)

> [!tip]+  Computing integrals in spherical coordinates
> $$
> dV = \rho^{2} \sin \phi \,dp\,d\phi\,d\theta
> $$

- Other orders of integration are possible

### Examples

> [!example]+ Use spherical coordinates to derive the formula for the volume of a ball of radius $a$.
> In spherical coordinates, a ball of radius $a$ is described by the inequalities:
>
> - $0 \leq \phi \leq a$
> - $0 \leq \phi \leq \pi$
> - $0 \leq \theta \leq 2\pi$
>
> $$
> \begin{align}
> \text{Volume} = \int_{R} 1 \, dV & = \int_{0}^{2\pi} \int_{0}^{\pi} \int_{0}^{a} \rho^{2} \sin \phi \, d\rho  \, d\phi  \, d\theta \\
>   & = \int_{0}^{2\pi} \int_{0}^{\pi} \frac{1}{3}a^{3}\sin \phi \, d\phi  \, d\theta \\
>   & = \frac{1}{3}a^{3} \int_{0}^{2\pi} \left. -\cos \phi \right| _{0}^{\pi} \, d\theta \\
>   & = \frac{2}{3}a^{3} \int_{0}^{2\pi}  \, d\theta \\
>   & = \frac{4\pi a^{3}}{3}
> \end{align}
> $$
