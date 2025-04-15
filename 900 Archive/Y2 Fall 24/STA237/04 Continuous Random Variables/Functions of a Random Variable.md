---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 9
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "16"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/IJgTcGqAZl4SKDLIfYpPM4
  - https://share.goodnotes.com/s/rFkXloK4CsBIqmMjfIpoxe
Date: 2024-11-07
Date created: Tue., Nov. 12, 2024, 2:12:13 pm
Date modified: Fri., Dec. 6, 2024, 6:43:27 am
---

# Example for Discrete Random Variables

- You are measuring and tagging Dolly Varden trout as part of your internship in northern BC.
- Number of trout you tag in ==an hour== follows a [[Poisson Distribution]] with rate $\lambda = 2$
- You work for 1 hour
- For every trout you tag, you will get a bonus of $5
- Amount of money you get is a RV $Y = 5X$, where $X \sim \text{Pois}(2)$

> [!question]+ What is the PMF of $Y$?
> Given that $Y = 5X$ where $X \sim \text{Pois}(2)$, we can find the PMF of $Y$ using this table:
>
> | $x$ | 0 | 1 | 2 | >2 |
> |-----|---|---|---|-----|
> | $y = 5x$ | 0 | 5 | 10 | >10 |
> | $P(X = x) = P(Y = y)$ | 0.1353 | 0.2707 | 0.2707 | 0.3233 |
>
> Therefore, the PMF of $Y$ is:
> $$P_Y(y) = \begin{cases}
> 0.1353 & y = 0 \\
> 0.2707 & y = 5 \\
> 0.2707 & y = 10 \\
> 0.3233 & y > 10 \\
> 0 & \text{otherwise}
> \end{cases}$$
>
> Note: This is still a discrete random variable, just with different possible values than $X$.

# CDF Method for Finding PDF of $Y = g(X)$

> [!thm]+ How to find the density of $Y = g(X)$
> 1. Determine possible values of $Y$ based on values of $X$ and function $g$
>     - i.e., ==Find support of $Y$==
> 2. Begin with the cdf ==$F_{Y}(y) = P(Y \leq y) = P(g(X) \leq y)$==. Express CDF in terms of original random variable $X$
> 3. From $P(g(X) \leq y)$, obtain an expression of the form $P(X \leq \cdots)$
>     - Right-hand side of expression will be a ==function of $y$==
>     - If $g$ is invertible, then ==$P(g(X) \leq y) = P(X \leq g^{-1}(y))$==
> 4. ==Differentiate== with respect to $y$ to obtain the density $f_{Y}(y)$

- Find CDF of function of the RV
- Use CDF to get PDF by taking derivative
- Be careful about the ==direction of inequality sign== when setting up probabilities
    - e.g., If $Y = -X$, then $P(Y \leq y) = P(X \geq -y)$
- Be careful about the ==support== of $Y$
    - May not be the same as support of $X$

## Example. Shifted Exponential

> [!example]+ Find the PDF of a shifted exponential distribution using the CDF method.
>
> > [!info]+ Problem Setup
> > - Given: $Y = X + 0.5$ where $X \sim \text{Exp}(0.1)$
> > - Goal: Show that $f_Y(y) = 0.1e^{-0.1(y-0.5)}$ for $y > 0.5$ and 0 otherwise
>
> > [!success]+ Solution Steps
> > 1. Find CDF of Y:
> >     - $F_Y(y) = P(Y \leq y) = P(X + 0.5 \leq y) = P(X \leq y - 0.5)$
> > 2. Use known CDF of X:
> >     - We know $F_X(x) = 1-e^{-0.1x}$ for $x > 0$
> >     - Therefore: $F_Y(y) = 1-e^{-0.1(y-0.5)}$ for $y-0.5 > 0 \implies y > 0.5$
> > 3. Find PDF by differentiating CDF:
> > - $f_Y(y) = \frac{d}{dy}F_Y(y) = 0.1e^{-0.1(y-0.5)}$ if $y > 0.5$, and 0 otherwise

