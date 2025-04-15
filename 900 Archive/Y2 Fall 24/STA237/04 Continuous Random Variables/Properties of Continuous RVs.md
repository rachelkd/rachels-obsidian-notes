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
Chapter: 6.2-3
Slides/Notes:
  - https://share.goodnotes.com/s/5O07xen8EixNbSCGgzvIwZ
Date: 2024-10-22
Date created: Mon., Oct. 28, 2024, 4:39:07 pm
Date modified: Fri., Dec. 6, 2024, 6:11:00 am
---

CDFs, PDFs, MGFs, quantiles, Expectations.

- Recall definition of **probability density function**
    - ![[Introduction to Continuous Random Variables#^6677b0]]
    - Probabilities now correspond with the area under $f(x)$
    - Everywhere we had a sum when dealing with discrete RVs, ==we now have an integral==
        - ![|300](https://i.imgur.com/mzcDdl9.png)

# Cumulative Distribution Functions

- CDF for continuous random variables works exact the same way as that for [[Discrete Random Variables#Cumulative Distribution Function (CDF)|discrete random variables]]

> [!def]+ Cumulative Distribution Function
> Let $X$ be a continuous random variable. The **cdf** of $X$ is the function
> $$F(x) = P(X \leq x) = \int_{-\infty}^{x} f(t)dt$$
> defined for all real numbers $x$

- If $F$ is differentiable at $x$, then we can take *derivatives* on both sides and use the fundamental theorem of calculus to get $$F'(x) = f(x)$$
    - → The **[[Introduction to Continuous Random Variables#Probability Density Functions (pdf)|pdf]]** is the derivative of the **cdf**
- Given a density for $X$, we can obtain the cdf and vice versa
    - → ==Either function can be used to specify the probability distribution of $X$==

## Continuous vs. Discrete CDF

- The properties of CDF are almost the same for discrete and continuous RVs
- PMF: Height of each step of the CDF
    - ![](https://i.imgur.com/4qIejhW.png)

- PDF: ==Rate of change== at each point of the CDF
    - ![](https://i.imgur.com/59KEggn.png)

# Properties of CDFs

> [!thm]+ Properties of CDFs
> A function $F$ is a cdf, that is, there exists a random variable $X$ whose cdf is $F$, if it satisfies the following properties:
> 1. $$\colorbox{#e6f3ff}{$\lim_{ x \to -\infty } F(x) = 0$}$$
> 2. $$\colorbox{#f0e6ff}{$\lim_{ x \to +\infty } F(x) = 1$}$$
> 3. If $a \leq b$, then $F(a) \leq F(b)$
> 4. $F$ is right-continuous. That is, for all real $a$, $$\lim_{ x \to a^{+} } F(x) = F(a)$$
>
> > [!tip] CDF for continuous RV is a continuous function

## Relationships between CDFs, PMFs, and PDFs

![](https://i.imgur.com/dNxCaPR.png)

# Example. Bus Waiting Time

You are taking the TTC bus and it is very unreliable. The amount of time in minutes that you wait for the bus can be modelled by the random variable $X$ with PDF $$f(x) = \frac{1}{5}e^{-\frac{x}{5}}$$for $x \in [0, \infty)$.

![|400|center](https://i.imgur.com/Bp5aNYu.png)


1. To find the CDF, we integrate the PDF from $-\infty$ to $x$:

$$\begin{align*}
F(x) = P(X \leq x) &= \int_{-\infty}^{x} f(t)dt \\
&= \int_{0}^{x} \frac{1}{5}e^{-\frac{t}{5}}dt \quad \text{(since $f(t)=0$ for $t<0$)} \\
&= -e^{-\frac{t}{5}}\Big|_{0}^{x} \\
&= -e^{-\frac{x}{5}} - (-e^{0}) \\
&= 1 - e^{-\frac{x}{5}} \quad \text{for } x \geq 0
\end{align*}$$

Therefore, the CDF is:
$$F(x) = \begin{cases}
0 & \text{if } x < 0 \\
1 - e^{-\frac{x}{5}} & \text{if } x \geq 0
\end{cases}$$

2. For the probability of waiting more than 10 minutes:

$$\begin{align*}
P(X > 10) &= 1 - P(X \leq 10) \\
&= 1 - F(10) \\
&= 1 - (1 - e^{-\frac{10}{5}}) \\
&= e^{-2} \\
&\approx 0.135
\end{align*}$$
```desmos-graph
left=-2; right=12;
top=2; bottom=-1;
---
f\left(x\right)\ =\ \left\{x\ >\ 0:\ 1-e^{-\frac{x}{5}},\ x\ \le\ 0\ :\ 0\right\}
```

*Graph of $F(x)$.*

# Interval Probabilities

### For Discrete RVs:

- If $a < b$: 
  $$P(a < X \leq b) = P(X \leq b) - P(X \leq a) = F(b) - F(a)$$
- The inequality signs matter because discrete RVs can take specific values

### For Continuous RVs:

- When $a < b$, all these expressions are equivalent: $$F(b) - F(a) = P(a < X \leq b) = P(a \leq X \leq b) = P(a \leq X < b) = P(a < X < b)$$
- For continuous RVs:
    - The probability of $X$ taking any exact value is 0
    - Therefore, including or excluding endpoints doesn't affect the probability
- This is a key difference from discrete RVs!

> [!tip] For continuous RVs, you can use any combination of strict (<) or non-strict (≤) inequalities when working with intervals - they all give the same probability!

### Example. Bus Waiting Time (continued)

Using the CDF found earlier, find the probability that you wait between 5 and 10 minutes for the bus.

For continuous RVs, when $a < b$:
$$P(a < X < b) = F(b) - F(a)$$

Therefore:
$$\begin{align*}
P(5 < X < 10) &= F(10) - F(5) \\
&= (1-e^{-\frac{10}{5}}) - (1-e^{-\frac{5}{5}}) \\
&= (1-e^{-2}) - (1-e^{-1}) \\
&= e^{-1} - e^{-2} \\
&\approx 0.2325
\end{align*}$$

```desmos-graph
left=-2; right=12;
top=0.7; bottom=-0.2;
---
y\ =\ \frac{1}{5}e^{-\frac{x}{5}}
0\le y\ \le\ \frac{1}{5}e^{-\frac{x}{5}}\left\{5\ <x<10\right\}
```

*Graph of $f(x)$. Shaded area is $P(5 < X < 10)$.*

![](https://i.imgur.com/nszal0U.png)

*This is equivalent to $P(5<X<10)$.*

### Example. Relationship between PDF and CDF

Verify that the PDF is indeed the derivative of the CDF for our bus waiting time example.

For $x \geq 0$:
$$\begin{align*}
\frac{d}{dx}F(x) &= \frac{d}{dx}(1-e^{-\frac{x}{5}}) \\
&= -e^{-\frac{x}{5}}(-\frac{1}{5}) \\
&= \frac{1}{5}e^{-\frac{x}{5}} \\
&= f(x)
\end{align*}$$

For $x < 0$:
$$\frac{d}{dx}F(x) = \frac{d}{dx}(0) = 0 = f(x)$$

> [!obs]+ Observation
> This verifies that $f(x) = F'(x)$ for all $x$, confirming the fundamental relationship between PDFs and CDFs for continuous random variables:
> - The ==PDF is the **derivative** of the CDF==
> - The ==CDF is the **integral** of the PDF==

# Quantiles for a Continuous RV

- ==Quantiles are the *inverse* of the CDF==
- **Median** of a distribution is its 50th percentile

> [!def]+ Quantile
> Let $0 < p < 1$.
> If $X$ is a continuous random variable, then the **$p$-th quantile** is the number $q$ that satisfies $$P(X \leq q) = \frac{p}{100}$$
> That is, the $p$-th quantile separates the bottom $p$ percent of the probability mass from the top $(1-p)\%$

## Example. Bus Waiting Time

What is the **median** of your wait times i.e., what is the 50th percentile of the wait times?

![](https://i.imgur.com/Yo1ePlo.png)

For the median, we need to find $q_{0.5}$ where:
$$F(q_{0.5}) = 0.5$$

Using our CDF:
$$1 - e^{-\frac{q_{0.5}}{5}} = 0.5$$

Solving for $q_{0.5}$:
$$\begin{align*}
e^{-\frac{q_{0.5}}{5}} &= 0.5 \\
-\frac{q_{0.5}}{5} &= \ln(0.5) \\
q_{0.5} &= -5\ln(0.5) \\
&\approx 3.466 \text{ minutes}
\end{align*}$$

# Expected Values of Continuous RV

- For discrete RVs, expectation was calculated using sums:
    - $E[g(X)] = \sum_{all\ x} g(x)P(X=x)$ where $P(X=x)$ is the PMF

- For continuous RVs, we replace sums with integrals:
    - $$E[g(X)] = \int_{-\infty}^{\infty} g(x)f(x)dx$$ where $f(x)$ is the PDF

## Variance

- Variance can be calculated using either formula:
    1. $$Var[X] = \int_{-\infty}^{\infty} (x-E[X])^2f(x)dx$$
    2. $$Var[X] = E[X^2] - E[X]^2$$

## Moment Generating Function (MGF)

- For continuous RVs, the MGF is defined as:
    - $M_X(t) = E(e^{tX}) = \int_{-\infty}^{\infty} e^{tx}f(x)dx$

> [!note] All properties of expectation, variance, and MGF that we learned for discrete random variables still apply to continuous random variables - only the method of calculation changes from sums to integrals.

## Example. Expected Bus Waiting Time

Let's find the expected amount of time you will wait for the bus.

Using the PDF $f(x) = \frac{1}{5}e^{-\frac{x}{5}}$, we calculate:

$$\begin{align*}
E[X] &= \int_{-\infty}^{\infty} xf(x)dx \\
&= \int_{0}^{\infty} x\cdot\frac{1}{5}e^{-\frac{x}{5}}dx \quad \text{(since $f(x)=0$ for $x<0$)}
\end{align*}$$

Using integration by parts with:
- $u = \frac{x}{5}$, $du = \frac{1}{5}dx$
- $dv = e^{-\frac{x}{5}}dx$, $v = -5e^{-\frac{x}{5}}$

$$\begin{align*}
E[X] &= \lim_{r \to \infty} \left[-xe^{-\frac{x}{5}} - 5e^{-\frac{x}{5}}\right]_0^r \\
&= \lim_{r \to \infty} \left[-re^{-\frac{r}{5}} - 5e^{-\frac{r}{5}} - (0 - 5e^0)\right] \\
&= 0 - 0 + 5 \\
&= 5
\end{align*}$$

Therefore, the expected (average) waiting time for the bus is 5 minutes.
