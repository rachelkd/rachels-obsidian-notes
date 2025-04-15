---
dg-publish: true
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 10
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "17"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/Dnr161wIkBMoLqOPJ5eblw
Date: 2024-11-12
Date created: Sun., Nov. 17, 2024, 6:23:33 pm
Date modified: Fri., Dec. 6, 2024, 6:12:14 pm
---

# Review: Joint vs. Marginal Probabilities

See [[Conditional Probability]]

![[Conditional Probability#Contingency Tables]]

![[Conditional Probability#Conditional Distributions]]

> [!example]+ Contingency Table Example - Ice Cream Survey
> Given data from survey of middle and high school students about ice cream preferences:
>
> |              | Vanilla | Chocolate | Strawberry |
> |--------------|---------|-----------|------------|
> |Middle School | 78      | 36        | 12         |
> |High School   | 53      | 47        | 29         |
> >
> > [!question]- Different Types of Probability Calculations
> >
> > 1. **Marginal Probability**
> >    - P(middle school) = $\frac{78 + 36 + 12}{255} = \frac{126}{255}$
> >    - Total students = 255 (sum of all cells)
> >
> > 2. **Joint Probability**
> >    - P(high school AND strawberry) = $\frac{29}{255}$
> >
> > 3. **Conditional Probability**
> >    - P(chocolate | high school) = $\frac{47}{129}$
> >    - Denominator = total high school students (53 + 47 + 29)
> >
> >    - P(middle school | vanilla) = $\frac{78}{131}$
> >    - Denominator = total vanilla preferences (78 + 53)
>
> > [!question]- Testing for Independence
> > Two events A and B are independent if $P(A|B) = P(A)$ for all values
> >
> > Testing if school level and ice cream preference are independent:
> >
> > - P(vanilla | middle school) = $\frac{78}{126}$
> > - P(vanilla | high school) = $\frac{53}{129}$
> >
> > Since these conditional probabilities are not equal, school level and ice cream preference are **not** independent.
>
> > [!important]+ Key Formulas
> >
> > - Marginal: $P(A) = \frac{\text{row or column total}}{\text{grand total}}$
> > - Joint: $P(A \text{ and } B) = \frac{\text{cell count}}{\text{grand total}}$
> > - Conditional: $P(A|B) = \frac{P(A \text{ and } B)}{P(B)} = \frac{\text{cell count}}{\text{row or column total}}$
> > - Independence: $P(A|B) = P(A)$ or equivalently $P(A \text{ and } B) = P(A)P(B)$

# Random Variables

> [!example]+ Two Dice Roll
> Consider rolling two fair dice:
>
> - $X$ = Number on dice 1, $X \sim \text{Unif}\{1,2,\ldots,6\}$
> - $Y$ = Number on dice 2, $Y \sim \text{Unif}\{1,2,\ldots,6\}$
> - $X$ and $Y$ are independent
>
> Joint PMF table (each cell = 1/36):
> ![|center|400](https://i.imgur.com/81dHM2K.png)
>
> > [!question]+ Probability Calculations
> >
> > 1. <mark style="background: #FFB86CA6;">P(Dice 1 = 2 and Dice 2 = 3)</mark>
> >    - Direct from joint PMF table: $\frac{1}{36}$
> >    - Since independent: $P(X=2)P(Y=3) = \frac{1}{6} \cdot \frac{1}{6} = \frac{1}{36}$
> >
> > 2. <mark style="background: #D2B3FFA6;">P(Dice 1 ≤ 2 and Dice 2 ≤ 3)</mark>
> >    - Count favourable outcomes: 6 cells (2 × 3 grid)
> >    - $P(X \leq 2, Y \leq 3) = \frac{6}{36} = \frac{1}{6}$
> >    - Since independent: $P(X \leq 2)P(Y \leq 3) = \frac{2}{6} \cdot \frac{3}{6} = \frac{1}{6}$
>
> > [!important]+ Key Points
> >
> > - For independent RVs:
> >   - $P(X=a, Y=b) = P(X=a)P(Y=b)$
> >   - Joint PMF has equal probabilities when uniform
> >   - Can multiply marginal probabilities for any event

# Joint PMFs and CDFs

> [!def]+ Joint PMF
> **Joint Probability Mass Function (PMF)** of $X$ and $Y$:
> $$p_{X,Y}(x,y) = P(X = x \cap Y = y)$$
>
> - Gives probability of specific $(x,y)$ pairs

> [!def]+ Joint CDF
> **Joint Cumulative Distribution Function (CDF)**:
> $$F_{X,Y}(a,b) = P(X \leq a \cap Y \leq b)$$ for $-\infty < a,b < \infty$
>
> - Gives probability of $X \leq a$ AND $Y \leq b$

## Visualization

![](https://i.imgur.com/V0IMBen.png)

- Joint PMFs can be represented as $3$D plots:
  - $X$-axis: values of first random variable
  - $Y$-axis: values of second random variable
  - $Z$-axis (height): probability of each $(x,y)$ pair

## Properties

 1. Probability over a region $A$:
    - For discrete pairs $(x,y)$ in sample space $\Omega$:
    - $$P((X,Y) \in A) = \sum_{(x,y)\in A} p_{X,Y}(x,y)$$
    - Sums joint PMF values over all points in region $A$
 2. Probability over intervals:
    - $$P(a \leq X \leq b, c \leq Y \leq d) = \sum_{x=a}^b \sum_{y=c}^d p_{X,Y}(x,y)$$
    - Used for rectangular regions in $xy$-plane
 3. Total Probability:
    - All probabilities must sum to $1$
    - If support of $X$ is $S$ and $Y$ is $T$:
    - $$\sum_{x\in S} \sum_{y\in T} p_{X,Y}(x,y) = 1$$

# From Joint to Marginal PMFs

> [!def]+ Marginal Distributions
> If $X$ takes values in a set $S$, and $Y$ takes values in a set $T$, then:
>
> **Marginal distribution of $X$**:
> $$P(X = x) = \sum_{y\in T} P(X = x, Y = y)$$
>
> **Marginal distribution of $Y$**:
> $$P(Y = y) = \sum_{x\in S} P(X = x, Y = y)$$
>
> Wagaman and Dobrow (2021), pp 136

## Example: Marginal PMFs for Two Dice

Consider rolling two fair dice:

- $X$ = Number on dice 1
- $Y$ = Number on dice 2
- Joint PMF: $p_{X,Y}(x,y) = \frac{1}{36}$ for $x,y \in \{1,2,3,4,5,6\}$, and $0$ otherwise

> [!example]+ Finding Marginal PMFs
> **For $X$ (Dice 1)**:
>
> - $P(X = x) = \sum_{y=1}^6 p_{X,Y}(x,y) = \sum_{y=1}^6 \frac{1}{36} = \frac{6}{36} = \frac{1}{6}$
> - Sum over all possible values of $Y$ (6 values)
>
> **For $Y$ (Dice 2)**:
>
> - $P(Y = y) = \sum_{x=1}^6 p_{X,Y}(x,y) = \sum_{x=1}^6 \frac{1}{36} = \frac{6}{36} = \frac{1}{6}$
> - Sum over all possible values of $X$ (6 values)

## Conditional PMFs

For the two dice example:

- $X$ = Number on dice 1
- $Y$ = Number on dice 2
- Joint PMF: $p_{X,Y}(x,y) = \frac{1}{36}$ for $x,y \in \{1,2,3,4,5,6\}$

> [!info]+ Conditional PMF
> To find conditional PMF:
>
> 1. Focus only on row/column being conditioned on
> 2. Rescale probabilities to sum to 1
> 3. $$P(Y|X=x) = \frac{P(Y \text{ and } X=x)}{P(X=x)}$$

> [!example]+ Finding $P(Y|X=5)$
> Looking at column where $X=5$:
>
> - Each cell in column has probability $\frac{1}{36}$
> - $P(X=5) = \frac{6}{36} = \frac{1}{6}$ (marginal probability)
> - Therefore, $P(Y|X=5) = \frac{\frac{1}{36}}{\frac{1}{6}} = \frac{1}{6}$ for all $y$
>
> This shows $Y$ is still uniform when conditioned on $X=5$, which makes sense since dice rolls are independent!

> [!def]+ Conditional PMFs for Discrete Random Variables
> For discrete random variables $X$ and $Y$:
>
> **Conditional PMF of $Y$ given $X$**:
> $$p_{Y|X}(y|X = x) = \frac{p_{X,Y}(x,y)}{p_X(x)}$$
>
> **Conditional PMF of $X$ given $Y$**:
> $$p_{X|Y}(x|Y = y) = \frac{p_{X,Y}(x,y)}{p_Y(y)}$$

# Summary: Two Discrete Random Variables

- Probabilities represented as heights in 3D plot
- Each point $(x,y)$ has associated probability height

> [!thm]+ Joint PMF
> $$p_{X,Y}(x,y) = P(X = x, Y = y)$$
>
> - Properties:
>   - Must be non-negative: $p_{X,Y}(x,y) \geq 0$
>   - Must sum to 1 over all pairs
> - For region $A$: $$P((X,Y) \in A) = \sum_{(x,y)\in A} p_{X,Y}(x,y)$$

> [!thm]+ Joint CDF
> $$F_{X,Y}(a,b) = P(X \leq a, Y \leq b)$$ for $-\infty < a,b < \infty$
>
> - Properties:
>   - Bounded between 0 and 1
>   - Non-decreasing in both variables

^dfc097

> [!thm]+ Marginal PMFs
> Sum over other variable:
>
> - For $X$: $P(X = x) = \sum_{all\,y} p_{X,Y}(x,y)$
> - For $Y$: $P(Y = y) = \sum_{all\,x} p_{X,Y}(x,y)$

> [!thm]+ Conditional PMFs
> Ratio of joint to marginal:
>
> - $p_{Y|X}(y|X = x) = \frac{p_{X,Y}(x,y)}{p_X(x)}$
> - $p_{X|Y}(x|Y = y) = \frac{p_{X,Y}(x,y)}{p_Y(y)}$

# Continuous Random Variables

## Joint PDF

> [!def]+ Joint Density Function
> For continuous random variables $X$ and $Y$ defined on a common sample space, the **joint density function** $f(x,y)$ has the following properties:
>
> 1. Non-negative: $f(x,y) \geq 0$ for all real numbers $x$ and $y$
>
> 2. Total probability equals 1:
> $$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x,y) \, dx \, dy = 1$$
>    - Represents ==total volume== under the PDF surface
>
> 3. Probability over region $S \subseteq \mathbb{R}^2$:
> $$P((X,Y) \in S) = \iint_S f(x,y) \, dx \, dy$$
>
> Wagaman and Dobrow (2021), pp 247

^f0872c

> [!note]- Key Differences from Discrete Case
>
> - Sums become double integrals
> - PMF becomes PDF (density function)
> - Probabilities found by integrating over regions
> - Height of PDF not directly interpretable as probability

> [!example]+ Finding Normalizing Constant for Joint PDF
> Given joint PDF: $f(x,y) = cxy$ for $1 < x < 4$ and $0 < y < 1$
>
> To find $c$, use the fact that total probability must equal 1:
>
> $$\begin{align*}
> 1 &= \int_0^1 \int_1^4 cxy \, dx \, dy \\
> &= c\int_0^1 y \left(\int_1^4 x \, dx\right) dy \\
> &= c\int_0^1 y \left(\frac{15}{2}\right) dy \\
> &= c\left(\frac{15}{2}\right)\left(\frac{1}{2}\right) \\
> &= \frac{15c}{4}
> \end{align*}$$
>
> Therefore:
> - $c = \frac{4}{15}$
> - Final joint PDF: $$f_{X,Y}(x,y) = \frac{4}{15}xy$$ for $1 < x < 4$ and $0 < y < 1$

### Visualization

- Joint PDFs can be visualized with a ==3D figure==
- Probabilities are *volumes* now
  - Instead of areas when we only had 1 RV

![](https://i.imgur.com/WdlB4n9.jpeg)

> [!example]+ Finding Probability from Joint PDF
> Given: $f(x,y) = \frac{4xy}{15}$ for $1 < x < 4$ and $0 < y < 1$
>
> Find: $P(2 < X < 3, Y > \frac{1}{4})$
>
> Solution:
> $$\begin{align*}
> P(2 < X < 3, Y > \frac{1}{4}) &= \int_{\frac{1}{4}}^1 \int_2^3 \frac{4xy}{15} \, dx \, dy \\[1em]
> &= \frac{4}{15} \int_{\frac{1}{4}}^1 y \left[\frac{x^2}{2}\right]_2^3 \, dy \\[1em]
> &= \frac{4}{15} \int_{\frac{1}{4}}^1 y \left(\frac{9}{2} - 2\right) \, dy \\[1em]
> &= \frac{4}{15} \int_{\frac{1}{4}}^1 y \left(\frac{5}{2}\right) \, dy \\[1em]
> &= \frac{4}{15} \cdot \frac{5}{2} \left[\frac{y^2}{2}\right]_{\frac{1}{4}}^1 \\[1em]
> &= \frac{4}{15} \cdot \frac{5}{2} \left(\frac{1}{2} - \frac{1}{32}\right) \\[1em]
> &= \frac{5}{16}
> \end{align*}$$
>
> > [!note]- Steps Explained
> > 1. Set up double integral with correct bounds
> > 2. Integrate with respect to $x$ first
> > 3. Simplify the inner integral result
> > 4. Integrate with respect to $y$
> > 5. Evaluate at the bounds

## Joint CDF

- Same as the “regular” CDF we already know, but
  - Integration region ==is a double integral==

> [!def]+ Joint Cumulative Distribution Function
> If $X$ and $Y$ have joint density function $f$, the **joint cumulative distribution function** of $X$ and $Y$ is
> $$
> F(x, y) = P(X \leq x, Y\leq y) = \int_{-\infty}^{x} \int_{-\infty}^{y} f(s, t) \, dt \, ds
> $$
> defined for all $x$ and $y$. Differentiating with respect to both $x$ and $y$ gives
> $$
> \frac{ \partial^{2} }{ \partial x \partial y } F(x, y) = f(x, y)
> $$
> Wagaman and Dobrow (2021), pp 248

^d0b0f5

> [!example]+ Finding Joint CDF Value
> Given: $f(x,y) = \frac{4xy}{15}$ for $1 < x < 4$ and $0 < y < 1$
>
> Find: $F_{X,Y}(3,0.25) = P(X \leq 3, Y \leq 0.25)$
>
> Solution:
> $$\begin{align*}
> F_{X,Y}(3,0.25) &= \int_1^3 \int_0^{0.25} \frac{4xy}{15} \, dy \, dx \\[1em]
> &= \int_1^3 \left[\frac{2xy^2}{15}\right]_0^{0.25} dx \\[1em]
> &= \int_1^3 \frac{2x(0.25)^2}{15} \, dx \\[1em]
> &= \int_1^3 \frac{x}{120} \, dx \\[1em]
> &= \left[\frac{x^2}{240}\right]_1^3 \\[1em]
> &= \frac{9-1}{240} = \frac{1}{30}
> \end{align*}$$
>
> > [!note]- Integration Steps
> > 1. Set up double integral with bounds from support
> > 2. Integrate with respect to $y$ first
> > 3. Evaluate at $y$ bounds and simplify
> > 4. Integrate with respect to $x$
> > 5. Evaluate at $x$ bounds

## Marginal Distributions from Joint PDFs

> [!def]+ Marginal PDFs
> For continuous random variables with joint PDF $f(x,y)$:
>
> **Marginal PDF of $X$**:
> $$f_X(x) = \int_{-\infty}^{\infty} f(x,y) \, dy$$
>
> **Marginal PDF of $Y$**:
> $$f_Y(y) = \int_{-\infty}^{\infty} f(x,y) \, dx$$
>
> Wagaman and Dobrow (2021), pp 254

![|center|400](https://i.imgur.com/YMzscwP.png)

> [!example]+ Finding Marginal PDF
> Given: $f(x,y) = \frac{4xy}{15}$ for $1 < x < 4$ and $0 < y < 1$
>
> Find marginal PDF of $X$:
> $$\begin{align*}
> f_X(x) &= \int_0^1 \frac{4xy}{15} \, dy \\[1em]
> &= \frac{4x}{15} \int_0^1 y \, dy \\[1em]
> &= \frac{4x}{15} \left[\frac{y^2}{2}\right]_0^1 \\[1em]
> &= \frac{2x}{15}
> \end{align*}$$
>
> > [!note]- Key Observation
> > The result $f_X(x)$ is only a function of $x$, as expected for a marginal distribution

## Conditional PDFs

> [!def]+ Conditional PDFs for Continuous Random Variables
> For continuous random variables $X$ and $Y$:
>
> **Conditional PDF of $Y$ given $X$**:
> $$f_{Y|X}(y|X = x) = \frac{f_{X,Y}(x,y)}{f_X(x)}$$
>
> **Conditional PDF of $X$ given $Y$**:
> $$f_{X|Y}(x|Y = y) = \frac{f_{X,Y}(x,y)}{f_Y(y)}$$
>
> Melchers (1999)

^b8409f

### Geometric Interpretation

- To find conditional PDF:
    1. “Fix” one variable (e.g., $X = x$)
    2. Take a slice of the joint PDF at that point
    3. Rescale the slice so its area equals 1

![|center|500](https://i.imgur.com/iynJY5d.png)

> [!note]- Visualization
>
> - Joint PDF shown as 3D surface
> - Marginal PDFs appear as curves on side planes
> - Conditional PDF is a rescaled slice of joint PDF
> - Area under each conditional PDF slice must equal 1

> [!example]+ Finding Conditional PDF
> Given: $f(x,y) = \frac{4xy}{15}$ for $1 < x < 4$ and $0 < y < 1$
>
> Find: $f_{Y|X}(y|X = x)$
>
> Solution:
> $$\begin{align*}
> f_{Y|X}(y|X = x) &= \frac{f_{X,Y}(x,y)}{f_X(x)} \\[1em]
> &= \frac{\frac{4xy}{15}}{\frac{2x}{15}} \\[1em]
> &= 2y \text{ for } 0 < y < 1 \text{ and } 0 \text{ otherwise}
> \end{align*}$$
>
> > [!important]- Special Case: Independence
> > In this example, $f_{Y|X}(y|X = x) = 2y$ doesn't depend on $x$. This means:
> > 1. The conditional distribution of $Y$ is the same regardless of what value we fix for $X$
> > 2. This is a special property indicating $X$ and $Y$ are **independent**
> > 3. Not all joint PDFs have this property! Usually, the conditional PDF will change based on the value we condition on
> >
> > For example, if $f(x,y) = \frac{x^2y}{k}$, then $f_{Y|X}(y|X = x) = \frac{x^2y}{c(x)}$ would depend on which $x$ value we fix

## Non-Rectangular Integration Regions

> [!warning]+ Integration Regions
> When finding probabilities from joint PDFs:
>
> - Integration regions are not always rectangular

- Must carefully identify the region based on the probability statement
- May need to change order of integration based on region shape

> [!example]+ Finding $P(X < Y)$ for Uniform Distribution
> Given: $f(x,y) = 1$ for $0 < x < 1, 0 < y < 1$ (i.e., $(X,Y) \sim \text{Unif}((0,1)^2)$)
>
> Find: $P(X < Y)$
>
> 1. **Visualize Region**:
>    - Region where $X < Y$ is below the line $y = x$
>    - Forms a triangle in the unit square
>
> ```desmos-graph
> left=0; right=1;
> top=1; bottom=0;
> ---
> y>x|BLUE|DASHED
> ```
>
> 2. **Set Up Integral**:
> $$P(X < Y) = \iint_{x<y} 1 \, dx \, dy = \int_0^1 \int_0^y 1 \, dx \, dy$$
>    - Outer integral: $y$ from 0 to 1
>    - Inner integral: $x$ from 0 to $y$ (not to 1!)
>
> 3. **Solve**:
>    $$\begin{align*}
>    P(X < Y) &= \int_0^1 \int_0^y 1 \, dx \, dy \\
>    &= \int_0^1 y \, dy \\
>    &= \frac{1}{2}
>    \end{align*}$$

# Summary: Two Continuous Random Variables

> [!thm]+ Joint PDF
>
> - Probabilities are volumes under the PDF in 3D plot
> - $f_{X,Y}(x,y)$ must be:
>   - Non-negative: $$f_{X,Y}(x,y) \geq 0$$
>   - Integrate to 1: $$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f_{X,Y}(x,y) \, dx \, dy = 1$$

> [!thm]+ Joint CDF
>
> - $$F_{X,Y}(a,b) = P(X \leq a, Y \leq b)$$ for $-\infty < a,b < \infty$
> - Properties:
>   - Bounded between 0 and 1
>   - Non-decreasing in both variables

> [!thm]+ Marginal PDFs
> Sum over other variable:
>
> - For $X$: $f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y) \, dy$
> - For $Y$: $f_Y(y) = \int_{-\infty}^{\infty} f_{X,Y}(x,y) \, dx$

> [!thm]+ Conditional PDFs
> Ratio of joint to marginal:
>
> - $f_{Y|X}(y|X = x) = \frac{f_{X,Y}(x,y)}{f_X(x)}$
> - $f_{X|Y}(x|Y = y) = \frac{f_{X,Y}(x,y)}{f_Y(y)}$

^84cf78

# Independence of Two RVs

> [!def]+ Independence for Continuous Random Variables
> Two continuous random variables $X$ and $Y$ are independent if and only if their ==joint density function is the product of their marginal densities==:
>
> - $$f_{X,Y}(x,y) = f_X(x)f_Y(y)$$ for all $x,y$
> - OR equivalently, if $$f_{Y|X}(y|X = x) = f_Y(y)$$ for all $x$
> - Also, for CDFs: $$F(x, y) = P(X \leq x, Y \leq y) = P(X \leq x)P(Y \leq y) = F_{X}(x)F_{Y}(y)$$

> [!example]+ Testing Independence with Uniform Distribution
> Given: $f(x,y) = 1$ for $0 < x < 1, 0 < y < 1$ (i.e., $(X,Y) \sim \text{Unif}((0,1)^2)$)
>
> **Find Marginal PDFs**:
>
> 1. For $X$:
> $$f_X(x) = \int_0^1 1 \, dy = 1, \quad 0 < x < 1$$
>    - $X \sim \text{Unif}(0,1)$
>
> 2. For $Y$:
> $$f_Y(y) = \int_0^1 1 \, dx = 1, \quad 0 < y < 1$$
>    - $Y \sim \text{Unif}(0,1)$
>
> See [[Continuous Uniform Distribution]].
>
> **Test Independence**:
>
> - Check if $f_{X,Y}(x,y) = f_X(x)f_Y(y)$
> - $1 = 1 \cdot 1$ ✓
> - Therefore, $X$ and $Y$ are independent

> [!tip]+ Quick Test for Independence
> Two continuous RVs are independent if:
>
> 1. Joint PDF is **separable** (can be written as product $g(x)h(y)$)
> 2. Support region is **rectangular** (bounds of $X$ don’t depend on $Y$ and vice versa)

In this example:

- $f(x,y) = 1 = 1 \cdot 1$ is separable
- Support $[0,1] \times [0,1]$ is rectangular

> [!example]+ Testing Independence with Exponential Joint PDF
> Given: $f(x,y) = 6e^{-(2x+3y)}$ for $x,y > 0$ and $0$ otherwise
>
> **Check Separability**:
> Can we write $f(x,y)$ as product of functions of $x$ and $y$?
> $$\begin{align*}
> f(x,y) &= 6e^{-(2x+3y)} \\
> &= 6e^{-2x}e^{-3y} \\
> &= (2e^{-2x})(3e^{-3y}) \\
> &= g(x)h(y) \quad ✓
> \end{align*}$$
>
> **Check Support**:
> - $x > 0$ only depends on $x$
> - $y > 0$ only depends on $y$
> - Support region is rectangular ✓
>
> **Distributions**:
> - $X \sim \text{Exp}(2)$ (rate parameter = 2)
> - $Y \sim \text{Exp}(3)$ (rate parameter = 3)
> - See [[Exponential Distribution]]
>
> **Conclusion**:
> - Joint PDF is separable
> - Support is rectangular
> - Therefore, $X$ and $Y$ are independent

> [!example]+ Testing Independence: Non-Rectangular Support
> Given: $f(x,y) = 15e^{-(2x+3y)}$ for $0 < x < y$ and $0$ otherwise
>
> **Check Support Region**:
>
> - Condition $0 < x < y$ means support is triangular
> - Support of $X$ depends on value of $Y$
> - Support is not rectangular ✗
>
> ```desmos-graph
> left=-1; right=5;
> top=5; bottom=-1;
> ---
> y>x|x>0|BLUE|DASHED
> ```
>
> **Check Separability**:
> $$\begin{align*}
> f(x,y) &= 15e^{-(2x+3y)} \\
> &= 15e^{-2x}e^{-3y}
> \end{align*}$$
> - Cannot factor joint density into the ==product of the marginal densities==
>
> Therefore, $X$ and $Y$ are **not** independent

# Expected Value of Functions of Two RVs

> [!def]+ Expected Value for Discrete Joint Distributions
> For a function $g(X,Y)$ of discrete random variables $X$ and $Y$:
> $$E[g(X,Y)] = \sum_{x\in S} \sum_{y\in T} g(x,y)P(X = x, Y = y)$$
> where $S$ and $T$ are the sample spaces of $X$ and $Y$ respectively.

^7711bc

> [!def]+ Expected Value for Continuous Joint Distributions
> For a function $g(X,Y)$ of continuous random variables $X$ and $Y$ with joint density $f(x,y)$:
> $$E[g(X,Y)] = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} g(x,y)f(x,y) \, dx \, dy$$

^57163f

## Conditional Expectations

^f27ff3

> [!def]+ Conditional Expected Value
> For a function $g$ of random variables $X$ and $Y$:
>
> **Conditioning on $Y$ (function of $y$)**:
> $$E[g(X)|Y = y] = \begin{cases}
> \sum_{all\,x} g(x)p_{X|Y}(x|y) & \text{discrete case} \\[1em]
> \int_{all\,x} g(x)p_{X|Y}(x|y)\,dx & \text{continuous case}
> \end{cases}$$
>
> **Conditioning on $X$ (function of $x$)**:
> $$E[g(Y)|X = x] = \begin{cases}
> \sum_{all\,y} g(y)p_{Y|X}(y|x) & \text{discrete case} \\[1em]
> \int_{all\,y} g(y)p_{Y|X}(y|x)\,dy & \text{continuous case}
> \end{cases}$$

^f90dcd

- $E[g(X)|Y=y]$ may depend on $y$
- $E[g(Y)|X=x]$ may depend on $x$

> [!example]+ Example: Linear Relationship
> Given:
>
> - $Y \sim \text{Unif}(0,1)$
> - $X = 2Y$
>
> Then:
> $$E[X|Y=y] = 2y$$
>
> > [!note]- Interpretation
> >
> > - Result is a function of $y$
> > - Makes sense since $X$ is deterministic given $Y$
> > - No need for integration since $X$ is fully determined by $Y$

## Properties of Expectation

Recall [[Expectations and Variances of Random Variables#Properties of Expectation]].

> [!thm]+ Key Properties of Expectation
> For random variables $X$ and $Y$, and constants $a$ and $b$:
>
> 1. **Constant Expectation**:
>    - $E(a) = a$
>
> 2. **Linearity of Expectation**:
>    - $E(X + Y) = E(X) + E(Y)$
>    - $E(a + bX) = a + bE(X)$
>
> 3. **Independence Properties**:
>    - If $X$ and $Y$ are independent:
>      - $E[XY] = E[X]E[Y]$
>      - $E[f(X)g(Y)] = E[f(X)]E[g(Y)]$
>
> > [!warning]- Common Mistakes
> >
> > 1. In general, $E[f(X)] \neq f(E[X])$
> >
> > 2. Expected value may not be possible outcome
> >    - Example: Fair six-sided die
> >    - $E[X] = 3.5$, but can’t roll 3.5

## Properties of Variance

See [[Expectations and Variances of Random Variables#Properties of Variance]].

> [!def]+ Variance
> Variance is a type of expected value:
> $$\text{Var}(X) = E((X - E[X])^2) = E(X^2) - E(X)^2$$

> [!thm]+ Key Properties of Variance
> For random variables $X$ and $Y$, and constants $a$ and $b$:
>
> 1. **Constant Variance**:
>    - $\text{Var}(a) = 0$
>
> 2. **Linear Transformation**:
>    - $\text{Var}(a + bX) = b^2\text{Var}(X)$
>    - $\text{SD}(a + bX) = |b|\text{SD}(X)$
>
> 3. **Sum of RVs**:
>    - $\text{Var}(aX + bY) = a^2\text{Var}(X) + b^2\text{Var}(Y) + 2ab\text{Cov}(X,Y)$
>    - If $X,Y$ independent:
>        - $\text{Cov}(X, Y) = 0$
>        - $\text{Var}(aX + bY) = a^2\text{Var}(X) + b^2\text{Var}(Y)$
>
> 4. **Non-negativity**:
>    - Variance and standard deviation are always non-negative

# Covariance and Correlation

Wagaman and Dobrow (2021), pp 262

> [!def]+ Covariance
> For jointly distributed RVs $X$ and $Y$ with means $\mu_X = E[X]$ and $\mu_Y = E[Y]$:
>
> $$\begin{align*}
> \text{Cov}(X,Y) &= E[(X - \mu_X)(Y - \mu_Y)] \\
> &= E[XY] - E[X]E[Y]
> \end{align*}$$
>
> **For Continuous RVs**:
> $$\text{Cov}(X,Y) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} (x - \mu_X)(y - \mu_Y)f(x,y) \, dx \, dy$$

> [!def]+ Correlation Coefficient
> $$\rho_{X,Y} = \rho(X,Y) = \text{Corr}(X,Y) = \frac{\text{Cov}(X,Y)}{\sigma_X\sigma_Y}$$
> where $\sigma_X = \sqrt{\text{Var}(X)}$ and $\sigma_Y = \sqrt{\text{Var}(Y)}$
>
> **Properties**:
>
> - Always bounded: $-1 \leq \rho(X,Y) \leq 1$
> - Standardized version of covariance
> - Dimensionless measure of linear association

1. **Variance** ($\text{Var}(X)$):
    - Measures variability of a single RV
    - Always non-negative

2. **Covariance** ($\text{Cov}(X,Y)$):
    - Measures joint variability of two RVs, or
    - Degree of association
    - Positive: $X$ and $Y$ tend to increase together
    - Negative: As $X$ increases, $Y$ tends to decrease
    - Zero: No linear relationship (but might have non-linear relationship)

3. **Correlation** ($\rho(X,Y)$):
    - Standardized measure of covariance (linear relationship)
    - +1: Perfect positive linear relationship
    - -1: Perfect negative linear relationship
    - 0: No linear relationship

- Zero correlation/covariance doesn’t imply independence
    - Only implies no linear relationship
    - Could still have non-linear relationship

## Correlation and Linear Dependence

> [!important]+ Key Relationship with Independence
>
> - If $X$ and $Y$ are independent: $\text{Cov}(X,Y) = 0$ and $\rho(X,Y) = 0$
>   - Converse is **not** true!
>   - $X, Y$ can be related but have 0 correlation
> - Zero correlation/covariance only implies ==no linear relationship==

- Correlation and covariance only measure **linear dependence**

![](https://i.imgur.com/nk8Yaaw.png)

## Properties of Covariance and Correlation

> [!thm]+ Key Properties
>
> 1. **Self-Covariance**:
>    $$\begin{align*}
>    \text{Cov}(X,X) &= E[(X-E(X))(X-E(X))] \\
>    &= E[(X-E(X))^2] \\
>    &= \text{Var}(X)
>    \end{align*}$$
>
> 2. **Correlation Bounds**:
>    - $-1 \leq \rho(X,Y) \leq 1$ for any RVs $X$ and $Y$
>    - $\rho(X,Y) = \pm 1$ if and only if $Y = aX + b$ where $a \neq 0$
>      - $+1$: perfect positive linear relationship ($a > 0$)
>      - $-1$: perfect negative linear relationship ($a < 0$)
>
> 2. **Linear Combinations**:
>    - $\text{Cov}(aX + b, cY + d) = ac\text{Cov}(X,Y)$
>    - Constants $b$ and $d$ don’t affect covariance
>
> 3. **Additivity**:
>    - $\text{Cov}(X + Y, Z) = \text{Cov}(X,Z) + \text{Cov}(Y,Z)$
>    - Covariance is linear in both arguments

> [!example]+ Example: Using Properties
> Given: $\text{Cov}(X,Y) = 4$ and $\text{Var}(X) = 9$
>
> Find $\text{Cov}(2X + 3, -Y + 1)$:
> $$\begin{align*}
> \text{Cov}(2X + 3, -Y + 1) &= 2(-1)\text{Cov}(X,Y) \\
> &= -2(4) \\
> &= -8
> \end{align*}$$
>
> Find $\text{Cov}(X,X)$:
> $$\text{Cov}(X,X) = \text{Var}(X) = 9$$

## Correlation is **not** Causation

- You collect data on elementary school aged children’s reading ability, and their height and weight
- You fit a **linear regression** and find that the linear association between height and reading ability is very strong

> [!question]+ What can you conclude?
>
> - Even with a strong correlation between two RVs ==does not imply== a **causal** relationship
