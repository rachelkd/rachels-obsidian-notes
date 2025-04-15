---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 11
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/JvMEYIPHcqWjrtPBX0XusL
Date: 2024-11-21
Date created: Sun., Nov. 24, 2024, 5:12:37 pm
Date modified: Tue., Dec. 3, 2024, 12:14:45 am
---

# Rolling Two Dice

- $X =$ Number on Dice 1
- $Y =$ Number on Dice 2

![|center|400](https://i.imgur.com/MBxiJ0g.png)

*Probabilities for each combination*

![|center|400](https://i.imgur.com/N3FSNdJ.png)

*Sums of each combination*

> [!tip] To get the marginal PMF, sum along either $X$ or $Y$

> [!tip] If we’re interested in $Z = X + Y$, sum along the ==diagonal==

> [!question]+ What is the PMF of $Z = X + Y$?
>
> Given:
>
> - $X$ = Number on Dice 1 (1-6)
> - $Y$ = Number on Dice 2 (1-6)
> - Each outcome is equally likely: $P(X = x, Y = y) = 1/36$
>
> For any value $z$, $P(Z = z) = P(X + Y = z) = \sum\limits_{\text{all }y}P(X = z-y, Y=y) = \sum\limits_{\text{all }x}P(X = x, Y = z - x)$:
>
> > [!obs]+ These are the sums of the diagonal probabilities
> >
> | $z$   | Ways to get sum | $P(Z = z)$ |
> |-----|----------------|-----------|
> | 2   | (1,1)          | 1/36      |
> | 3   | (1,2),(2,1)    | 2/36      |
> | 4   | (1,3),(2,2),(3,1) | 3/36   |
> | 5   | (1,4),(2,3),(3,2),(4,1) | 4/36 |
> | 6   | (1,5),(2,4),(3,3),(4,2),(5,1) | 5/36 |
> | 7   | (1,6),(2,5),(3,4),(4,3),(5,2),(6,1) | 6/36 |
> | 8   | (2,6),(3,5),(4,4),(5,3),(6,2) | 5/36 |
> | 9   | (3,6),(4,5),(5,4),(6,3) | 4/36 |
> | 10  | (4,6),(5,5),(6,4) | 3/36 |
> | 11  | (5,6),(6,5) | 2/36 |
> | 12  | (6,6) | 1/36 |
>
> Therefore:
> $$P(Z = z) = \begin{cases}
> \frac{z-1}{36} & \text{for } z = 2,3,\dots,7 \\
> \frac{13-z}{36} & \text{for } z = 8,9,\dots,12 \\
> 0 & \text{otherwise}
> \end{cases}$$

![](https://i.imgur.com/3ETZorr.jpeg)

> [!warning]+ Be careful about the range that you are summing over
>
> What is $P(Z = 6)$?
> $$
> \sum\limits_{x=1}^{5} P(X = x, Y = 6 - z) = \sum\limits_{x = 1}^{5} \frac{1}{36} = \frac{5}{36}
> $$
>
> - We sum all $x$‘s until 5 (inclusive), since $x = 6 \implies y = 6-6=0$ doesn’t exist!

> [!thm]+ PMF of Sum of *Discrete* Random Variables
> For any random variables $X$ and $Y$:
> $$P(X + Y = k) = P\left(\bigcup_i \{X = i, Y = k-i\}\right) = \sum_i P(X = i, Y = k-i)$$
>
> If $X$ and $Y$ are independent:
> $$P(X + Y = k) = \sum_i P(X = i, Y = k-i) = \sum_i P(X = i)P(Y = k-i)$$
>
> Source: Wagaman and Dobrow (2021), pp. 142

# Functions of Continuous RVs

- Everywhere we had a sum, we swap it out for an **integral**

> [!thm]+ PMF of Sum of *Continuous* Random Variables
>
> $$
> \begin{align*}
> f_{X+Y}(t) &= \int_{-\infty}^{\infty} f_{X,Y}(x, t-x) \, dx \\
> f_{X+Y}(t) &= \int_{-\infty}^{\infty} f_{X, Y}(t - y, dy) \, dy
> \end{align*}
> $$

- We are integrating over the line where $x + y = t$

![](https://i.imgur.com/RXyfAlz.png)

## Example. $Z = X + Y$ for Continuous RV

- You need to reply to two emails that are in your inbox
- The time it takes for you to reply to the first email is **exponentially distributed** with rate parameter $\lambda$
- The time it takes for you to reply to the second email is also **exponentially** distributed with rate parameter $\lambda$
- Assume the time it takes you to answer each email is independent

> [!question]+ What is the PDF of the total time it takes you to reply to both emails?
>
> > [!obs]+ Method 1: This is the **[[Gamma Distribution|Gamma]]** distribution with $\alpha = 2$
> >
> > - Interested in the time until 2 events, where the time to each event are independent and follow an exponential distribution with the *same* rate parameter
> > - If $X \sim Gamma(\alpha, \lambda)$, then $f(x) = \frac{\lambda^{\alpha}x^{\alpha-1}e^{-\lambda x}}{\Gamma(\alpha)}$ for $x \geq 0$
> > So,
> > $$f(z) = \frac{\lambda^{2}z^{2 - 1}e^{-\lambda z}}{\Gamma(2)} = \lambda^{2}ze^{-\lambda z}$$
>
> > [!info]+ Method 2: $T = X + Y$
> >
> > - Use $f_{X + Y}(t) = \int_{-\infty}^{\infty} f_{X, Y}(t-y, y) \, dy$
> > - This works even if $X + Y$ are *not* i.i.d., so this is a more general method
> > - We are integrating along the line where $x + y = t$
>
> $$
> \begin{align*}
> \int_{-\infty}^{\infty} f_{X,Y}(t-y, y) \, dy &= \int_{-\infty}^{\infty} f_{X}(t-y) f_{Y}(y) \, dy \\
> &= \int_0^t \lambda e^{-\lambda(t-y)} \lambda e^{-\lambda y} \, dy && \text{(for } X,Y \sim \text{Exp}(\lambda)\text{)} \\
> &= \lambda^2 e^{-\lambda t} \int_0^t dy && \text{(combine exponentials)} \\
> &= \lambda^2 t e^{-\lambda t}
> \end{align*}
> $$
>
> Note: Integration bounds changed to $(0,t)$ because:
>
> - $X > 0 \implies t-y > 0 \implies y < t$
> - $Y > 0 \implies y > 0$
> - Therefore $0 < y < t$

> [!info]+ Method 3: Find $P(X + Y < z)$ then differentiate with respect to $z$
>
> 1. Find the joint PDF of $X$ and $Y$ (Use the fact that they are independent) and their support
> 2. Figure out the integration region
> 3. Evaluate integral to obtain $P(X + Y < z)$ (the CDF)
> 4. Differentiate with respect to $z$

## What About Other Kinds of Transformations?

- For **discrete** distributions:
  - If we are interested in the PMF of a function of random variables, we find all combinations of $(x, y)$ where $f(x, y) =$ the value we want and then add up the PMF
- For **continuous** distributions:
  - Extend the ==transformation method to 2 dimensions==

## Recall: Transformation Method for One RV

> [!thm]+ Transformation Method
> If $Y = g(X)$ is strictly increasing or strictly decreasing over the range of $X$, so that it is **invertible** with inverse function $X = g^{-1}(Y)$, then
> $$
> f_{Y}(y) = f_{X}\left( g^{-1}(y) \right) \left| \frac{d}{dy} g^{-1}(y) \right| 
> $$

- There is an *inverse piece* and *derivative piece*

## Transformation Method for Joint PDF

- Similar to single RV transformation method
- For two variables, suppose $g_{1}$ and $g_{2}$ are *invertible* in the sense that $v = g_{1}(x, y)$ and $w = g_{2}(x, y)$ can be solved uniquely for $x$ and $y$ with $x = h_{1}(v, w)$ and $y = h_{2}(v, w)$
  - i.e., $g_{1}$ and $g_{2}$ define a one-to-one transformation in the plane
- If $h_{1}$ and $h_{2}$ have continuous partial derivatives, let $J$ denote the **Jacobian** consisting of the determinant of the partial derivatives
  - Assume $J \neq 0$

> [!def]+ Jacobian Matrix
> $$J = \underbrace{\begin{vmatrix}
> \frac{\partial h_1}{\partial v} & \frac{\partial h_1}{\partial w} \\
> \frac{\partial h_2}{\partial v} & \frac{\partial h_2}{\partial w}
> \end{vmatrix}}_{\text{determinant}} =
> \frac{\partial h_1}{\partial v}\frac{\partial h_2}{\partial w} -
> \frac{\partial h_2}{\partial v}\frac{\partial h_1}{\partial w}$$

> [!thm]+ Joint Density of $V$ and $W$
> Under the aforementioned assumptions,
> $$f_{V,W}(v,w) = f_{X,Y}(h_1(v,w), h_2(v,w))|J|$$

### Example 1

- Let $X$ and $Y$ be i.i.d. [[Exponential Distribution|Exponential]] RVs with rate $\lambda = 1$
- Let $v = g_{1}(x, y) = x - y$ and $w = g_{2}(x, y) = x + y$

#### Step 1. Find the Inverse Functions $x = h_{1}(v, w)$ and $y = h_{2}(v, w)$

$$
f_{X,Y}(x, y) = e^{-x}e^{-y} \quad x > 0, y > 0
$$
See [[Probability Distributions of More than One RV#Independence of Two RVs]].
$$
\begin{align*}
v + w &= (x - y) + (x + y) = 2x \\
v - w &= (x - y) - (x + y) = -2y \\
\\
\implies x &= \frac{{v + w}}{2} = h_{1}(v, w) \\
\implies y &= \frac{{w - v}}{2} = h_{2}(v, w)
\end{align*}
$$

#### Step 2. Find the Determinant of the Jacobian

$$
\begin{align*}
\frac{ \partial h_{1} }{ \partial v } &= \frac{1}{2}, \frac{ \partial h_{1} }{ \partial w } = \frac{1}{2}, \\
\frac{ \partial h_{2} }{ \partial v } &= -\frac{1}{2}, \frac{ \partial h_{2} }{ \partial w } = \frac{1}{2} \\
\end{align*}
$$
$$
\begin{align*}
\implies J &= \begin{vmatrix}
\frac{1}{2} & \frac{1}{2}  \\
-\frac{1}{2}  & \frac{1}{2}
\end{vmatrix}
= \frac{1}{2} \cdot \frac{1}{2} - \left( -\frac{1}{2} \cdot \frac{1}{2} \right) = \frac{1}{2}
\end{align*}
$$

#### Step 3. Put it together to Find $f_{V, W}(v, w)$

$$
\begin{align*}
& f_{X,Y}(h_{1}(v, w), h_{2}(v, w))|J| \\
&= \frac{1}{2} f_{X, Y}\left( \frac{{v+w}}{2}, \frac{{w-v}}{2} \right) \\
&= \frac{1}{2} e^{-\frac{v+w}{2} - \frac{w-v}{2}} \\
&= \frac{1}{2} e^{-w}
\end{align*}
$$
The support is $x = \frac{{v+w}}{2} > 0$ and $y = \frac{{w-v}}{2} > 0$, which
$$
\begin{align*}
\implies v + w > 0, w - v > 0 \\
\implies -w < v < w, w > 0
\end{align*}
$$

### Example 2

> [!tip] Sometimes, it is easier to solve a 1-D problem using 2-D formulas

- Let $X, Y$ be i.i.d. Exponential RVs with rate $\lambda$
- What is the distribution of $\frac{X}{X + Y}$?
    - $f_{X, Y}(x, y) = \left( \lambda e^{-\lambda x} \right)(-\lambda e^{\lambda y}) = \lambda^{2}e^{-\lambda(x + y)}$
- Let $v = g_{1}(x,y) = \frac{x}{x+y}$ and $w = g_{2}(x,y) = x+y$

#### Step 1. Find the Inverse Functions $x = h_{1}(v, w)$ and $y = h_{2}(v, w)$

$$
\begin{align*}
x &= vw = \frac{x}{x+y}\bigg( x + y \bigg)  = h_{1}(v, w) \\
y &= w - vw = x + y - x = h_{2}(v, w)
\end{align*}
$$

#### Step 2. Find the Determinant of the Jacobian

$$
J = \begin{vmatrix}
\frac{ \partial h_{1} }{ \partial v }  & \frac{ \partial h_{1} }{ \partial w } \\
\frac{ \partial h_{2} }{ \partial v }  & \frac{ \partial h_{2} }{ \partial w } 
\end{vmatrix}
= \begin{vmatrix}
w & v \\
-w & 1-v
\end{vmatrix}
= w(1-v) - v(-w) = w
$$

### Step 3. Find $f_{V, W}(v, w)$

$$
\begin{align}
f_{X, Y}(x, y)) &= \lambda^{2}e^{-\lambda (x + y)} \\
 \\
f_{V, W}(v, w) &= f_{X, Y}(vw, w - vw)w \\
&= \lambda^{2}we^{-\lambda(vw + w - vw)} \\
&= \lambda^{2}we^{-\lambda w}
\end{align}
$$

#### Step 4. Integrate away $w$ to Find the Marginal Density of $V$

$$
\begin{align}
f_{V}(v) &= \int_{0}^{\infty} \lambda^{2}we^{-\lambda w} \, dw \\
&= \lambda^2 \int_{0}^{\infty} we^{-\lambda w} \, dw \\
&\text{Using integration by parts:} \, u = w, \, dv = e^{-\lambda w}dw \\
&\text{Then } du = dw, \, v = -\frac{1}{\lambda}e^{-\lambda w} \\
&= \lambda^2 \left[ -\frac{w}{\lambda}e^{-\lambda w} + \int_{0}^{\infty} \frac{1}{\lambda}e^{-\lambda w} \, dw \right] \\
&= \lambda^2 \left[ -\frac{w}{\lambda}e^{-\lambda w} - \frac{1}{\lambda^2}e^{-\lambda w} \right]_{0}^{\infty} \\
&= \lambda^2 \left[ \left(0 - 0\right) - \left(-\frac{0}{\lambda}e^{0} - \frac{1}{\lambda^2}e^{0}\right) \right] \\
&= \lambda^2 \cdot \frac{1}{\lambda^2} \\
&= 1
\end{align}
$$
for $0 < v < 1$ since $v = \frac{x}{x+y}$ and $x < x + y$ since $x, y > 0$.
$$
\implies V \sim \text{Unif}(0, 1)
$$

## The Ratio $\frac{X}{X + Y}$ is Uniform

> [!obs] The ratio $\frac{X}{X+Y}$ is [[Continuous Uniform Distribution|uniform]].

![](https://i.imgur.com/tlyPmxx.png)

```r
# lambda = 3
X = rexp(10000, rate = 3)
Y = rexp(10000, rate = 3)
Z = X / (X + Y)

par(mfrow - c(1, 2))
hist(X, main = "Histogram of exp(3)")
hist(Z, main = "Histogram of X/(X+Y) which is Unif(0, 1)")
```
