---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 8
Module:
  - "[[4 - Continuous Random Variables]]"
Lecture:
  - "13"
Chapter: "6.4"
Slides/Notes:
  - https://share.goodnotes.com/s/5O07xen8EixNbSCGgzvIwZ
Date: 2024-10-22
Date created: Tue., Oct. 29, 2024, 6:20:33 pm
Date modified: Sun., Nov. 17, 2024, 7:10:35 pm
---

# Continuous Uniform Distribution

- $X$ is a RV taking on values in the interval $(a, b)$ with ==all subintervals of a fixed length being equally likely==
- Since all intervals of the same length are equally likely, the ==PDF must be constant==
    - $f(x) = k, a \leq x \leq b$ for some constant $k$
    - To make $\int_{a}^{b} f(x)dx = 1$, we must have:

> [!def]+ Uniform Distribution
> A random variable $X$ is **uniformly distribution on $(a, b)$** if the density function of $X$ is $$f(x) = \frac{1}{b-a}, \quad \text{for } a < x < b,$$
> and 0, otherwise. We write $X \sim \text{Unif}(a, b)$

![|center|400](https://i.imgur.com/Y1pUIuL.png)

![|center|400](https://i.imgur.com/Dzcl59T.png)

# Continuous Uniform CDF

For a $\text{Unif}(a,b)$ distribution, we can find the CDF by integrating the PDF:

Given the PDF:
$$f(x) = \begin{cases} 
\frac{1}{b-a} & \text{if } a < x < b \\
0 & \text{otherwise}
\end{cases}$$

The CDF is:
$$F(x) = \int_{-\infty}^x f(k)dk = \begin{cases}
0 & \text{if } x \leq a \\
\frac{x-a}{b-a} & \text{if } a \leq x \leq b \\
1 & \text{if } x \geq b
\end{cases}$$

> [!note]+ This gives us a piecewise function that:
> - Is 0 before $x=a$ (no probability mass before the interval)
> - Increases linearly from 0 to 1 over the interval $[a,b]$
> - Is 1 after $x=b$ (all probability mass has been accumulated)

![|center|400](https://i.imgur.com/aP2ob8d.png)

# Continuous Uniform Expectation

For a $\text{Unif}(a,b)$ distribution, we can find the expected value:

$$\begin{align*}
E[X] &= \int_{-\infty}^{\infty} xf(x)dx \\
&= \int_{a}^{b} x \cdot \frac{1}{b-a}dx \\
&= \frac{1}{b-a} \cdot \frac{x^2}{2}\Big|_{a}^{b} \\
&= \frac{b^2 - a^2}{2(b-a)} \\
&= \frac{(b-a)(b+a)}{2(b-a)} \\
&= \frac{b+a}{2}
\end{align*}$$

> [!note]+ Interpretation
> The expected value of a uniform distribution is the midpoint of its interval, which makes intuitive sense given the symmetry of the distribution.
> ![|center|400](https://i.imgur.com/633ZhGf.png)

> [!tip]+ Exercise
> Try finding $E[X^2]$ and $\text{Var}(X)$ using similar integration techniques.
> 
> For $E[X^2]$:
> $$\begin{align*}
> E[X^2] &= \int_{a}^{b} x^2 \cdot \frac{1}{b-a}dx \\
> &= \frac{1}{b-a} \cdot \frac{x^3}{3}\Big|_{a}^{b} \\
> &= \frac{b^3 - a^3}{3(b-a)} \\
> &= \frac{(b-a)(b^2 + ab + a^2)}{3(b-a)} \\
> &= \frac{b^2 + ab + a^2}{3}
> \end{align*}$$
> 
> For $\text{Var}(X)$:
> $$\begin{align*}
> \text{Var}(X) &= E[X^2] - (E[X])^2 \\
> &= \frac{b^2 + ab + a^2}{3} - \left(\frac{b+a}{2}\right)^2 \\
> &= \frac{b^2 + ab + a^2}{3} - \frac{b^2 + 2ab + a^2}{4} \\
> &= \frac{4(b^2 + ab + a^2) - 3(b^2 + 2ab + a^2)}{12} \\
> &= \frac{b^2 - 2ab + a^2}{12} \\
> &= \frac{(b-a)^2}{12}
> \end{align*}$$

# Continuous Uniform MGF

For a $\text{Unif}(a,b)$ distribution, the moment generating function is:

$$\begin{align*}
M_X(t) &= E[e^{tX}] = \int_{a}^{b} e^{tx} \cdot \frac{1}{b-a}dx \\
&= \frac{e^{tx}}{t(b-a)}\Big|_{a}^{b} \\
&= \begin{cases}
\frac{e^{tb} - e^{ta}}{t(b-a)} & \text{if } t \neq 0 \\
E[e^0] = E[1] = 1 & \text{if } t = 0
\end{cases}
\end{align*}$$

> [!tip]+ Exercise: Moments from MGF
> Try at home:
> 1. Verify the first moment $E[X] = \frac{a+b}{2}$ by using $E[X] = M'_X(0)$
> 2. Find the second moment $E[X^2]$ using $M''_X(0)$
> 3. Calculate the variance using $\text{Var}(X) = M''_X(0) - (M'_X(0))^2$
>
> > [!summary]- Solution
> > 1. First moment using $M'_X(0)$:
> > $$\begin{align*}
> > M'_X(t) &= \frac{be^{tb} - ae^{ta}}{t(b-a)} - \frac{e^{tb} - e^{ta}}{t^2(b-a)} \\
> > M'_X(0) &= \lim_{t \to 0} \left(\frac{be^{tb} - ae^{ta}}{t(b-a)} - \frac{e^{tb} - e^{ta}}{t^2(b-a)}\right) \\
> > &= \frac{b+a}{2} = E[X]
> > \end{align*}$$
> >
> > 2. Second moment using $M''_X(0)$:
> > $$\begin{align*}
> > M''_X(t) &= \frac{b^2e^{tb} - a^2e^{ta}}{t(b-a)} - 2\frac{be^{tb} - ae^{ta}}{t^2(b-a)} + 2\frac{e^{tb} - e^{ta}}{t^3(b-a)} \\
> > M''_X(0) &= \frac{b^2 + ab + a^2}{3} = E[X^2]
> > \end{align*}$$
> >
> > 3. Variance:
> > $$\begin{align*}
> > \text{Var}(X) &= M''_X(0) - (M'_X(0))^2 \\
> > &= \frac{b^2 + ab + a^2}{3} - \left(\frac{b+a}{2}\right)^2 \\
> > &= \frac{(b-a)^2}{12}
> > \end{align*}$$

# Summary


For a random variable $X \sim \text{Unif}(a,b)$:

> [!summary]+ Key Functions & Properties
> **PDF (Probability Density Function)**
> $$f(x) = \begin{cases} 
> \frac{1}{b-a}, & a \leq x \leq b \\
> 0, & \text{otherwise}
> \end{cases}$$
>
> **CDF (Cumulative Distribution Function)**
> $$F(x) = \begin{cases}
> 0, & x < a \\
> \frac{x-a}{b-a}, & a \leq x \leq b \\
> 1, & x > b
> \end{cases}$$
>
> **Expected Value**
> $$E[X] = \frac{a+b}{2}$$
>
> **Variance**
> $$\text{Var}[X] = \frac{(b-a)^2}{12}$$
>
> **MGF (Moment Generating Function)**
> $$M_X(t) = \begin{cases}
> \frac{e^{tb}-e^{ta}}{t(b-a)} & \text{if } t \neq 0 \\
> 1 & \text{if } t = 0
> \end{cases}$$

> [!note] If a random variable has an MGF of the form $M_X(t) = \begin{cases} \frac{e^{tb}-e^{ta}}{t(b-a)} & \text{if } t \neq 0 \\ 1 & \text{if } t = 0 \end{cases}$, then it must be uniformly distributed.

> [!info]+ Key R Functions
> - `dunif(x, a, b)`: PDF evaluation
> - `punif(x, a, b)`: CDF evaluation
> - `qunif(p, a, b)`: Quantile function (inverse CDF)
> - `runif(k, a, b)`: Generate $k$ random observations

```r
# PDF Evaluation at X = 0.6 for Unif(0,2)
> dunif(0.6, 0, 2)
[1] 0.5 # f(0.6)

# CDF Evaluation at X = 0.6 for Unif(0,2)
> punif(0.6, 0, 2)
[1] 0.3 # F(0.6)

# Find X where F(x) = 0.3 for Unif(0,2)
> qunif(0.3, 0, 2)
[1] 0.6 # F^(-1)(0.3)

# Generate 10 Random Uniform Values
> runif(10, 0, 2)
[1] 1.57012130 1.33451994 0.3595822 0.17420411 1.48354348
[6] 0.04748160 1.59426867 1.02585573 0.09359830 1.55010642
```

> [!note]+ Generating uniform random numbers between 0 and 1 is fundamental in simulation, as it serves as the basis for generating random numbers from other distributions.

# Example. Manufacturing

> [!example]+ Manufacturing
>  Suppose the research department of a steel manufacturer believes that one of the company’s rolling machines is producing sheets of steel of varying thickness.
> 
> The thickness is a uniform random variable with values between 150 and 200 mm.

Let $X =$ Thickness of the sheets produced by this machine in mm
$X \sim \text{Unif}(150, 200)$

- **PDF**:
    - $$ f(x) = \begin{cases} \frac{1}{50} && \text{if } 150 < x < 200 \\ 0 && \text{otherwise}\end{cases}$$
    - Note that $\frac{1}{b-a} = \frac{1}{200-150} = \frac{1}{50}$
    - ![|400](https://i.imgur.com/Ewn9tCD.png)
- **Mean**:
    - $$\begin{align*}E[X] &= \frac{b+a}{2} = \frac{200 + 150}{2} = 175 \text{ mm} \end{align*}$$
- **Variance**
    - $$\begin{align*} \text{Var}(X) &= \frac{(b-a)^2}{12} = \frac{(200-150)^2}{12} = \frac{2500}{12} = 208.33 \end{align*}$$
- **Standard deviation**:
    - $$\begin{align*} \text{SD}(X) &= \sqrt{\text{Var}(X)} = \sqrt{208.33} = 14.434 \text{ mm} \end{align*}$$

> [!question]+ Any sheet less than 160mm must be scrapped. Calculate the fraction of steel sheets produced by this machine that have to be scraped
> $$\begin{align*}
> P(X < 160) &= F(160) \\ &= \frac{{160-150}}{200-150} \\ &= \frac{10}{50} \\ &= 0.2
> \end{align*}$$
> 20% of the sheets will have to be scraped
> ![|center|300](https://i.imgur.com/Xj618Zq.png)

> [!question]+ Given a sheet that is *not* scrapped, what is the probability that it is less than 180mm thick?
> $$\begin{align*} 
> P(X < 180 \vert X > 160) &= \frac{P(X < 180 \cap X > 160)}{P(X > 160)} \\
> &= \frac{{P(160 < X < 180)}}{P(X > 160)} \\ 
> &= \frac{{F(180) - F(160)}}{1-F(160)} \\
> &= \frac{{\frac{20}{50}}}{1-\frac{10}{50}} \\ 
> &= 0.5
> \end{align*}$$
> ![|center|300](https://i.imgur.com/fAM3Y73.png)
