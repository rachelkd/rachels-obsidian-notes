# Course Reflection 12

## 1. Joint and Marginal PDFs

### Part (b): Marginal PDF of $U = X + Y$

To find the marginal probability density function (PDF) of $U = X + Y$, we integrate the joint PDF $f_{X,Y}(x,y)$ over the line $x + y = u$.

The range of $U$ is from 0 to 1, as $0 \leq x + y \leq 1$.

The marginal PDF of $U$ is given by:

$$
f_U(u) = \int_{0}^{u} f_{X,Y}(x, u-x) \, dx = \int_{0}^{u} 2 \, dx = 2u
$$

However, we must consider the valid range for $u$:

1. For $0 \leq u \leq 1$, the integral becomes:

$$
f_U(u) = 2u
$$

1. For $u > 1$, $f_U(u) = 0$ because $x + y$ cannot exceed 1.

Thus, the marginal PDF of $U$ is:

$$
f_U(u) = \begin{cases}
2u, & \text{if } 0 \leq u \leq 1 \\
0, & \text{if } u > 1
\end{cases}
$$

### Part (c): Marginal PDF of $V = X$

To find the marginal probability density function (PDF) of $V = X$, we integrate the joint PDF $f_{X,Y}(x,y)$ over $y$.

The range of $X$ is from 0 to 1.

The marginal PDF of $V$ is given by:

$$
f_V(x) = \int_{0}^{1-x} f_{X,Y}(x, y) \, dy = \int_{0}^{1-x} 2 \, dy = 2(1-x)
$$

Thus, the marginal PDF of $V$ is:

$$
f_V(x) = \begin{cases}
2(1-x), & \text{if } 0 \leq x \leq 1 \\
0, & \text{if } x > 1
\end{cases}
$$

## Explanation of Simulation Convergence

The phenomenon where the simulated probability of an event becomes closer to the true probability as more trials are conducted is explained by the **Law of Large Numbers**.

### Law of Large Numbers

- **Definition**: The Law of Large Numbers states that as the number of trials increases, the sample average (or relative frequency) of the outcomes will converge to the expected value (or true probability) of the distribution.

- **Application to Coin Flips**: In the case of simulating coin flips, each flip is an independent trial with a true probability of 0.5 for heads (assuming a fair coin). As you increase the number of flips, the relative frequency of heads will converge to 0.5.

### Central Limit Theorem (CLT)

- While not directly responsible for the convergence, the CLT supports the idea that the distribution of the sample mean will be approximately normally distributed around the true mean as the number of samples increases.

### Summary

- The Law of Large Numbers ensures that the more experiments you conduct, the closer the simulated probability will be to the true probability.
- This principle is fundamental in probability and statistics, providing a basis for making predictions based on sample data.

## Properties of the Sampling Distribution of the Sample Mean

1. **The larger the sample is, the smaller the variance of $\bar{X}_n$ is compared to $\text{Var}(X_i) = \sigma_X$.**
   - **True**: The variance of the sample mean $\bar{X}_n$ is $\frac{\sigma_X^2}{n}$, which decreases as the sample size $n$ increases.

2. **$\bar{X}_n$ is always equal to $E(X_i) = \mu$.**
   - **False**: $\bar{X}_n$ is an estimator of $\mu$, but it is not always exactly equal to $\mu$. It converges to $\mu$ as $n$ increases.

3. **The sample size must be large for $\bar{X}_n$ to follow a normal distribution.**
   - **True**: According to the Central Limit Theorem, the distribution of $\bar{X}_n$ approaches a normal distribution as the sample size increases, regardless of the original distribution of $X_i$.

- The larger the sample is, the smaller the variance of $\bar{X}_n$ is compared to $\text{Var}(X_i) = \sigma_X$.
- The sample size must be large for $\bar{X}_n$ to follow a normal distribution.

## Probability Model for Sample Proportion

Assume 25% of students at a university wear contact lenses. Suppose we select 100 students at random from that university and we are interested in the proportion of selected students who wear contacts.

**Which of the following probability models would approximate the distribution of the sample proportion?**

- **Answer:** Binomial distribution $\text{Bin}(n = 100, p = 0.25)$

The binomial distribution is appropriate here because we are dealing with a fixed number of independent trials (100 students), each with two possible outcomes (wearing or not wearing contact lenses), and a constant probability of success (25%).

## Probability of Average Fracture Strength

