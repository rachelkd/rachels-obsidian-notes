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
Chapter: "18.3"
Slides/Notes: 
Date: 2025-02-24
Date created: Mon., Feb. 24, 2025, 2:12:16 pm
Date modified: Mon., Feb. 24, 2025, 10:13:59 pm
---

# Gradient Fields and Path-Independent Fields

## Motivation

Recall:

> [!info]+ For a function $f$, of one variable, the **Fundamental Theorem of Calculus** tells us that the ==definite integral of a rate of change, $f'$==, gives us the ==total change in $f$==
> $$\int_{a}^{b} f'(t) \, dt = f(b) - f(a) $$

- & Applies to functions of two or more variables

> [!question]- What is the quantity that decrease the rate of change?
> - Gradient vector field

> [!tip]+ If we know the **gradient** of a function, we can compute the total change in $f$ between two points
> - Using a **line integral**

## Finding the Total Change in $f$ from $\text{grad } f$: the Fundamental Theorem

> [!info]+ To find the change in $f$ between two points $P, Q$…
> - Choose a smooth path $C$ from $P$ to $Q$
> - Divide that path into many small pieces
>
> ![](https://i.imgur.com/sxSOipW.png)

- & Estimate the change in $f$ as we move through a displacement $\Delta \vec{r}_{i}$ from $\vec{r}_{i}$ to $\vec{r}_{i + 1}$
    - Suppose $\vec{u}$ is a unit vector in the direction of $\Delta \vec{r}_{i}$
    - Change in $f$ is given by
        $$
        \begin{align}
        f(\vec{r}_{i + 1}) - f(\vec{r}_{i}) & \approx \text{Rate of change of }f \times \text{Distance moved in direction of } \vec{u} \\
         & = f_{\vec{u}} (\vec{r}_{i}) \; \left\| \Delta \vec{r}_{i} \right\|  \\
         & = \text{grad } f \cdot \vec{u} \left\| \Delta \vec{r}_{i} \right\| \\
         & = \text{grad }f \cdot \Delta \vec{r}_{i}  &  & (\text{since } \Delta \vec{r}_{i} = \| \Delta \vec{r}_{i} \| \vec{u})
        \end{align}
        $$
    - See definition of directional derivative in [[Gradients and Directional Derivatives in Space#Directional Derivatives in Space]]
- Sum over all pieces of the path
    $$
    \text{Total change} = f(Q) - f(P) \approx \sum\limits_{i=0}^{n - 1} \text{grad }f(\vec{r}_{i}) \cdot \Delta \vec{r}_{i}
    $$
In the limit as $\| \Delta \vec{r}_{i} \|$ approaches zero, this suggests the following result:

> [!thm]+ Theorem 18.1: The Fundamental Theorem of Calculus for Line Integrals
> Suppose $C$ is a *piecewise smooth* oriented path with starting point $P$ and ending point $Q$.
>
> If $f$ is a function whose gradient is continuous on the path $C$, then
> $$\int_{C} \text{grad }f \cdot d\vec{r} = f(Q) - f(P)$$

^d508ee

- % There are many different paths from $P$ to $Q$
    - ![](https://i.imgur.com/bHl8o15.png)
- Value of the line integral $\int_{C} \text{grad } f \cdot d\vec{r}$ depends only on the endpoints of $C$
    - Does not depend on where $C$ goes in between

### Examples

> [!example]+ Suppose that grad $f$ is everywhere perpendicular to the curve joining $P$ and $Q$.
> 1. Explain why you expect the path joining $P$ and $Q$ to be a contour
> 2. Using a line integral, show that $f(P) = f(Q)$
>
> ![](https://i.imgur.com/Ol4DndK.png)
> *Figure 18.26: The gradient vector field of the function $f$*
>
> > [!check]- Solution
> > 1. Gradient of $f$ is everywhere perpendicular to the path from $P$ to $Q$, as you expect along a contour
> > 2. Consider the path from $P$ to $Q$ shown in Figure 18.28.
> >     - Evaluate the line integral $$\int_{C} \text{grad } f \cdot d\vec{r} = f(Q) - f(P)$$
> >     - grad $f$ is everywhere perpendicular to the path $\implies$ line integral is 0 $\implies f(Q) = f(P)$

![](https://i.imgur.com/EVYezvj.png)

## Path-Independent, or Conservative, Vector Fields

- Example 2:
    - Line integral was *independent* of the path taken between the two (fixed) endpoints
    - Vector fields whose line integrals have this property have a special name

> [!def]+ Path-independent i.e., conservative
> A vector field $\vec{F}$ is said to be **path-independent**, or **conservative**, if:
> - For any two points $P$ and $Q$:
>     - & Line integral $\int_{C} \vec{F} \cdot d\vec{r}$ has the same value along any piecewise smooth path $C$ from $P$ to $Q$ lying in the domain of $\vec{F}$

- On the other hand:
    - If line integral $\int_{C} \vec{F} \cdot d\vec{r}$ does depend on the path $C$ joining $P$ to $Q$:
        - $\vec{F}$ is said to be a **path-dependent** vector field

Suppose that $\vec{F}$ is any continuous gradient field, so $\vec{F} = \text{grad }f$.

- If $C$ is a path from $P$ to $Q$:
- Fundamental Theorem for Line Integrals tells us that $$\int_{C} \vec{F} \cdot d\vec{r} = f(Q) - f(P)$$
    - $ Right-hand side of this equation does not depend on path
        - Only endpoints

Then, we have the following property:

> [!thm]+ Property of Continuous Gradient Vector Field
> If $\vec{F}$ is a continuous gradient vector field, then $\vec{F}$ is path-independent

## Path-Independent Fields and Gradient Fields

> [!obs]+ Every gradient field is path-independent
> - ? What about the converse?
>     - Given a path-independent vector field $\vec{F}$, can we find a function $f$ such that $\vec{F} = \text{grad }f$?
>         - & Yes, provided that $\vec{F}$ is continuous

### Construct $f$ from $\vec{F}$

- $ There are many choices for $f$
    - Can add a constant to $f$ without changing $\text{grad }f$
- If we pick a fixed starting point $P$:
    - & By adding or subtracting a constant to $f$, we can ==ensure that $f(P) = 0$==
- $\implies$ For any other point $Q$, we define $f(Q)$ by the formula $$f(Q) = \int_{C} \vec{F} \cdot d\vec{r}, \quad \text{where }C\text{ is any path from } P \to Q$$
- Since $\vec{F}$ is path-independent:
    - Does not matter which path we choose from $P \to Q$
- If $\vec{F}$ is not path-independent, then different choices might give different values for $f(Q)$
    - ! $f$ would not be a function
        - $\because$ Function has to have a single value at each point

> [!thm]+ Theorem 18.2: Path-Independent Fields are Gradient Fields
> If $\vec{F}$ is a continuous path-independent vector field on an open region $R$, then $$\vec{F} = \text{grad }f$$for some $f$ defined on $R$

^657a90

Combining [[#^d508ee|Theorem 18.1]] and [[#^657a90|Theorem 18.2]]:

> [!thm] A continuous vector field $\vec{F}$ defined on an open region is **path-independent** if and only if $\vec{F}$ is a gradient vector field

- Function $f$ is sufficiently important that is has a given special name

> [!def]+ Potential function
> If a vector field $\vec{F}$ is of the form $\vec{F} = \text{grad }f$ for some scalar function $f$, then $f$ is called a **potential function** for the vector field $\vec{F}$.