![](https://i.imgur.com/L6lQd5V.png)

- The support of Y is $(0.5, \infty)$ due to the shift of 0.5
- The shape of the exponential distribution is preserved, just shifted right by 0.5
- The rate parameter (0.1) remains unchanged in the transformed distribution

## Example. Changing Unit of Measurement

- Amount of time in *hours* you plan on studying this week can be modelled as a RV $X \sim \text{Unif}(1, 3)$
- What if instead: Wanted to express this in minutes
- Let $Y$ be the time in minutes that you study this week

> [!question]+ What is the PDF of $Y$? Use the MGF of the Uniform distribution
>
> > [!abstract]+ Recall
> > MGF of a $\text{Unif}(a, b)$ RV
> > $$M_{X}(t) = \begin{cases}
> > \frac{e^{tb}-e^{ta}}{t(b-a)}, \quad t \neq 0 \\
> > 1, \quad t = 0 \\
> > \end{cases}$$
> > Property of MGF
> > $$M_{aX + b}(t) = e^{bt}M_{X}(at)$$
>
> > [!info]+ Given
> > - $X \sim \text{Unif}(1, 3)$
> > - $Y = 60X$
>
> > [!success]+ Solution Steps
> > 1. Find MGF of Y using the property of MGF:
> > $$M_Y(t) = M_{60X}(t) = M_X(60t)$$
> > 2. Substitute into the Uniform MGF formula:
> > $$M_Y(t) = \frac{e^{60t(3)} - e^{60t(1)}}{60t(3-1)} = \frac{e^{180t} - e^{60t}}{120t}$$
> > 3. This is the MGF of a Uniform distribution with parameters (60, 180)
> > Therefore:
> > $$Y \sim \text{Unif}(60, 180)$$
> > 4. The PDF of Y is:
> > $$f_Y(y) = \frac{1}{120} \text{ for } 60 < y < 180 \text{ and } 0 \text{ otherwise}$$
>
> - When we multiply a Uniform random variable by a constant $c$, the resulting distribution is still Uniform
> - The new endpoints are multiplied by the same constant
> - The height of the PDF is adjusted to maintain total probability of 1

> [!question]+ What is the PDF of $Y$? Use the CDF method.
>
> > [!info]+ Given
> > - $X \sim \text{Unif}(1, 3)$
> > - $Y = 60X$ (converting hours to minutes)
>
> > [!success]+ Solution Steps
> > 1. Find CDF of Y:
> > $$F_Y(y) = P(Y \leq y) = P(60X \leq y) = P(X \leq \frac{y}{60})$$
> >
> > 2. Use CDF of uniform distribution:
> > For $1 \leq \frac{y}{60} \leq 3$ (or equivalently $60 \leq y \leq 180$):
> > $$F_Y(y) = \frac{\frac{y}{60} - 1}{3-1} = \frac{y/60 - 1}{2}$$
> >
> > 3. Find PDF by differentiating CDF:
> > $$f_Y(y) = \frac{d}{dy}F_Y(y) = \frac{1}{2} \cdot \frac{1}{60} = \frac{1}{120}$$
> > for $60 < y < 180$ and 0 otherwise
>
> > [!note]+ Verification
> > - This matches our result from the MGF method
> > - The PDF is constant over the interval [60, 180], as expected for a uniform distribution
> > - The height $\frac{1}{120}$ ensures the total probability equals 1 since $120 \cdot \frac{1}{120} = 1$

![](https://i.imgur.com/SM5UdGj.png)

## Example. Linear Function of RV

- Let $X \sim \text{Exp}(4)$ and $Y = 6X + 11$

> [!example]+ Find the PDF of $Y$ using CDF method.
>
> > [!info]+ Given
> > - $X \sim \text{Exp}(4)$
> > - $Y = 6X + 11$
>
> > [!success]+ Solution Steps
> > 1. Find CDF of Y:
> > $$F_Y(y) = P(Y \leq y) = P(6X + 11 \leq y) = P(X \leq \frac{y-11}{6})$$
> >
> > 2. Use CDF of exponential distribution:
> > For $\frac{y-11}{6} > 0$ (which means $y > 11$):
> > $$F_Y(y) = 1 - e^{-4(\frac{y-11}{6})}$$
> >
> > 3. Find PDF by differentiating CDF:
> > $$f_Y(y) = \frac{d}{dy}F_Y(y) = \begin{cases}
> >    \frac{2}{3}e^{-\frac{2}{3}(y-11)}, & \text{if } y > 11 \\
> >    0, & \text{otherwise}
> >    \end{cases}$$
>
> > [!note]+ Key Points
> > - The support of Y starts at 11 due to the shift
> > - The exponential shape is preserved but scaled and shifted
> > - The rate parameter is adjusted from 4 to $\frac{2}{3}$ due to the scaling by 6

![](https://i.imgur.com/bYgNLc3.png)

## Example. Quadratic Function of RV

> [!example]+ Find the PDF of $Y$ using CDF method.
>
> > [!info]+ Given
> > - $X \sim \text{Unif}(0,1)$
> > - $Y = X^2$
>
> > [!success]+ Solution Steps
> > 1. Find CDF of Y:
> > $$F_Y(y) = P(Y \leq y) = P(X^{2} \leq y) = P(X \leq \sqrt{y})= \sqrt{ y }$$
> >
> > 2. Use CDF of uniform distribution:
> > For $0 \leq \sqrt{y} \leq 1$ (which means $0 \leq y \leq 1$):
> > $$F_Y(y) = \sqrt{y}$$
> >
> > 3. Find PDF by differentiating CDF:
> > $$f_Y(y) = \frac{d}{dy}F_Y(y) = \begin{cases}
> >    \frac{1}{2}y^{-\frac{1}{2}}, & \text{if } 0 < y \leq 1 \\
> >    0, & \text{otherwise}
> >    \end{cases}$$

- What if $X \sim \text{Unif}(-1, 1)$?

> [!example]+ What if $X \sim \text{Unif}(-1, 1)$?
>
> > [!info]+ Given
> > - $X \sim \text{Unif}(-1,1)$
> > - $Y = X^2$
>
> > [!success]+ Solution Steps
> > 1. Find CDF of Y:
> > $$F_Y(y) = P(Y \leq y) = P(X^2 \leq y) = P(-\sqrt{y} \leq X \leq \sqrt{y})$$
> >
> > 2. Use CDF of uniform distribution:
> > For $0 \leq y \leq 1$ (since $x^2 \leq 1$ when $x \in [-1,1]$):
> > $$F_Y(y) = F_X(\sqrt{y}) - F_X(-\sqrt{y}) = \frac{\sqrt{y} - (-\sqrt{y})}{2} = \sqrt{y}$$
> >
> > 3. Find PDF by differentiating CDF:
> > $$f_Y(y) = \frac{d}{dy}F_Y(y) = \begin{cases}
> >    \frac{1}{2}y^{-\frac{1}{2}}, & \text{if } 0 < y \leq 1 \\
> >    0, & \text{otherwise}
> >    \end{cases}$$

![](https://i.imgur.com/LfCqIrc.png)

# Method of Transformations

- Special case based on the CDF Method
- Don’t always need to return to the CDF as starting point for finding a PDF

> [!thm]+ Method of Transformations for Finding PDF
> If $Y = g(X)$ is ==strictly increasing or strictly decreasing== over the range of $X$, so that it is **invertible** with inverse function $X = g^{-1}(Y)$, then:
> $$f_Y(y) = f_X(g^{-1}(y))\left|\frac{d}{dy}g^{-1}(y)\right|$$
> where:
> - $f_X(x)$ is the PDF of the original random variable X
> - $g^{-1}(y)$ is the inverse function of the transformation
> - $\left|\frac{d}{dy}g^{-1}(y)\right|$ is the absolute value of the derivative of the inverse function
>     - Result of the chain rule when doing $\frac{d}{dy}$

- Function must be ==strictly monotonic== (either increasing or decreasing)
- Function must be ==invertible== over the support of X
- The derivative of the inverse function must exist

> [!warning]+ Limitations
> - Doesn’t work for all transformations and all RVs
> - Must verify that the transformation is invertible over the support of X
> - Examples:
>   - $Y = X^2$ is ==not invertible== if support of X is ℝ
>   - $Y = X^2$ ==is invertible== if support of X is $(0,3)$

- Often simpler than using the CDF method
- Direct way to find PDF without going through CDF
- Particularly useful for linear transformations and other simple monotonic functions

> [!tip]+ When to Use
> - When the transformation function is clearly monotonic
> - When finding the inverse function is straightforward
> - When the derivative of the inverse function is easy to compute

## Example. Shifted Exponential

> [!example]+ Find the PDF of a shifted exponential distribution using the transformation method.
>
> > [!info]+ Problem Setup
> > - Given: The lifetime of a product can be modelled as $Y = X + 0.5$ where $X \sim \text{Exp}(0.1)$
> > - Goal: Show that $f_Y(y) = 0.1e^{-0.1(y-0.5)}$ for $y > 0.5$ and 0 otherwise
>
> > [!success]+ Solution Steps
> > 1. Identify the transformation:
> > - $y = g(x) = x + 0.5$
> > - This is a strictly increasing function (linear shift)
> >
> > 1. Find the inverse function:
> > - $x = g^{-1}(y) = y - 0.5$
> >
> > 1. Find the derivative of inverse:
> > - $\frac{d}{dy}g^{-1}(y) = 1$
> >
> > 1. Apply transformation method:
> > $$f_Y(y) = f_X(y-0.5) \cdot |1| = 0.1e^{-0.1(y-0.5)}$$
> > for $y > 0.5$ and 0 otherwise

## Example. Changing Unit of Measurement

> [!example]+ Find the PDF of Y using the transformation method
>
> > [!info]+ Problem Setup
> > - Given: Study time in hours $X \sim \text{Unif}(1,3)$
> > - Want: Study time in minutes $Y = 60X$
> > - Goal: Find PDF of Y using transformation method
>
> > [!success]+ Solution Steps
> > 1. Identify the transformation:
> > - $y = g(x) = 60x$
> > - This is a strictly increasing function (linear scaling)
> >
> > 1. Find the inverse function:
> > - $g^{-1}(y) = \frac{y}{60}$
> >
> > 1. Find the derivative of inverse:
> > - $\frac{d}{dy}g^{-1}(y) = \frac{1}{60}$
> >
> > 1. Apply transformation method:
> > $$f_Y(y) = f_X(\frac{y}{60}) \cdot \left|\frac{1}{60}\right| = \frac{1}{2} \cdot \frac{1}{60} = \frac{1}{120}$$
> > for $60 < y < 180$ and 0 otherwise

## Example. Linear Function of RV

> [!example]+ Find the PDF of Y using the transformation method
>
> > [!info]+ Given
> > - $X \sim \text{Exp}(4)$
> > - $Y = 6X + 11$
>
> > [!success]+ Solution Steps
> > 1. Identify the transformation:
> > - $y = g(x) = 6x + 11$
> > - This is a strictly increasing function
> >
> > 1. Find the inverse function:
> > - $g^{-1}(y) = \frac{y-11}{6}$
> >
> > 1. Find the derivative of inverse:
> > - $\frac{d}{dy}g^{-1}(y) = \frac{1}{6}$
> >
> > 1. Apply transformation method:
> > $$f_Y(y) = f_X(\frac{y-11}{6}) \cdot \left|\frac{1}{6}\right| = 4e^{-4(\frac{y-11}{6})} \cdot \frac{1}{6} = \frac{2}{3}e^{-\frac{2}{3}(y-11)}$$
> > for $y > 11$ and 0 otherwise

## Example. Quadratic Function of RV

> [!example]+ Find the PDF of Y using the transformation method when X ~ Unif(0,1)
>
> > [!info]+ Given
> > - $X \sim \text{Unif}(0,1)$
> > - $Y = X^2$
>
> > [!success]+ Solution Steps
> > 1. Identify the transformation:
> > - $y = g(x) = x^2$
> > - This is strictly increasing on (0,1)
> >
> > 1. Find the inverse function:
> > - $g^{-1}(y) = \sqrt{y}$
> >
> > 1. Find the derivative of inverse:
> > - $\frac{d}{dy}g^{-1}(y) = \frac{1}{2\sqrt{y}}$
> >
> > 1. Apply transformation method:
> > $$f_Y(y) = f_X(\sqrt{y}) \cdot \left|\frac{1}{2\sqrt{y}}\right| = 1 \cdot \frac{1}{2\sqrt{y}} = \frac{1}{2}y^{-\frac{1}{2}}$$
> > for $0 < y \leq 1$ and 0 otherwise

> [!warning]+ Note on Quadratic Transformation
> The transformation method cannot be directly applied to $Y = X^2$ when $X \sim \text{Unif}(-1,1)$ because:
> - The function is not monotonic over the support [-1,1]
> - The same y value corresponds to both positive and negative x values
> - This is why we needed to use the CDF method for this case

# Standardizing a Normal RV

> [!question]+ Show that if $X \sim N(\mu, \sigma^2)$, then $Z = \frac{X-\mu}{\sigma} \sim N(0,1)$ using MGFs, CDF method, and transformation method.

Recall:

- If $X \sim N(\mu, \sigma^2)$, then $M_X(t) = e^{\mu t + (\sigma^2t^2)/2}$
- Property of MGF: $M_{aX+b}(t) = e^{bt}M_X(at)$
- MGF (and PDF, CDF) uniquely determines a probability distribution

> [!success]+ Using MGFs
> 1. Express Z in terms of X:
>    - $Z = \frac{X-\mu}{\sigma} = \frac{1}{\sigma}X + (-\frac{\mu}{\sigma})$
>    - Of the form $aX + b$ where $a = \frac{1}{\sigma}$ and $b = -\frac{\mu}{\sigma}$
>
> 2. Find MGF of Z using the property:
> $$M_Z(t) = M_{\frac{X-\mu}{\sigma}}(t) = e^{-\frac{\mu}{\sigma}t}M_X(\frac{t}{\sigma})$$
>
> 3. Substitute the MGF of X:
> $$M_Z(t) = e^{-\frac{\mu}{\sigma}t} \cdot e^{\mu(\frac{t}{\sigma}) + \frac{\sigma^2(\frac{t}{\sigma})^2}{2}}$$
>
> 4. Simplify:
> $$M_Z(t) = e^{-\frac{\mu t}{\sigma}} \cdot e^{\frac{\mu t}{\sigma}} \cdot e^{\frac{t^2}{2}} = e^{\frac{t^2}{2}}$$
>
> 5. Recognize this as the MGF of $N(0,1)$

> [!success]+ Using CDF method
> 1. Find CDF of Z:
> $$F_Z(z) = P(Z \leq z) = P(\frac{X-\mu}{\sigma} \leq z) = P(X \leq \sigma z + \mu)$$
>
> 2. Use CDF of normal distribution:
> $$F_Z(z) = \int_{-\infty}^{\sigma z + \mu} \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}dx$$
>
> 3. Make substitution $u = \frac{x-\mu}{\sigma}$:
> $$F_Z(z) = \int_{-\infty}^{z} \frac{1}{\sqrt{2\pi}}e^{-\frac{u^2}{2}}du$$
>
> 4. Recognize this as the CDF of $N(0,1)$

Recall:

- If $X \sim N(\mu, \sigma^{2})$, $$f(x) = \frac{1}{\sigma \sqrt{ 2\pi }}e^{\frac{-{x-\mu}^{2}}{2 \sigma^{2}}}$$for $x \in \mathbb{R}$
- PDF uniquely specifies a distribution

> [!success]+ Using transformation method
> 1. Identify the transformation:
>    - $z = g(x) = \frac{x-\mu}{\sigma}$
>    - This is a strictly increasing function
>
> 2. Find the inverse function:
>    - $g^{-1}(z) = \sigma z + \mu$
>
> 3. Find the derivative of inverse:
>    - $\frac{d}{dz}g^{-1}(z) = \sigma$
>
> 4. Apply transformation method:
> $$f_Z(z) = f_X(\sigma z + \mu) \cdot |\sigma| = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(\sigma z + \mu-\mu)^2}{2\sigma^2}} \cdot \sigma$$
>
> 5. Simplify:
> $$f_Z(z) = \frac{1}{\sqrt{2\pi}}e^{-\frac{z^2}{2}}$$
> which is the PDF of $N(0,1)$

> [!obs] All three methods confirm that if $X \sim N(\mu, \sigma^2)$, then $Z = \frac{X-\mu}{\sigma} \sim N(0,1)$

![](https://i.imgur.com/KDYewSS.jpeg)

# Summary

- We already knew how to use the MGF to find the distribution of a function of a RV
    - Not every MGF of a function of a RV will have a recognizable form
    - Might want the PDF of the new RV directly
- **CDF Method**
    - Find the CDF of the new RV
    - Take derivative to get PDF
- **Transformation Method**
    - If $g$ is invertible on its support,
    - $$f_{Y}(y) = f_{X}(g^{-1}(y)) \left|\frac{d}{dy} g^{-1}(y)\right|$$
