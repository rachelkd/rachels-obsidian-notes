---
dg-publish: false
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 17
Module:
  - "[[6 - Line Integrals and the Fundamental Theorem of Calculus]]"
Lecture:
  - "47"
  - "48"
Chapter: "18.1"
Slides/Notes: 
Date: 2025-02-05
Date created: Mon., Feb. 17, 2025, 7:05:21 pm
Date modified: Mon., Feb. 24, 2025, 2:41:11 pm
---

# The Idea of a Line Integral

> [!idea]+ The line integral, defined in this section, measures the extent to which a curve in a vector field is, overall, going with the vector field or against it.
> - Imagine rowing on a river with a noticeable current
>     - At times: Might be working against the current
>     - At other times: May be moving with current
>     - At the end: Have a sense of whether, overall, you were helped or hindered by the current

## Orientation of a Curve

- A curve can be traced out in *two directions*
- & Need to choose one direction before we can define a line integral

![](https://i.imgur.com/RPn2498.png)
*Figure 18.1: A curve with two different orientations represented by arrowheads*

> [!def]+ Orientation
> A curve is said to be **oriented** if we have ==chosen a direction== of travel on it.

## Definition of the Line Integral

Consider a vector field $\vec{F}$ and an oriented curve $C$.

- Divide $C$ into $n$ small, almost straight pieces along which $\vec{F}$ is approximately constant
    - Each piece can be represented by a *displacement vector* $\Delta \overrightarrow{r_{i}} = \overrightarrow{r_{i + 1}} - \overrightarrow{r_{i}}$
    - Value of $\vec{F}$ at each point of this small piece of $C$ is approximately $\vec{F}(\vec{r_{i}})$

![](https://i.imgur.com/nz71KrY.png)
*Figure 18.2: The curve $C$, oriented from $P$ to $Q$, approximated by straight line segments represented by displacement vectors $\Delta \overrightarrow{r_{i}} = \overrightarrow{r_{i + 1}}$*

![](https://i.imgur.com/CXo3YsA.png)
*Figure 18.3: The vector field $\vec{F}$ evaluated at the points with position vector $\vec{r}_{i}$ on the curve $C$ oriented from $P$ to $Q$*

### Example: Current

Returning to our original example of a person rowing against or with a current:

- Vector field $\vec{F}$ represents the current
- Oriented curve $C$ is the path of the person rowing the boat

> [!goal] We wish to determine to what extent the vector field $\vec{F}$ helps or hinders motion along $C$

- & Dot product can be used to measure to what extent two vectors point in the same or opposing directions
    - Form the dot product $\vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}$ for each point
        - With position vector $\vec{r}_{i}$ on $C$
- Summing over all pieces, we get a Riemann sum

$$
\sum\limits_{i=0}^{n - 1}  \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}
$$

We define the line integral, written $\int_{C} \vec{F} \cdot d\vec{r}$, by taking the limit as $\| \Delta \vec{r}_{i} \| \to 0$.
Provided the limit exists, we make the following definition:

> [!def]+ Line integral
> The **line integral** of a vector field $\vec{F}$ along an oriented curve $C$ is
> $$
> \int_{C} \vec{F} \cdot d\vec{r} = \lim_{ \| \Delta \vec{r}_{i} \| \to 0 } \sum\limits_{i=0}^{n - 1} \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}
> $$

### How Does the Limit Defining a Line Integral Work?

> [!important]+ The limit in the definition of a line integral exists if…
> - $\vec{F}$ is continuous on the curve $C$, and
> - $C$ is made by joining end to end a *finite* number of smooth curves
> - % A vector field $\vec{F} = F_{1} \vec{i} + F_{2} \vec{j} + F_{3} \vec{k}$ is *continuous* if $F_{1}, F_{2}, F_{3}$ are continuous and a *smooth curve* is one that can be parameterized by smooth functions

- We subdivide the curve using a ==parameterization==
    - That goes from one end of the curve to the other
    - In the forward direction
    - Without retracing any portion of the curve
- A subdivision of the parameter interval gives a subdivision of the curve

> [!info] All the curves we consider in this course are **piecewise smooth**