The fracture strength of tempered glass averages 20 (measured in thousands of pounds per square inch) and has a variance of 25 units².

**What is the probability that the average fracture strength of 64 randomly selected pieces of this glass is between 18.1 and 20.6 units?**

- **Solution:**
  - Given:
    - Mean ($\mu$) = 20
    - Variance ($\sigma^2$) = 25
    - Standard deviation ($\sigma$) = 5
    - Sample size ($n$) = 64
  - The standard deviation of the sample mean ($\sigma_{\bar{X}}$) is $\frac{\sigma}{\sqrt{n}} = \frac{5}{8} = 0.625$.
  - We use the normal distribution to find the probability:
    - $P(18.1 < \bar{X} < 20.6)$
    - Convert to z-scores:
      - $z_1 = \frac{18.1 - 20}{0.625} \approx -3.04$
      - $z_2 = \frac{20.6 - 20}{0.625} \approx 0.96$
  - Using standard normal distribution tables or a calculator:
    - $P(-3.04 < Z < 0.96) \approx P(Z < 0.96) - P(Z < -3.04)$
    - $\approx 0.8315 - 0.0012 = 0.8303$

- **Answer:** The probability is approximately 0.8303.

## Theoretical Value of "result"

Consider the following R code:

```r
n <- 20
reps <- 5000
xbars <- numeric(length=reps)

for(i in 1:reps) {
  xbars[i] <- mean(rnbinom(n, 3, 0.4))
}

result <- mean(xbars) + 3
```

**What theoretical value does "result" estimate?**

- **Answer:** $E(\bar{X}_{20}) + 3$ where $X_i \sim \text{NegBin}(r = 3, p = 0.4)$ for $i = 1, 2, \ldots, 20$

The code simulates the average of 20 negative binomial random variables with parameters $r = 3$ and $p = 0.4$, repeated 5000 times. The `result` adds 3 to the expected value of these sample means.

## Probability of Crate Weighing More than 510 oz

A local farm grows tomatoes and packs them in crates for shipping. Suppose when picked, their tomatoes have a mean weight of 10 oz and a standard deviation of 3 oz.

**What is the probability that 50 tomatoes (assumed to be a simple random sample) in a crate weigh more than 510 oz?**

- **Solution:**
  - Given:
    - Mean weight per tomato ($\mu$) = 10 oz
    - Standard deviation ($\sigma$) = 3 oz
    - Sample size ($n$) = 50
  - Total mean weight of 50 tomatoes = $n \times \mu = 50 \times 10 = 500$ oz
  - Standard deviation of the sample mean ($\sigma_{\bar{X}}$) is $\frac{\sigma}{\sqrt{n}} = \frac{3}{\sqrt{50}} \approx 0.424$ oz
  - We use the normal distribution to find the probability:
    - $P(\bar{X} > 10.2)$ where $\bar{X} = \frac{510}{50} = 10.2$
    - Convert to z-score:
      - $z = \frac{10.2 - 10}{0.424} \approx 0.47$
  - Using standard normal distribution tables or a calculator:
    - $P(Z > 0.47) \approx 1 - 0.6808 = 0.3192$

- **Answer:** The probability is approximately 0.3192.

## Sample Size for Desired Confidence Interval

Based on her experience, a teacher knows that the exam grade of a randomly selected student is a random variable with a mean of 75 and a standard deviation of 8.

**What is the least number of students who have to do the exam to ensure there is a 95% chance that the (sample) average will be between 73 and 77? Use the Central Limit Theorem (CLT).**

- **Solution:**
  - Given:
    - Mean ($\mu$) = 75
    - Standard deviation ($\sigma$) = 8
    - Desired confidence interval: 73 to 77
    - Confidence level: 95%
  - The margin of error is $\pm 2$ (since $77 - 75 = 2$ and $75 - 73 = 2$).
  - For a 95% confidence level, the z-score is approximately 1.96.
  - Using the formula for the margin of error (ME):
    - $\text{ME} = z \times \frac{\sigma}{\sqrt{n}}$
    - $2 = 1.96 \times \frac{8}{\sqrt{n}}$
    - Solving for $n$:
      - $\sqrt{n} = \frac{1.96 \times 8}{2}$
      - $\sqrt{n} = 7.84$
      - $n = 7.84^2 \approx 61.47$
  - Since $n$ must be a whole number, round up to 62.

- **Answer:** At least 62 students are needed.
