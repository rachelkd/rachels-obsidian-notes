---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 12
Module:
  - "[[5 - Bivariate Probability Distributions]]"
Lecture:
  - "21"
  - "22"
Chapter:
Slides/Notes:
Date: 2024-11-26
Date created: Tue., Dec. 3, 2024, 12:15:02 am
Date modified: Fri., Dec. 6, 2024, 9:06:59 pm
aliases: [CLT]
---

# Central Limit Theorem (CLT)

> [!tldr]+ We know the sample mean [[Law of Large Numbers|converges to the true mean]], but what about its distribution?
>
> - Just because $\overline{X_{n}}$ will converge to $\mu$, this does not mean that $\overline{X_{n}}$ will exactly equal to $\mu$
> - $\overline{X_{n}}$ is a random variable
>     - Since it is a function of random variables
> - Distribution of $\overline{X_{n}}$:
>     - Will describe what happens as samples of your experiment is repeated

- First, try to figure out the *expected value* and *variance* of $\overline{X_{n}}$

## Expected Value of $\overline{X_{n}}$

Recall: $\overline{X_{n}} = \frac{{X_{1} + X_{2} + \dots + X_{n}}}{n}$, where each of the $X_{i}$‘s are i.i.d. with mean $\mu$ and variance $\sigma^{2}$.
$E(\overline{X_{n}})$ is the mean of the *sample mean*

$$
\begin{align}
E(\overline{X_{n}})  & = E \left( \frac{{X_{1} + \dots + X_{n}}}{n} \right)  \\
 & = \frac{1}{n} E(X_{1} + \dots + X_{n}) \\
 & = \frac{1}{n} \left( E(X_{1}) + \dots + E(X_{n}) \right)  \\
 & = \frac{1}{n} (\mu + \dots + \mu) \\
 & = \frac{1}{n} (n \mu) \\
 & = \mu
\end{align}
$$

## Variance of $\overline{X_{n}}$