> [!example]+ Example 1
> Find the line integral of the constant vector field $\vec{F} = \vec{i} + 2\vec{j}$ along the path from $(1 ,1)$ to $(10, 10)$ as shown in Figure 18.4.
>
> ![](https://i.imgur.com/v6KFoVy.png)
> *Figure 18.4: The constant vector field $\vec{F} = \vec{i} + 2 \vec{j}$ and the path from $(1, 1)$ to $(10, 10)$*
>
> > [!check]- Solution
> > Let $C_{1}$ be the horizontal segment of the path going from $(1, 1)$ to $(10, 1)$.
> >
> > When we break this path into pieces:
> >
> > - Each piece $\Delta \vec{r}$ is horizontal
> >     - $\implies \Delta \vec{r} = \Delta x \vec{i}$
> > - $\vec{F} \cdot \Delta \vec{r} = (\vec{i} + 2 \vec{j}) \cdot \Delta x \vec{i} = \Delta x$
> >
> > $$
> > \int_{C_{1}} \vec{F} \cdot d\vec{r} = \int_{x = 1}^{x = 10}  \, dx = 9
> > $$
> >
> > Along the vertical segment $C_{2}$:
> >
> > - $\Delta \vec{r} = \Delta y \vec{j}$
> > - $\vec{F} \cdot \Delta \vec{r} = (\vec{i} + 2\vec{j}) \cdot \Delta y \vec{j} = 2 \Delta y$
> >
> > $$
> > \int_{C_{2}} \vec{F} \cdot d \vec{r} = \int_{y = 1}^{y = 10} 2 \, dy = 18 
> > $$
> >
> > Then, we get:
> > $$
> > \int_{C} \vec{F} \cdot d\vec{r} = \int_{C_{1}} \vec{F} \cdot d\vec{r} + \int_{C_{2}} \vec{F} \cdot d\vec{r} = 9 + 18 = 27
> > $$

## What Does the Line Integral Tell Us?

Recall:

- For any two vectors $\vec{u}, \vec{v}$, the dot product of $\vec{u} \cdot \vec{v}$ is *positive* if $\vec{u}$ and $\vec{v}$ point roughly in the same direction
    - i.e., the angle between them is less than $\pi / 2$
- The dot product is *zero* if $\vec{u}, \vec{v}$ are perpendicular
- Dot product is *negative* if they point roughly in opposite directions
    - i.e., the angle between them is greater than $\pi / 2$

<!-- break -->
- & Line integral of $\vec{F}$ adds the dot products of $\vec{F}$ and $\Delta \vec{r}$ along the path
- If $\| \vec{F} \|$ is constant:
    - Line integral gives a *positive* number if $\vec{F}$ is mostly pointing in the ==same== direction as $C$
    - *Negative* if $\vec{F}$ is mostly pointing in the ==opposite== direction to $C$
    - *Zero* if $\vec{F}$ is ==perpendicular== to the path at all points or positive and negative contributions ==cancel== out

> [!info] In general, the line integral of a vector field $\vec{F}$ along a curve $C$ measures the extent to which $C$ is going with $\vec{F}$ or against it.

> [!example]+ Example 2
> The vector field $\vec{F}$ and the oriented curves $C_{1}, C_{2}, C_{3}, C_{4}$ are shown below.
>
> ![](https://i.imgur.com/IuK3rBh.png)
>
> The curves $C_{1}$ and $C_{3}$ are the same length.
> Which of the line integrals $\int_{C_{1}} \vec{F} \cdot d\vec{r}$, for $i = 1,2,3,4$ are positive? Which are negative? Arrange these line integrals in ascending order.
>
> > [!check]- Solution
> >
> > - Vector field $\vec{F}$ and line segments $\Delta \vec{r}$ are approximately parallel and in the same direction for curves $C_{1}, C_{2}, C_{3}$
> >     - Contributions of each term $\vec{F} \cdot \Delta \vec{r}$ are positive for these curves
> > - $\implies \int_{C_{1}} \vec{F} \cdot d\vec{r}, \int_{C_{2}} \vec{F} \cdot d\vec{r}, \int_{C_{3}} \vec{F} \cdot d\vec{r}$ are each *positive*
> > - For curve $C_{4}$:
> >     - Vector field and line segments are in opposite directions
> >         - Each term $\vec{F} \cdot \Delta \vec{r}$ is negative
> >     - $\implies \int_{C_{4}} \vec{F} \cdot d\vec{r}$ is *negative*
> >
> > <!-- break -->
> > - Magnitude of vector field is smaller along $C_{1}$ than along $C_{3}$ and two curves are the same length
> >     - $$\int_{C_{1}} \vec{F} \cdot d\vec{r} < \int_{C_{3}} \vec{F} \cdot d\vec{r}$$
> > - Magnitude of vector field is the same along $C_{2}$ and $C_{3}$, but curve $C_{2}$ is longer than curve $C_{3}$
> >     - $$\int_{C_{3}} \vec{F} \cdot d\vec{r} < \int_{C_{2}} \vec{F} \cdot d \vec{r}$$
> >
> > Since $\int_{C_{4}} \vec{F} \cdot d\vec{r}$ is negative, we have
> > $$
> > \int_{C_{4}} \vec{F} \cdot d\vec{r} < \int_{C_{1}} \vec{F} \cdot d\vec{r} < \int_{C_{3}} \vec{F} \cdot d\vec{r} < \int_{C_{2}} \vec{F} \cdot d\vec{r}
> > $$

## Interpretations of the Line Integral

### Work

Recall from [[Dot Product|13.3]]:

- If a constant force $\vec{F}$ acts on an object while it moves along a straight line through a displacement $\vec{d}$:
    - Work done by force on the object is $$\text{Work done} = \vec{F} \cdot \vec{d}$$

Suppose we want to find the work done by gravity on an object moving far above the surface of the earth.

- Force of gravity varies with distance from the earth and path may not be straight
    - Cannot use the formula $\vec{F} \cdot \vec{d}$
- & Approximate path by line segments which are small enough that force is approximately constant on each one

Suppose force at a point with position vector $\vec{r}$ is $\vec{F}(\vec{r})$. Then,
$$
\text{Work done by force } \vec{F}(\vec{r}_{i}) \text{ over small displacement } \Delta \vec{r}_{i} \approx \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i},
$$
and so,
$$
\text{Total work done by force along oriented curve }C \approx \sum_{i} \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}.
$$
Taking the limit as $\| \Delta \vec{r}_{i} \| \to 0$, we get

