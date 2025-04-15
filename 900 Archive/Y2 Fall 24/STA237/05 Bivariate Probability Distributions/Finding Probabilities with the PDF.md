---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 10
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "18"
  - "19"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/JvMEYIPHcqWjrtPBX0XusL
  - https://share.goodnotes.com/s/QiB8Js5Y2tB7C6Jxj7SIte
Date: 2024-11-14
Date created: Sat., Nov. 23, 2024, 4:34:43 pm
Date modified: Fri., Dec. 6, 2024, 12:19:19 am
---

# Conditional Density Functions

Recall
![[Probability Distributions of More than One RV#^b8409f]]

- The denominators $f_{X}(x), f_{Y}(y)$ are not probabilities
    - $x, y$ are like constants in their respective cases
        - i.e., We fix them in our conditionals
    - Different $x,y$ can give different $f(y|x)$

## Example 9.1: Conditional PDF

Given $f(x, y) = \frac{y}{4x}, 0 < y < x < 4$ and 0 otherwise,

- Sketch the support of $(x, y)$

```desmos-graph
left=-1; right=4;
top=4; bottom=-1
---

y<x|0<x<4|0<y|black
x=4|y>0|black
x=0.5|y>0|blue
(0,0)
```

> [!question]+ Find $P(Y < 1 | X = 0.5)$ without doing any calculations
>
> - Valid values of $y$ are $0 < y < x \to 0 < y < 0.5$
> - So, $P(Y < 1 | X = 0.5) = 1$
>   - $Y$ is always less than 1 when $X = 0.5$

> [!question]+ Find $P(Y < 1 | X = 2)$
>
> ```desmos-graph
> left=-1; right=4;
> top=4; bottom=-1
> ---
> 
> y<x|0<x<4|0<y|black
> x=4|y>0|black
> x=2|y>0|blue
> y<1|y>0|blue
> (0,0)
> ```
>
> $$
> \begin{align*}
> f_{x}(x) = \int_{\text{all }y} f(x, y) \, dy &= \int_{0}^{x} \frac{y}{4x}  \, dy \\
> &= \frac{1}{4x} \int_{0}^{x} y \, dy \\
> &= \frac{1}{4x}\left( \left.\frac{y^{2}}{2}\right|_{0}^{x} \right) \\
> &= \frac{x^{2}}{8x} \\
> &= \frac{x}{8} \quad \text{for } 0 < x < 4
> \end{align*}
> $$
> Then,
> $$\begin{align*}
> f_{Y|X}(y|X) &= \frac{f(x,y)}{f_{X}(x)} \\
> &= \frac{\frac{y}{4x}}{\frac{x}{8}} \\
> &= \frac{2y}{x^{2}}
> \end{align*}$$
>
> So, $P(Y < 1 | X = x) = \int_{0}^{1} \frac{2y}{x^{2}} \, dy = \frac{1}{x^{2}}$, and substituting $X = 2$ gives us $\frac{1}{4}$.

## Example 9.3: Conditional PDF on a Circle

Let $X$ and $Y$ be uniformly distributed in the circle of radius 1 centred at the origin.

- Sketch out the support of $(x, y)$
- What is $f(x,y)$?

> [!important]+ Support Region
>
> - Points $(x,y)$ that satisfy $x^2 + y^2 \leq 1$
> - This is the unit circle and its interior

![](https://i.imgur.com/h4SIcgo.png)

> [!question]+ Find $f(x,y)$
>
> 1. For uniform distribution on a region:
>     - Probability density is constant over the region
>     - Must integrate to 1 over entire region
>         - $\int_{\text{all}  x} \int_{\text{all }x} f(x,y) \, dxdy = 1$
> 2. Area of circle = $\pi r^2 = \pi(1)^2 = \pi$
> 3. Therefore:
>     - $f(x,y) = \frac{1}{\pi}$ for $x^2 + y^2 < 1$
>     - $f(x,y) = 0$ otherwise

> [!thm]+ Joint PDF
> $$f(x,y) = \begin{cases}
> \frac{1}{\pi} & \text{if } x^2 + y^2 < 1 \\
> 0 & \text{otherwise}
> \end{cases}$$

> [!note]+ Key Points
>
> - Uniform distribution means equal probability density everywhere in support region
> - Total probability must equal 1, so density = 1/(area of region)
> - Support region is circular due to condition $x^2 + y^2 \leq 1$

> [!question]+ Find the marginal PDF of $X$
>
> Note that $x^{2} + y^{2} < 1 \implies y^{2} < 1 - x^{2} \implies -\sqrt{ 1-x^{2} } < y < \sqrt{ 1-x^{2} }$.
>
> ![|center|400](https://i.imgur.com/VckjRwC.png)
>
>
>
> $$
> \begin{align*}
> f_{X}(x) &= \int_{\text{all }y} f(x, y) \, dy \\
> &= \int_{-\sqrt{ 1-x^{2} }}^{\sqrt{ 1-x^{2} }} \frac{1}{\pi} \, dy \\
> &= \left. \frac{y}{\pi} \right|_{-\sqrt{ 1-x^{2} }}^{\sqrt{ 1-x^{2} }} \\
> &= \frac{2\sqrt{ 1-x^{2} }}{\pi} && \text{for } -1 < x < 1
> \end{align*}
> $$

> [!question]+ Find the conditional distribution of $Y|X = x$
>
> ![|center|400](https://i.imgur.com/vb5DmFe.png)
>
> $$
> \begin{align*}
> f_{Y|X}(y|x) &= \frac{f_{X, Y}(x, y)}{f_{X}(x)} \\
> &= \frac{\frac{1}{\pi}}{\frac{2\sqrt{ 1-x^{2} }}{\pi}} \\
> &= \frac{1}{2\sqrt{ 1-x^{2} }} && \text{for } -\sqrt{ 1-x^{2} } < y < \sqrt{ 1-x^{2} }
> \end{align*}
> $$

> [!obs]+ Notice that $f(y | x)$ depends on $x$, but not $y$
> - Since $x$ is treated as a constant, $Y|X - x \sim Unif\left( -\sqrt{ 1-x^{2} }, \sqrt{ 1-x^{2} } \right)$
> - Condition distribution of $Y|X$ is **[[Continuous Uniform Distribution|uniform]]**, but the marginal distributions of $Y$ and $X$ are not!

# Relationship Between CDF and PDF

Recall from [[Probability Distributions of More than One RV#Joint CDF]]:

![[Probability Distributions of More than One RV#^d0b0f5]]

## CDF to PDF: Partial Derivatives

> [!tip] If derivatives exist and are continuous, then the order you take derivatives does not matter

Suppose the joint CDF of $X$ and $Y$ is
$$
F(x, y) = \frac{x^{2}y}{2} + \frac{xy^{2}}{2}
$$
and the support of $(x,y)$ is $0 <x,y < 1$.
$$
F(x, y) = \begin{cases}
1 && x \geq 1, y \geq 1 \\
F(x, 1) = \frac{x^{2}}{2} + \frac{x}{2} && 0 < x < 1, y \geq 1 \\
\frac{y^{2}}{2} + \frac{y}{2} && x \geq 1, 0 < y < 1 \\
\frac{x^{2}y}{2} + \frac{xy^{2}}{2} && 0 < x < 1, 0 < y < 1  \\
0 && x < 0 \text{ or } y<0
\end{cases}
$$

> [!question]+ Find the joint PDF
> 1. Take partial derivatives:
>     - First w.r.t. $x$: $\frac{\partial}{\partial x}F(x,y) = xy + \frac{y^2}{2}$
>     - Then w.r.t. $y$: $\frac{\partial^2}{\partial y \partial x}F(x,y) = x + y$
>
> 2. Therefore:
>     - $f(x,y) = x + y$ for $0 < x,y < 1$
>     - $f(x,y) = 0$ otherwise

## PDF to CDF: Double Integrals

> [!tip] If the *bounds* represent a **rectangular** region, you can switch the order of integration

The joint PDF of $X$ and $Y$ is $f(x, y) = x + y$ for $0 < x, y < 1$ and 0 otherwise.

> [!question]+ Find the joint CDF

$$
\begin{align*}
F(x,y) &= \int_{-\infty}^{x} \left( \int_{-\infty}^{y} f_{X, Y}(s, t) \, dt \right) \, ds \\
&= \int_{0}^{x} \left( \int_{0}^{y} (s + t) \, dt \right) \, ds \\
&= \int_{0}^{x} \left.\left( st + \frac{t^{2}}{2} \right) \right|_{t = 0}^{t=y} \, ds \\
&= \int_{0}^{x}ys + \frac{y^{2}}{2} \, ds \\
&= \left. \frac{y}{2}s^{2} + \frac{y^{2}}{2}s \right|_{s = 0}^{s = x} \\
&= \frac{x^{2}y}{2} + \frac{xy^{2}}{2}
\end{align*}
$$

# Finding Probabilities with the PDF

![[Probability Distributions of More than One RV#^f0872c]]

## Example D.1 I

$X$ and $Y$ have joint PDF $f(x, y) = x + y$ with support $0 \leq x, y \leq 1$.

> [!question]+ Find $P\left( X < \frac{1}{2}, Y < \frac{1}{3} \right)$
>
> 1. Draw the support
>
>     ```desmos-graph
>     left=0; right=1.5;
>     top=1.5; bottom=0;
>     ---
>     x <= 1| 0 <= y <= 1 | blue
>     y = 1|x<=1|blue
>     ```
>
> 2. Draw the probability you care about
>
>     ```desmos-graph
>     left=-0.5; right=1;
>     top=1; bottom=-0.5;
>     ---
>     x < 1/2|y < 1/3|purple
>     y < 1/3|x < 1/2|purple
>     ```
>
> 3. Combine to find the **integration area**
>
>     ```desmos-graph
>     left=-0.5; right=1;
>     top=1; bottom=-0.5;
>     ---
>     0 <= x < 1/2|0 <= y < 1/3|purple
>     0 <= y < 1/3|0 <= x < 1/2|purple
>     ```
>
> 4. Set up **integration bounds**
>
> > [!tip] If the bounds represent a rectangular region, you can switch the order of integration
>
> $$
> \begin{align*}
> \int_{0}^{ 1/3 } \left( \int_{0}^{1/2} x + y \, dx \right) \, dy &= \int_{0}^{1/2}\left( \int_{0}^{1/3} x + y \, dy \right) \, dx  \\
> \end{align*}
> $$
> $$
> \begin{align*}
> \int_{0}^{1/3}\left( \int_{0}^{1/2} (x + y) \, dx \right) \, dy &= \int_{0}^{1/3} \left( \left. \frac{x^{2}}{2} + yx \right|_{x=0}^{x=\frac{1}{2}} \right) \, dy \\
> &= \int_{0}^{1/3} \frac{1}{8} + \frac{y}{2} \, dy \\
> &= \frac{y}{8} + \frac{y^{2}}{4} \bigg|_{0}^{1/3} \\
> &= \frac{5}{72}
> \end{align*}
> $$

## Example D.1 Ii

$X$ and $Y$ have joint pdf $f(x, y) = x + y$ with support $0 \leq x, y \leq 1$

> [!question]+ Find $P(X < Y)$
>
> Then, the support of $f$ is
>
> ```desmos-graph
> left=0; right=1.5;
> top=1.5; bottom=0;
> ---
> x<=1|y<=1|blue
> y<=1|x<=1|blue
> ```
>
> and the probability that we care about ($X < Y$) is
>
> ```desmos-graph
> left=-1; right=1.5;
> top=1.5; bottom=-1;
> ---
> x<y|blue
> ```
>
> Combining to get our **integration region** gives us
>
> ```desmos-graph
> left=0; right=1.2;
> top=1.2; bottom=0;
> ---
> x < y | y <= 1 | magenta
> 
> ```
>
> ![|center|300](https://i.imgur.com/BVogpkM.png)
>
> $$
> \begin{align*}
> \int_{0}^{1} \left( \int_{0}^{y} (x + y) \, dx \right) \, dy &= \int_{0}^{1} \left[ \left.\frac{x^{2}}{2}+xy\right|_{x = 0}^{x = y} \right] \, dy \\
> &= \int_{0}^{1} \frac{3}{2}y^{2} \, dy \\
> &= \left.\frac{1}{2} y^{3}\right|_{y=0}^{y=1} \\
> &= \frac{1}{2}
> \end{align*}
> $$
>
> ![|center|300](https://i.imgur.com/gVh48dr.png)
>
> $$
> \int_{0}^{1} \left( \int_{x}^{1} (x + y) \, dy \right) \, dx
> $$

## Example D.1 Iii

$X, Y$ have joint PDF $f(x, y) = 8xy$ with support $0 \leq x \leq y \leq 1$.

> [!question]+ Find $P(X + Y < 1)$
>
> 1. Draw the support
>
>     ```desmos-graph
>     left=-0.5; right=1;
>     top=1; bottom=-0.5;
>     ---
>     x <= y | 0 <= x <= 1 | 0 <= y <= 1 | purple
>     ```
> 2. Draw the area that represents the probability that you care about
>
> $P(X + Y < 1) = P(Y < 1 - X)$
>
> ```desmos-graph
> left=-0.5; right=1;
> top=1; bottom=-0.5;
> ---
> y < 1 - x
> ```
>
>  3. Combine to figure out integration area
>
>     ```desmos-graph
>     left=-0.5; right=1;
>     top=1; bottom=-0.5;
>     ---
>     y < 1 - x | 0 <= y <= 1 | 0 <= x <= y |purple
>     x <= y | 0 <= x <= 1 | 0 <= y < 1 - x | purple
>     ```
>
> 4. Set up integration bounds
>
> ![|center|400](https://i.imgur.com/h37KqLP.png)
>
> $$\int_{0}^{1/2} \left( \int _{x}^{1-x} 8 xy \, dy \right) \, dx$$
>
> ![|center|400](https://i.imgur.com/XOY0A8Z.png)
>
> $$\int_{0}^{1/2}\int_{0}^{y}8xy\,dxdy + \int_{\frac{1}{2}}^{1}\int_{0}^{1-y} 8xy \,dxdy$$
>
> 1. Evaluate the integral
>
>     $$
>     \begin{align*}
>     P(X + Y < 1) &= \int_{0}^{1/2} \left( \int_{x}^{1-x} 8xy \, dy \right) \, dx \\
>     &= \int_{0}^{1/2} \left[ 4xy^{2} \big|_{y=x}^{y=1-x} \right] \, dx \\
>     &= \int_{0}^{1/2}4x(1-2x)\, dx \\
>     &= \int_{0}^{1/2}4x - 8x^{2} \, dx \\
>     &= 2x^{2} - \frac{8}{3}x^{3} \bigg|_{x=0}^{x=1/2} \\
>     &= \frac{1}{2} - \frac{1}{3} \\
>     &= \frac{1}{6}
>     \end{align*}
>     $$