$$
\begin{align}
Var(\overline{X_{n}})  & = Var \left( \frac{{X_{1} + \dots + X_{n}}}{n} \right)  \\
 & = \frac{1}{n^{2}} Var \big(X_{1} + \dots + X_{n}\big) && (\text{since }X_{i} \text{'s are independent}) \\
 & = \frac{1}{n^{2}} \bigg[Var(X_{1}) + \dots + Var(X_{n}) \bigg] \\
 & = \frac{1}{n^{2}} (\sigma^{2} + \dots + \sigma^{2}) \\
 & = \frac{n\sigma^{2}}{n^{2}} \\
 & = \frac{\sigma^{2}}{n}
\end{align}
$$

## Sample Mean of Normal Observations

> [!obs]+ Claim 1.
> When measurements are random values that follow a normal distribution, the probability distribution of sample means is also a normal distribution.

- Let $X_{1}, \dots, X_{n}$ be our random sample data
- $\overline{X}$ is the sample mean

> [!goal] If $X_{i} \sim N(\mu, \sigma^{2})$, then $\overline{X} \sim N(\quad, \quad)$

- Due to the fact that if you average independent normal variables, it is still normal
    - Recall [[Using MGFs to Determine the Probability Distribution of the Sum of IID RVs#The Sum of Independent Normal RVs is Normal]]

> [!obs] Claim 2.
> The mean of the normal probability distribution of the sample means is the same as the mean of the probability distribution of the individual measurements.

- Let $X_{1}, \dots, X_{n}$ be our random sample data, and $\overline{X}$ is the sample mean

> [!goal] If $X_{i} \sim N(\mu, \sigma^{2})$, then $\overline{X} \sim N(\mu, \quad)$

- This is because $E(\overline{X}) = E(X)$

> [!obs] Claim 3.
>
> - The standard deviation of the probability distribution of the sample means is *smaller* than the standard deviation of the probability distribution of the individual observations.
> - If there are $n$ values in the random sample, and $\sigma$ is the standard deviation of the probability distribution of the individual observations, the standard deviation of the probability distribution of the sample means is $\frac{\sigma}{\sqrt{ n }}$

- Let $X_{1}, \dots, X_{n}$ be our random sample data, and $\overline{X}$ is the sample mean

> [!thm] Distribution of Sample Means
> If $X_{i} \sim N(\mu, \sigma^{2})$, then $\overline{X} \sim N\left( \mu, \frac{\sigma^{2}}{n} \right)$

- This is because $Var(\overline{X}) = \frac{Var(X)}{n}$

### Example. IQ Scores

- IQ measurements are designed to follow a normal distribution with a mean of 100 and a standard deviation of 15.
- Suppose you collect a random sample of 5 individuals and measure their IQ scores.

> [!example]+ State the probability distribution of the sample mean of these 5 measurements.
> We are given:
>
> - $n = 5$
> - $\mu = 100$
> - $\sigma = 15$
>
> Then, $\overline{X} \sim N\left( \mu = 100, \frac{\sigma^{2}}{n} = \frac{15^{2}}{5} = 45 \right)$
>
> $$\overline{X} \sim N(100, 45)$$

## Sample Mean of Observations for *Any* Observation

### Not Normally Distributed

- We have just discussed a case when the individual observations were normally distributed
    - What if they are not normally distributed?
    - → Leads us into a different but similar result about the sample mean

### Central Limit Theorem Definition

- **Central Limit Theorem**
    - For *large enough sample size*, the probability distribution of sample means is *approximately* a Normal distribution
        - Regardless of the probability distribution of the individual measurements
    - $$\overline{X_{n}} \approx N\left( \mu, \frac{\sigma^{2}}{n} \right)$$

> [!thm]+ Central Limit Theorem (CLT)
> Let $X_{1}, X_{2}, \dots$ be an i.i.d. sequence of random variables with ==finite mean $\mu$== and ==variance $\sigma^{2}$==.
> For $n = 1, 2, \dots$, let $S_{n} = X_{1} + \dots + X_{n}$.
> Then, the distribution of the standardized random variable $\left( \frac{S_{n}}{n} - \mu \right)/\left( \frac{\sigma}{\sqrt{ n }} \right)$ converges to a standard normal distribution in the following sense:
> $$
> P \left( \frac{{\frac{S_{n}}{n} - \mu}}{\frac{\sigma}{\sqrt{ n }}} \leq t \right) \to P(Z \leq t), \quad \text{as }n \to \infty
> $$
> where $Z \sim \text{Norm}(0, 1)$

### Distribution of $\overline{X_{n}}$ when $X_{i}$‘s Are not Normal

- **CLT**
    - Tells us about the ==distribution of $\overline{X_{n}}$==
    - States that the ==distribution of $\overline{X_{n}}$ converges to the Normal distribution==
    - $N \left( \mu, \frac{\sigma^{2}}{n} \right)$ as $n$ increases, *regardless* of the original distribution
- As more and more measurements are taken, the probability distribution for the possible averages of the measurements will converge to a Normal distribution
- $n$ must be *large enough* for the distribution of the sample mean to resemble a Normal distribution
    - ? What is large?
        - Many classes use a cutoff of $n = 30$ (or if you are explicitly told to use CLT)
        - In real life, what counts as *large* depends on the context, the research question, etc.
            - Need to defer to experts in that specific field

![](https://i.imgur.com/n8WwyfB.jpeg)

## Example. CLT

- Consider the data on the time between subway arrivals
- The data is *not* normal
- Most come between 0-4 mins, but some are very delayed

![|center|500](https://i.imgur.com/CHOhHAY.png)

- Suppose we take 10 measurements
    - We have a sample size of 10
- Then, we calculate the sample mean
- Let’s repeat this 1000s of times:
    - Get a sample of size 10, and calculate the sample mean
- Examine the distribution of the sample mean by plotting all those sample means in a histogram:
    - ![|500](https://i.imgur.com/v3dQcbr.png)

<!-- break -->
- Do the same thing for a sample size $n = 25$ and sample size $n = 50$
- The larger the sample size $n$, the more the distribution of $\overline{X}$ resembles a normal distribution
- Result holds true, even when original measurements were not even close to a normal distribution:
    - ![|500](https://i.imgur.com/XU5VtKA.png)

## Example. Potato Chips

- Weight of potato chips in a medium-size bag is stated to be 300g
- Amount that the packaging machine puts in the bag is believed to follow a distribution with mean 306g and standard deviation 3.6g
- We want to determine how often the machine is overfilling the bags, so we take a ==sample of 100 bags==
    - “Sample of 100 bags” → Should use CLT

> [!example]+ What is the probability that the ==average of the sampled bags== has weight more than 305g?
>
> - “Average of the sampled bags” → Should use CLT
> - Original distribution: $X_i \sim$ distribution with mean $306g$ and variance $3.6^2$
> - By CLT: $\bar{X}_n \sim N(306, \frac{3.6^2}{n})$
> - For large $n$, $\bar{X}_n \sim N(306, 0.36^2)$ approximately where $0.36^2 = \frac{3.6^2}{100}$
> - Need to find: $P(\bar{X}_n > 305)$
> - Using standard normal:
>     $$\begin{align}
>     1 - \Phi\left(\frac{305 - 306}{0.36}\right) &= 1 - \Phi(-2.78) \\
>     &= 1 - 0.003 \\
>     &= 0.997
>     \end{align}$$
> - Therefore, $P(\bar{X}_n > 305) \approx 99.7\%$

## Some Notes about CLT

- Convergence described by the CLT is called **convergence in distribution**
- We say that random variables $X_{1}, X_{2}, \dots$ converge in distribution to $X$, if
    - For all $t$, $P(X_{n} \leq t) \to P(X \leq t)$, as $n \to \infty$
- Proof of CLT comes from taking the limit as $n \to \infty$ of the MGF of $\frac{{\overline{X_{n}} - \mu}}{\frac{\sigma}{\sqrt{ n }}}$ and showing that this limit is $e^{t^{2}/2}$

> [!info]+ Difference between $X_n$ and $X$ in Convergence in Distribution
>
> - $X_n$ represents a sequence of random variables
>     - In CLT context: $X_n = \frac{\overline{X_n} - \mu}{\sigma/\sqrt{n}}$ (the standardized sample mean)
>     - Each $n$ gives us a different random variable
> - $X$ represents the limiting distribution
>     - In CLT context: $X \sim N(0,1)$ (the standard normal distribution)
>     - This is the distribution that $X_n$ approaches as $n \to \infty$

> [!example]+ Visual Intuition
>
> - Think of $X_n$ as histograms that gradually become more bell-shaped
> - As $n$ gets larger:
>     - $X_1$ might be very different from normal
>     - $X_{10}$ starts looking somewhat normal
>     - $X_{100}$ looks very close to normal
>     - $X_{1000}$ looks almost exactly normal
> - $X$ is the perfect bell curve these histograms approach

## Sample Size for CLT

- Many textbooks use the cutoff of $n = 30$ for “large enough $n$ for CLT”
    - Not a hard rule

> [!tip] How large the sample size needs to be for CLT to apply changes is greatly based on the ==context== of the problem.

> [!example]+ You are interested in the proportion of male students in your program. The probability of a student being male is 0.45. If we collect data from 30 students, will the sample proportion of males approximately follow a normal distribution?
>
> > [!note]- R Code
> >
> > ```r
> > n = 30
> > p = 0.45
> > means <- c()  # Create an empty vector to store the means
> >
> > for (i in c(1:10000)) {
> > sample = rbinom(n, 1, p)  # Generate n values from the *same* binomial distribution. Change n to see what happens
> >     means = c(means, mean(sample))  # Append the mean of those values to the vector
> > }
> >
> > hist(means, breaks = 20, probability = TRUE, main = paste0("Sample proportions with p = ", p, "and n = ", n), xlab = "SAmple Means")  # Approximately the shape of the normal distribution
> >
> > # Plot the PDF
> > x <- seq(0, 1, length = 4000)
> > fun <- dnorm(x, mean = p, sd = sqrt(p * (1 - p) / n))
> > lines(x, fun, col = "darkblue", lwd = 2)
> > abline(v = p, col = "red")  # Adds a vertical line at mean
> > ```
>
> ![|center|500](https://i.imgur.com/wKvGisD.png)
>
> - If $p = 0.45, n = 30$, approximation is probably ok, but bigger will still be better
>     - → Try again with $n = 100$
>
> ![|center|500](https://i.imgur.com/58bTInr.png)

> [!example]+ You ask students if they have ever visited the Arctic. Assume the proportion of students who have visited is 1%.
>
> - If we use $n = 100$, the approximation using the normal distribution is not very good
>     - ![|500](https://i.imgur.com/BJrCBpb.png)
> - At $n = 1000$, the distribution of sample means is a little bit asymmetric
>     - Recall that normal distribution is always symmetric about its mean
>     - ![|500](https://i.imgur.com/ZQSfRKy.png)
> - $n = 10000$ is a good approximation using CLT
>     - ![|500](https://i.imgur.com/aIr18Vw.png)

> [!obs] The sample size required for a good approximation using CLT will vary greatly by context.

## Example. Hourly Wage

> [!important]+ CLT gives us the **sampling distribution** of the mean
>
> - We can use it to approximate probabilities

- Suppose the mean hourly wage of second-year UofT students who do a work-study is $17.84.
- Suppose that these wages follow a normal distribution, with a standard deviation of $\sigma = \$2.50$
- Let $X$ be the student’s wage
- $X \sim \text{Norm}(17.84, 2.5^{2})$

> [!question]+ What is the probability that the hourly wage of a *randomly selected student* from this population is greater than $19?
>
> - “Randomly selected student” → Do not use CLT
>
> $$
> \begin{align}
> P(X > 19) & = 1 - P(X \leq 19) \\
>  & \approx 0.321
> \end{align}
> $$
>
> ```r
> 1 - pnorm(19, 17.84, 2.5)
> [1] 0.3213239
> ```

> [!question]+ Suppose *a student’s* earnings are at the 78th percentile. How much did they earn?
>
> - “A student” → Do not use CLT
>
> $$
> \begin{align}
> P(X \leq a)  = F(a)& = 0.78 \\
> \implies a  & = F^{-1}(0.78) \\
>  & = \$19.77
> \end{align}
> $$
>
> ```r
> qnorm(0.78, 17.84, 2.5)
> [1] 19.77048
> ```

> [!question]+ A *random sample* of 40 students is selected from this population. What is the probability that the *mean* hourly wage of this sample is more than $19?
>
> - $\bar{X}$ is a linear combination of normal distributions
>     - $\implies \bar{X}$ is normal
>     - $\bar{X} \sim N\left( 17.84, \frac{2.5^{2}}{40} \right)$
>         - $\mu_{\bar{X}} = \mu_{X} = 17.84$
>         - $\sigma^{2}_{\bar{X}} = \frac{\sigma^{2}_{X}}{n} = \frac{2.5^{2}}{40}$
>
> $$
> \begin{align}
> P(\bar{X} > 19) & = 1 - P(\bar{X} \leq 19) \\
>  & = 0.0017
> \end{align}
> $$
>
> ```r
> 1 - pnorm(19, 17.84, 2.5/sqrt(40))
> [1] 0.001669924
> ```
>
> - Smaller than what we got in first question
> - We expect $\bar{X}$ to be closer to $p$

> [!question]+ Consider again the probability specified in question 3. Suppose the wages were not normally distributed, but had the same mean and variance. Would this considerably change the probability that the mean hourly wage of this sample is more than \$19? Assume a sample size of $n = 40$ is “large enough”.
>
> $$
> \bar{X} \approx N\left( 17.84, \frac{2.5^{2}}{40} \right)
> $$
> by the CLT
>
> - All of the calculations stay the same

> [!question]+ When working with the *sample mean*, what effect — if any — would increasing the sample size, $n$, have on probabilities of the form $P(\bar{X} > a)$?
>
> - As $n$ increases:
>     - Standard deviation of sample mean $\frac{\sigma}{\sqrt{ n }}$ decreases
>     - → Distribution of sample mean becomes more concentrated around population mean $\mu$
> - If $a > \mu$, the probability $P(\bar{X} > a)$ will decrease
>     - Distribution becomes narrower and more concentrated around $\mu$
>     - → Less likely for $\bar{X}$ to exceed $a$
> - If $a < \mu$, the probability $P(\bar{X} > a)$ will increase
>     - Narrowing of distribution around $\mu$ makes it more likely for $\bar{X}$ to be greater than $a$
