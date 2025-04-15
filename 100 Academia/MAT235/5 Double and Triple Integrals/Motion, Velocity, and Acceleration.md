---
dg-publish: true
tags: [lecture, math, note, university]
Course Code:
  - "[[MAT235]]"
aliases: []
Week: 16
Module:
  - "[[5 - Double and Triple Integrals]]"
Lecture:
  - "43"
  - "44"
  - "45"
Chapter: "17.2"
Slides/Notes: 
Date: 2025-01-29
Date created: Wed., Jan. 29, 2025, 4:08:29 am
Date modified: Mon., Feb. 17, 2025, 7:04:33 pm
---

# Motion, Velocity, and Acceleration

> [!goal] Find the vector quantities of velocity and acceleration from a parametric equation for the motion of an object.

## The Velocity Vector

Velocity of a moving particle can be represented by a vector with the following properties:

> [!summary]+ Velocity Vector Properties
> The **velocity vector** of a moving object is a vector $\vec{v}$ such that:
>
> 1. Magnitude of $\vec{v}$ is the *speed* of the object
> 2. Direction of $\vec{v}$ is the direction of motion

- Speed of object is $\| \vec{v} \|$
- Velocity vector is ==tangent== to object’s path

> [!example]- A child is sitting on a Ferris wheel of diameter 10 meters, making one revolution every 2 minutes. Find the speed of the child and draw velocity vectors at two different times.
>
> - Child moves at constant speed around a circle of radius 5 meters
>     - One revolution every 2 minutes
>     - One revolution: $2\pi r = 10\pi$
>     - Speed is $\frac{10\pi}{2}=5\pi \approx 15.7$ m/min.
> - Direction of motion is ==tangent== to circle
>     - → Perpendicular to radius at that point
>
> ![|454](https://i.imgur.com/xnwJbWR.png)
>
> *Figure 17.12: Velocity vectors of a child on a Ferris wheel (note that vectors would be in opposite direction if viewed from the other side)*

^e13055

### Computing the Velocity

- Find the velocity by taking a limit
    - As in one-variable calculus
- If position vector of particle is $\vec{r}(t)$ at time $t$, then displacement vector between its positions at times $t$ and $t + \Delta t$ is:
    - $\Delta \vec{r} = \vec{r}(t + \Delta t) - \vec{r}(t)$
    - See Figure 17.13

$$
\text{Average velocity} = \frac{\Delta \vec{r}}{\Delta t}
$$

- Limit as $\Delta t$ goes to zero → Instantaneous velocity at time $t$

> [!thm]+ Velocity vector
> The **velocity vector**, $\vec{v}(t)$, of a moving object with position vector $\vec{r}(t)$ at time $t$ is
> $$
> \vec{v}(t) = \lim_{ \Delta t \to 0 } \frac{\Delta \vec{r}}{\Delta t} = \lim_{ \Delta t \to 0 } \frac{\vec{r}(t + \Delta t) - \vec{r}(t)}{\Delta t},
> $$
> whenever the limit exists.
>
> > [!note]+ Notation
> > $$\vec{v} = \frac{d\vec{r}}{dt} = \vec{r'}(t)$$

- Direction of the velocity vector $\vec{r'}(t)$ is approximated by direction of vector $\Delta \vec{r}$
    - Approximation gets better as $\Delta t \to 0$
    - See Figure 17.13

![|477](https://i.imgur.com/0rmrZAa.png)

*Figure 17.13: The change, $\vec{\Delta}r$, in the position vector for a particle moving on a curve and the velocity vector $\vec{v} = \vec{r'}(t)$.*

### Components of the Velocity Vector

> [!tip]+ From parametric equations to position vector to velocity vector
>
> - Recall: If we represent a curve parametrically by $x = f(t), y = g(t), z = h(t)$, then its position vector is
>     - $\vec{r}(t) = f(t)\vec{i} + g(t)\vec{j} + h(t)\vec{k}$
>     - From [[Parameterized Curves#Vector Form of Parameterized Curves]]
> - Then, we can compute the velocity vector
>
> $$
> \begin{align}
> \vec{v}(t) & = \lim_{ \Delta t \to 0 } \frac{\vec{r}(t + \Delta t) - \vec{r}(t)}{\Delta t} \\
>  & = \lim_{ \Delta t \to 0 } \frac{\Big(f(t + \Delta t)\vec{i} + g(t + \Delta t) + h(t + \Delta t)\vec{k} \Big) - \Big(f(t)\vec{i} + g(t)\vec{j} + h(t)\vec{k} \Big)}{\Delta t} \\
>  & = \lim_{ \Delta t \to 0 } \left( \frac{f(t + \Delta t) - f(t)}{\Delta t} \vec{i} + \frac{g(t + \Delta t) - g(t)}{\Delta t} \vec{j} + \frac{h(t + \Delta t) - h(t)}{\Delta t} \vec{k} \right)  \\
>  & = f'(t)\vec{i} + g'(t)\vec{j} + h'(t)\vec{k} \\
>  & = \frac{dx}{dt} \vec{i} + \frac{dy}{dt}\vec{j} + \frac{dz}{dt} \vec{k}
> \end{align}
> $$

^a59f83

Then, we have the following result:

> [!thm]+ Components of the velocity vector of a particle
> The **components of the velocity vector** of a particle moving in space with position vector $\vec{r}(t) = f(t)\vec{i} + g(t)\vec{j} + h(t)\vec{k}$ at time $t$ are given by
> $$
> \vec{v}(t) = f'(t) \vec{i} + g'(t) \vec{j} + h'(t)\vec{k} = \frac{dx}{dt}\vec{i} + \frac{dy}{dt} \vec{j} + \frac{dz}{dt} \vec{k}
> $$

> [!example]+ Find the components of the velocity vector for the child on the ferris wheel in the [[Motion, Velocity, and Acceleration#^e13055|example]] using a coordinate system, which has its origin at the center of the Ferris wheel and which makes the rotation counterclockwise.
>
> - Ferris wheel has radius 5 meters and completes 1 revolution counterclockwise every 2 minutes
>     - → Motion is parameterized by equation of the form: $$\vec{r}(t) = 5 \cos(\omega t) \vec{i} + 5\sin(\omega t)\vec{j}$$
>         - $\omega$ is chosen to make the period 2 minutes
>     - Period of $\cos(\omega t), \sin(\omega t)$ is $2\pi/\omega$, so we solve: $$\frac{2\pi}{\omega} = 2 \implies \omega = \pi$$
> - Motion is described by the equation: $$\vec{r}(t) = 5 \cos(\pi t) \vec{i} + 5 \sin(\pi t)\vec{j}$$
> - Velocity is given by
>     $$
>     \vec{v} = \frac{dx}{dt}\vec{i} + \frac{dy}{dt}\vec{j} = -5\pi \sin(\pi t)\vec{i} + 5\pi \cos(\pi t)\vec{j}
>     $$
> - To check if speed is correct, calculate the magnitude of $\vec{v}$:
>     $$
>     \|\vec{v}\| = \sqrt{ (-5\pi)^{2}\sin ^{2}(\pi t) + (5\pi)^{2}\cos ^{2}(\pi t) } = 5\pi \sqrt{ \sin ^{2}(\pi t) + (5\pi)^{2}\cos ^{2}(\pi t) } = 5\pi \sqrt{ \sin ^{2}(\pi t) + \cos ^{2}(\pi t) } = 5\pi \approx 15.7
>     $$
>
> - To check if direction is correct, show that vector $\vec{v}$ at any time $t$ is perpendicular to position vector of child at time $t$
>     - i.e., Compute dot product
>         $$
>         \begin{align}
>         \vec{v} \cdot \vec{r} & = \Big(-5 \sin(\pi t)\vec{i} + 5\pi \cos(\pi t)\vec{j} \Big) \cdot \Big(5\cos(\pi t)\vec{i} + 5 \sin(\pi t) \Big) \\
>          & = -25 \pi \sin (\pi t) \cos(\pi t) + 25\pi \cos(\pi t) \sin(\pi t) \\
>          & = 0
>         \end{align}
>         $$
>
> ![|384](https://i.imgur.com/qSkKASr.png)
>
> *Figure 17.14: Velocity and radius vector of motion around a circle*

^cc2870

### Velocity Vectors and Tangent Lines

> [!tip]+ Velocity vectors can be used to find parametric equations for the tangent line, if there is one.
>
> - $\because$ Velocity vector is tangent to the path of motion

> [!example]+ Find the tangent line at the point $(1, 1, 2)$ to the curve defined by the parametric equation $\vec{r}(t) = t^{2}\vec{i} + t^{3}\vec{j} + 2t\vec{k}$.
>
> At time $t = 1$:
>
> - Particle is at point $(1, 1, 2)$ with position vector $\vec{r_{0}} = \vec{i} + \vec{j} + 2\vec{k}$
> - Velocity is $\vec{v} = \vec{r'}(1) = 2\vec{i} + 3\vec{j} + 2\vec{k}$
>     - $\because \vec{r'}(t) = 2t\vec{i} + 3t^{2}\vec{j} + 2\vec{k}$ at time $t$
>
> <!-- break -->
> - & Tangent line passes through $(1, 1, 2)$ in the direction of $\vec{v}$
>     - Has the parametric equation:
>         $$
>         \vec{r}(t) = \vec{r_{0}} + t\vec{v} = (\vec{i} + \vec{j} + 2\vec{k}) + t(2\vec{i} + 3\vec{j} + 2\vec{k})
>         $$

## The Acceleration Vector

- & The *rate of change* of the velocity of the particle i.e., **acceleration**, is a vector quantity
    - Just as the velocity of a particle moving in 2-space or 3-space

![|516](https://i.imgur.com/5mXcsU6.png)

*Figure 17.15: Computing the difference between two velocity vectors*

- Figure 17.15:
    - Shows particle at time $t$ with velocity vector $\vec{v}(t)$ then a little later time $t + \Delta t$
    - Vector $\Delta \vec{v} = \vec{v}(t + \Delta t) - \vec{v}(t)$
        - Change in velocity
        - Points approximately in the direction of acceleration

$$
\text{Average acceleration} = \frac{\Delta \vec{v}}{\Delta t}
$$

> [!def]+ Acceleration vector
> The **acceleration vector** of an object moving with velocity $\vec{v}(t)$ at time $t$ is
> $$
> \vec{a}(t) = \lim_{ \Delta t \to 0 } \frac{\Delta \vec{v}}{\Delta t} = \lim_{ \Delta t \to 0 } \frac{\vec{v}(t + \Delta t) - \vec{v}(t)}{\Delta t}
> $$
> if the limit exists.
>
> > [!note]+ Notation
> > $$\vec{a} = \frac{d\vec{v}}{dt} = \frac{d^{2}r}{dt^{2}} = \vec{r''}(t)$$

### Components of the Acceleration Vector

- Recall:
    - Represent a curve in space *parametrically* by $x = f(t), y = g(t), z = h(t)$
    - Velocity vector $\vec{v}(t)$ is given by $\vec{v}(t) = f'(t) \vec{i} + g'(t) \vec{j} + h'(t) \vec{k}$
- → Can express acceleration in *components*

From the definition of the acceleration vector:
$$
\vec{a}(t) = \lim_{ \Delta t \to 0 } \frac{\vec{v}(t + \Delta t) - \vec{v}(t)}{\Delta t} = \frac{d\vec{v}}{dt}
$$

Using the same [[Motion, Velocity, and Acceleration#^a59f83|method]] to compute $\frac{d\vec{v}}{dt}$ as we used to compute $\frac{d\vec{r}}{dt}$, we obtain:

> [!def]+ Components of the acceleration vector
> The **components of the acceleration vector**, $\vec{a}(t)$, at time $t$ of a particle moving in space with position vector $\vec{r}(t) = f(t)\vec{i} + g(t)\vec{j} + h(t)\vec{k}$ at time $t$ are given by
>
> $$
> \vec{a}(t) = f''(t)\vec{i} + g''(t) \vec{j} + h''(t) \vec{k} = \frac{d^{2}x}{dt^{2}} \vec{i} + \frac{d^{2}y}{dt} \vec{j} + \frac{d^{2}z}{dt^{2}} \vec{k}
> $$

## Motion in a Circle and Along a Line

Consider the velocity and acceleration vectors for two basic motions: *uniform motion around a circle*, and *motion along a straight line*.

> [!example]+ Find the acceleration vector for the child on the ferris wheel in earlier [[Motion, Velocity, and Acceleration#^cc2870|example]].
>
> - Child’s position vector is given by: $$\vec{r}(t) = 5\cos(\pi t) \vec{i} + 5\sin(\pi t) \vec{j}$$
> - Velocity vector is $$\vec{v}(t) = -5\pi \sin(\pi t) \vec{i} + 5\pi \cos(\pi t) \vec{j}$$
> - → Acceleration vector is $$\vec{a}(t) = \frac{d^{2}x}{dt}\vec{i} + \frac{d^{2}y}{dt}\vec{j} = -5\pi^{2} \cos(\pi t) \vec{i} - 5\pi^{2} \sin(\pi t) \vec{j}$$
>
> > [!obs]+ Notice that $\vec{a}(t) = -\pi^{2} \, \vec{r}(t)$
> >
> > - Acceleration vector is a multiple of $\vec{r}(t)$ and points towards the origin

This is an example of uniform circular motion.

> [!thm]+ Uniform circular motion properties
> For a particle whose motion is described by
> $$
> \vec{r}(t) = R \cos(\omega t) \vec{i} + R \sin(\omega t) \vec{j},
> $$
>
> - Motion is in a circle of radius $R$ with period $\frac{2\pi}{|\omega|}$
> - Velocity, $\vec{v}$, is ==tangent== to circle and speed is constant $\|\vec{v}\| = |\omega|R$
> - Acceleration, $\vec{a}$, points towards the center of the circle with $\|\vec{a}\| = \frac{\|\vec{v}\|^{2}}{R}$

- Uniform circular motion:
    - & Acceleration vector is perpendicular to the velocity vector $\vec{v}$
        - $\because \vec{v}$ does not change in magnitude, only in direction
- Straight line motion:
    - Acceleration vector points in the same direction as velocity vector if speed is ==increasing==
    - Points in opposite direction to velocity vector if speed is ==decreasing==

> [!example]+ Consider the motion given by the vector equation $$\vec{r}(t) = 2\vec{i} + 6\vec{j} + (t^{3} + t)(4\vec{i} + 3\vec{j} + \vec{k}).$$ Show that this is a straight-line motion in the direction of the vector $4\vec{i} + 3\vec{j} + \vec{k}$ and relate the acceleration vector to the velocity vector.
>
> - Velocity vector is $$\vec{v} = (3t^{2} + 1)(4\vec{i} + 3\vec{j} + \vec{k})$$
> - $(3t^{2} + 1)$ is a positive scalar
>     - → Velocity vector $\vec{v}$ always points in the direction of the vector $4\vec{i} + 3\vec{j} + \vec{k}$
>
> $$
> \text{Speed} = \|\vec{v}\| = (3t^{2} + 1)\sqrt{ 4^{2} + 3^{2} + 1^{2} } = \sqrt{ 26 }(3t^{2} + 1)
> $$
>
> - Speed is decreasing until $t = 0$, then starts increasing
>
> $$
> \vec{a} = 6t(4\vec{i} + 3\vec{j} + \vec{k})
> $$
>
> - For $t > 0$:
>     - Acceleration points in same direction as $4\vec{i} + 3\vec{j} + \vec{k}$
>     - Same direction as $\vec{v}$
> - For $t < 0$:
>     - Acceleration vector points in opposite direction to $\vec{v}$ because object is slowing down

> [!thm]+ Motion in a straight line properties
> For a particle whose motion is described by $$\vec{r}(t) = \vec{r_{0}} + f(t)\vec{v},$$
>
> 1. Motion is along a straight line through point with position vector $\vec{r_{0}}$ parallel to $\vec{v}$
> 2. Velocity, $\vec{v}$, and acceleration, $\vec{a}$, are parallel to the line

## The Length of a Curve

- Speed of a particle is the **magnitude** of its velocity vector $$\text{Speed} = \|\vec{v}\| = \sqrt{ \left( \frac{dx}{dt}^{2} + \left( \frac{dy}{dt} \right)^{2} + \left( \frac{dz}{dt}^{2} \right) \right) }$$
- As in one dimension, we can find the distance travelled by a particle along a curve by ==integrating its speed== $$\text{Distance travelled} = \int_{a}^{b} \|\vec{v}(t)\| \, dt .$$
- If particle never stops or reverses its direction as it moves along the curve:
    - & Distance travelled will be the same as the length of the curve

> [!thm]+ If the curve $C$ is given parametrically for $a \leq t \leq b$ by smooth functions and if the velocity vector $\vec{v}$ is not $\vec{0}$ for $a < t < b$, then
> $$
> \text{Length of C} = \int_{a}^{b} \|\vec{v}\| \, dt.
> $$
