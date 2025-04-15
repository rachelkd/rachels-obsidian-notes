---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 12
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 2024-11-28
Date created: Thu., Dec. 5, 2024, 3:16:17 pm
Date modified: Thu., Dec. 5, 2024, 3:57:54 pm
aliases: []
---

# Multivariate Normal Distribution

- Useful joint distribution of normal random variables that comes up in practice

> [!def]+ Multivariate Normal Distribution
> Joint PDF is given by
>
> $$
> f_{X, Y}(x, y) = \frac{1}{2\pi\sigma_{X}\sigma_{Y}\sqrt{ 1 - \rho^{2} }}\exp \left( -\frac{1}{2(1 - \rho^{2})} \left\{ \left( \frac{{x-\mu_{X}}}{\sigma_{X}} \right)^{2} + \left( \frac{{y - \mu_{Y}}}{\sigma_{Y}} \right)^{2} - \frac{{2\rho(x - \mu_{X})(y - \mu_{Y})}}{\sigma_{X}  \sigma_{Y}}  \right\}  \right) 
> $$
> for $-\infty \leq x \leq \infty$ and $-\infty \leq y \leq \infty$.
>
> - Five parameters: $\mu_{X}, \mu_{Y}, \sigma_{X}, \sigma_{Y}, \rho$
>     - $\rho$ is the *[[Probability Distributions of More than One RV#Covariance and Correlation|correlation]]* between $X$ and $Y$

Joint PDF defines a surface such as pictured below:

![|549](https://i.imgur.com/p1KeWkD.png)

- Marginal PDFs
    - $N(\mu_{X}, \sigma_{X}^{2})$ and $N(\mu_{Y}, \sigma_{Y}^{2})$ respectively
- Conditional distributions for $X | Y = y$ and for $Y | X = x$
    - are *univariate normal*
$$
\begin{align}
E(X|Y=y)  & = \mu_{X} + \rho\sigma_{X} \left( \frac{{y-\mu_{Y}}}{\sigma_{Y}} \right) &&& Var(X | Y = y) & = \sigma_{X}^{2}(1 - \rho^{2})  \\
E(Y | X = x)  & = \mu_{Y} + \rho\sigma_{Y} \left( \frac{{x - \mu_{X}}}{\sigma_{X}} \right) &&& Var(Y | X - x)  & = \sigma_{Y}^{2} (1 - \rho^{2})
\end{align}
$$

## Multivariate Distributions in R

- R functions for bivariate normal can be accessed through `mvtnorm` package

```r
install.packages("mvtnorm")
library(mvtnorm)
```

Compute the PDF, CDF, and generate $\binom {X}{Y}$ values for the bivariate normal distribution with mean $\binom{\mu_{X}}{\mu_{Y}}$ and covariance matrix $\begin{pmatrix} \sigma_{X}^{2} & \sigma_{XY} \\ \sigma_{XY} & \sigma_{Y}^{2} \end{pmatrix}$.

> [!important]+ $\texttt{dmvnorm(c(x, y), mean = c(} \mu_{X}, \mu_{Y} \texttt{), sigma = matrix(c(} \sigma_{X}^{2}, \sigma_{XY}, \sigma_{XY}, \sigma_{Y}^{2} \texttt{), nrow = 2))}$
> - Density evaluated at $(x, y)$

> [!important]+ $\texttt{pmvnorm(upper = c(x, y), mean = c(} \mu_{X}, \mu_{Y} \texttt{), sigma = matrix(c(} \sigma_{X}^{2}, \sigma_{XY}, \sigma_{XY}, \sigma_{Y}^{2} \texttt{), nrow = 2))}$
> - CDF evaluated at $(x, y)$

> [!important]+ $\texttt{rmvnorm(n, mean = c(} \mu_{X}, \mu_{Y} \texttt{), sigma = matrix(c(} \sigma_{X}^{2}, \sigma_{XY}, \sigma_{XY}, \sigma_{Y}^{2} \texttt{), nrow = 2))}$
> - Generates $n$ realizations of $(X, Y)$

> [!note]+ *Instead* of specifying the sigma matrix in the $\texttt{pmvnorm}$ function:
> - Specify the correlation matrix as $\texttt{corr= matrix(c(1, } \rho_{XY}, \rho_{XY}, 1\texttt{), nrow = 2)}$.
> - The covariance matrix $\begin{pmatrix} \sigma_{X}^{2} & \sigma_{XY} \\ \sigma_{XY} & \sigma_{Y}^{2} \end{pmatrix}$ is needed for the other two functions.

## Example

Suppose $N(\mu_{X}, \sigma_{X}^{2})$ and $N(\mu_{Y}, \sigma_{Y}^{2})$ and $X, Y$ are independent.

> [!question]+ Write down the joint PDF $f_{X, Y}(x, y)$
> - Since $X$ and $Y$ are independent, the joint PDF is:
> $$f_{X,Y}(x,y) = f_X(x) \cdot f_Y(y) = \frac{1}{\sqrt{2\pi}\sigma_X} e^{-\frac{(x-\mu_X)^2}{2\sigma_X^2}} \cdot \frac{1}{\sqrt{2\pi}\sigma_Y} e^{-\frac{(y-\mu_Y)^2}{2\sigma_Y^2}}$$

> [!question]+ What is the conditional distribution of $X | Y = y$ when $X, Y$ are independent?
> - Since $X$ and $Y$ are independent, the conditional distribution $X | Y = y$ is simply $X$ itself:
> $$f_{X|Y}(x|y) = f_X(x) = \frac{1}{\sqrt{2\pi}\sigma_X} e^{-\frac{(x-\mu_X)^2}{2\sigma_X^2}}$$

> [!example]+ Let $\binom{X}{Y}$ follow a bivariate normal distribution with $\mu_{X} = \mu_{Y} = 0$ and $\sigma_{X} = \sigma_{Y} = 1$.
>
> > [!question]+ Compare $E(X)$ and $V(X)$ to $E(X | Y = y)$ and $V(X | Y = y)$ when $\rho = 0.7$ and $\rho = -0.9$.
> > - **Expectation**:
> >   - $E(X) = \mu_X = 0$
> >   - $E(X|Y = y) = \mu_X + \rho \frac{\sigma_X}{\sigma_Y}(y - \mu_Y) = \rho y$
> > - **Variance**:
> >   - $V(X) = \sigma_X^2 = 1$
> >   - $V(X|Y = y) = (1 - \rho^2)\sigma_X^2 = 1 - \rho^2$
>
> > [!question]+ Use R to compute the CDF at $X = 0.05$ and $Y = 0$ when $\rho = 0.7$ and $\rho = -0.9$.
> > - Use the `pmvnorm` function from the `mvtnorm` package in R:
> > ```R
> > library(mvtnorm)
> > pmvnorm(lower = c(-Inf, -Inf), upper = c(0.05, 0), mean = c(0, 0), sigma = matrix(c(1, rho, rho, 1), nrow = 2))
> > ```
>
> > [!question]+ Randomly generate 10 $(x, y)$ values when $X$ and $Y$ are independent in R.
> > - Use the `rnorm` function in R:
> > ```R
> > x <- rnorm(10, mean = 0, sd = 1)
> > y <- rnorm(10, mean = 0, sd = 1)
> > data.frame(x, y)
> > ```