> [!thm]+ Work done by force $\vec{F}(\vec{r})$ along curve $C$
> $$
> \lim_{ \| \Delta \vec{r}_{i} \| \to 0 } \sum_{i} \vec{F}(\vec{r}_{i}) \cdot \Delta \vec{r}_{i} = \int_{C} \vec{F} \cdot d\vec{r}
> $$

> [!example]+ Example 3
> ![](https://i.imgur.com/aubPnzi.png)
>
> > [!check]- Solution
> > - The path from $x = 20$ to $x = 0$ is divided as shown in Figure 18.7, with a typical segment represented by $$ \Delta \vec{r} = \Delta x\vec{i} $$
> > - Moving from $x = 20$ to $x=0$
> >     - $\implies$ Quantity $\Delta x$ will be negative
> >
> > Work done by force as mass moves through this segment is approximated by
> > $$
> > \text{Work done} \approx \vec{F} \cdot \Delta \vec{r} = (-kx \vec{i}) \cdot \Delta x \vec{i} = -kx \Delta x
> > $$
> > This gives us
> > $$
> > \text{Total work done} \approx \sum -kx\Delta x
> > $$
> > In the limit, as $\| \Delta x \| \to 0$ , this sum becomes an ordinary definite integral.
> > $$
> > \text{Total work done} = \int_{x = 20}^{x = 0} -kx \, d = \left. \frac{kx^{2}}{2} \right| _{20}^{0} = \frac{k(20)^{2}}{2} = 200k 
> > $$
>
> - % Note that the work done is positive, since the force acts in the direction of motion

![](https://i.imgur.com/0vzysMa.png)

![](https://i.imgur.com/Gf9Bu0p.png)

### Circulation

> [!def]+ Circulation
> If $C$ is an oriented closed curve, the line integral of a vector field $\vec{F}$ around $C$, $\int_{C} \vec{F} \cdot d\vec{r}$, is called the **circulation** of $\vec{F}$ around $C$.

- **Circulation**
    - Measure of the net tendency of the vector field to point around the curve $C$
- % To emphasize that $C$ is closed, the circulation is sometimes denoted $\oint \vec{F} \cdot d\vec{r}$
    - Circle on the integral sign

> [!example]+ Example 7
> ![](https://i.imgur.com/yO8uGlj.png)
>
> > [!check]- Solution
> > - Consider vector field in Figure 18.9
> >     - If you think of this as representing the velocity of water flowing in a pond, the water is *circulating*
> > - Line integral around $C_{1}$
> >     - Measures the circulation around $C_{1}$
> >     - Positive $\because$ vectors of the field are all pointing in the direction of the path
> > - Line integral around $C_{0}$
> >     - Zero $\because$ vertical portions of the path are perpendicular to the field and contributions from the two horizontal portions cancel out
> >     - No net tendency for the water to circulate around $C_{2}$

## Properties of Line Integrals

> [!thm]+ Properties of Line Integrals
> For a scalar constant $\lambda$, vector fields $\vec{F}, \vec{G}$, and oriented curves $C, C_{1}, C_{2}$,
>
> $$
> \begin{align}
> \int_{C} \lambda \vec{F} \cdot d \vec{r}  & = \lambda \int_{C} \vec{F} \cdot d\vec{r} \tag{1} \\
> \int_{C} \left( \vec{F} + \vec{G} \right) \cdot d\vec{r}  & = \int_{C} \vec{F} \cdot d\vec{r} + \int_{C} \vec{G} \cdot d\vec{r} \tag{2} \\
> \int_{-C} \vec{F} \cdot d\vec{r}  & = -\int_{C} \vec{F} \cdot d\vec{r} \tag{3} \\
> \int_{C_{1} + C_{2}} \vec{F} \cdot d\vec{r} & = \int_{C_{1}} \vec{F} \cdot d\vec{r} + \int_{C_{2}} \vec{F} \cdot d\vec{r} \tag{4}
> \end{align}
> $$

^ee3617
