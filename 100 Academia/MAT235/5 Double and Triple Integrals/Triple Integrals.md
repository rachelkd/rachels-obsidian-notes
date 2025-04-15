---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 14
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "37"
  - "38"
Chapter: "16.3"
Slides/Notes:
  - https://share.goodnotes.com/s/ZGEJdFGIYmyfezG09OYxxa
Date: 2025-01-13
Date created: Mon., Jan. 13, 2025, 7:49:19 pm
Date modified: Mon., Feb. 24, 2025, 3:24:21 am
---

# Triple Integrals

- A continuous function of three variables can be integrated over a solid region $W$ in 3-space
    - In the same way as a function of two variables is integrated over a flat region in 2-space

<!-- break -->
- Subdivide $W$ into smaller regions
- Multiply volume of each region by a value of that function in that region
- Add the results
- Example:
    - If $W$ is the box $a \leq x \leq b, c \leq y \leq d, p \leq z \leq q$
    - Subdivide each side into $n, m, l$ pieces
    - → Chopping $W$ into $nml$ smaller boxes

![|361](https://i.imgur.com/WuMHpN8.png)

- The volume of each smaller box is $\Delta V = \Delta x \Delta y \Delta z$, where
    - $\Delta x = \frac{b-a}{n}$
    - $\Delta y = \frac{d-c}{m}$
    - $\Delta z = \frac{q-p}{l}$
- Using this subdivision, we pick a point $(u_{ijk},v_{ijk}, w_{ijk})$ in the $ijk$-th small box and construct a Riemann sum
    $$
    \sum\limits_{ijk} f(u_{ijk}, v_{ijk}, w_{ijk}) \Delta V
    $$
- If $f$ is continuous, as $\Delta x, \Delta y, \Delta z$ approach 0, this Riemann sum approaches the definite integral $\int_{W} f \, dV$ called a **triple integral**
    $$
    \int_{W} f \, dV = \lim_{ \Delta x, \Delta y, \Delta z \to 0 } \sum_{i,j,k} f(u_{ijk}, v_{ijk}, w_{ijk}) \Delta x \Delta y \Delta z
    $$

## Triple Integral as an Iterated Integral

See [[Iterated Integrals]].

> [!def]+ Triple integral as an iterated integral
> $$
> \int_{W} f \, dV = \int_{p}^{q} \left( \int_{c}^{d} \left( \int_{a}^{b} f(x,y,z) \, dx  \right)  \, dy  \right)  \, dz
> $$
> where $y$ and $z$ are treated as constants in the innermost ($dx$) integral, and $z$ is treated as a constant in the middle ($dy$) integral.
>
> - Five other orders of integration are possible

> [!info]+ Limits on Triple Integrals
>
> - Limits for *outer* integral are ==constants==
> - Limits for *middle* integral can involve only ==one== variable — that in the ==outer integral==
> - Limits for the inner integral can involve ==two== variables — those in the two outer integrals

From lecture:

> [!thm]+ Three cases
>
> > [!summary]+ I.
> > If $R$ consists of the points $(x,y,z)$ with $(x,y)$ in $D$ and $h_{1}(x,y) \leq z \leq h_{2}(x,y)$ for $h_{1}, h_{2}$ continuous on a bounded region $D$ in the plane, then
> > $$
> > \int \int \int_{R} f(x,y,z) \, dV = \int \int_{D} \int_{h_{1}(x,y)}^{h_{2}(x,y)} f(x,y,z) \, dz \, dA
> > $$
> >
> > > [!note]+ D is the projection of $R$ to the $xy$-plane
> > > ![|400](https://i.imgur.com/tg6S0wQ.png)
>
> > [!summary]+ II. Exchange roles of $x,z$ in I.
> > ![|400](https://i.imgur.com/Pbgfkv7.png)
> > Note: Mistake in drawing. $R$ should be the 3D region, and $D$ should be the projection on the $yz$-plane
>
> > [!summary]+ III. Exchange roles of $y, z$ in I.
> > ![|400](https://i.imgur.com/OWtOYOQ.png)

That is, if a solid region $W$ is *above* the graph of $z = g(x, y)$ and *below* $z = h(x, y)$ with $(x, y)$ lying inside the region $R$ in the $xy$-plane, then
$$
\int_{W} f \, dV = \int \int \int_{g(x, y)}^{h(x, y)} f(x,y,z) \, dz \, dy \, dz
$$
This is the definition given in the textbook.

> [!example]+ A cube $C$ has sides of length 4 cm and is made of a material of variable density. If one corner is at the origin and the adjacent corners are on the positive $x, y, z$ axes, then the density at the point $(x, y, z)$ is $\delta(x, y, z) = 1 + xyz$ gm/cm$^{3}$. Find the mass of the cube
>
> Consider a small piece $\Delta V$ of the cube, small enough so that the density remains close to constant over the piece. Then,
> $$
> \text{Mass of small piece} = \text{Density} \cdot \text{Volume} \approx \delta(x, y, z) \Delta V
> $$
> To get the total mass, we add the masses of the small pieces and take the limit as $\Delta V \to 0$. Thus, the mass is the triple integral
> $$
> \begin{align}
> M & = \int_{C} \delta \, dV = \int_{0}^{4} \int_{0}^{4} \int_{0}^{4} (1 + xyz) \, dx  \, dy  \, dz = \int_{0}^{4} \int_{0}^{4} \left.\left( x + \frac{1}{2}x^{2} yz \right)\right|_{x=0}^{x=4}  \, dy  \, dz \\
>  & = \int_{0}^{4} \int_{0}^{4} (4 + 8yz) \, dy \, dz = \int_{0}^{4} \left.(4y + 4y^{2}z)\right|_{y=0}^{y=4} \, dz = \int_{0}^{4} (16 + 64z) \, dz \\
>  & = 576 \text{ gm}
> \end{align}
> $$

> [!thm]+ Volume
>
> - The **volume** of the solid region $W$ is given by the triple integral
>
> $$
> \int_{W} 1 \, dV
> $$

> [!thm]+ Density
>
> - If $\rho(x,y,z)$ is **density**, then
>     $$
>     \int_{W}\rho\,dV
>     $$
>
> is the **total quantity** in the solid region $W$.
