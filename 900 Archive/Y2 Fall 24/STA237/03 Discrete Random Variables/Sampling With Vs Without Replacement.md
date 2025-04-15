---
dg-publish: false
tags: ["#lecture", "#note", university]
Course Code:
  - "[[STA237]]"
Week: 5
Module:
  - "[[3 - Discrete Random Variables]]"
Chapter:
Slides/Notes: https://share.goodnotes.com/s/Hwo2JV8eXyikMOJ7eeI91d
Date: 2024-10-03
Date created: Tue., Oct. 8, 2024, 7:53:28 pm
Date modified: Fri., Dec. 6, 2024, 1:05:41 am
---

# Ex. Capture Recapture

In wildlife ecology studies, a common method is to randomly sample (capture) some animals, mark them in some way, and then release them back in the wild. A secondary sample is later captured, and the number of marked and unmarked animals are recorded. This process can repeat several times, with some animals acquiring multiple marks.

- We are trying to study an extremely endangered species
- There are 10 living members of this species left
- 5 animals (50%) are sampled, marked, then released back into the wild

> [!question]- Is this sampling with or without replacement?
>
> - Sampling *without* replacement

- A secondary sample is performed a month later
- Assume no animals left the study area during this period
- 4 animals were sampled in this secondary sample, some who are marked and some who are not

> [!question]- Is using a [[The Bernoulli and Binomial Distributions|binomial distribution]] with parameters $n = 4, p = 0.5$ to model the number of marked animals appropriate in this situation? Why or why not?
>
> - No; not independent

# Sampling with vs. without Replacement

> [!important]- If we sample *without replacement* in a finite population, we *cannot* use the binomial distribution
>
> - Trials are no longer independent

- e.g., If a bag has 2 red balls and 1 blue ball, and you choose the blue ball, for the next trial there are no more blue balls to pick.
- → Use **[[Hypergeometric Distribution|hypergeometric distribution]]** instead!
    - Hypergeometric distribution formalizes some of the counting techniques seen already

> [!important] When we sample *with* replacement (finite or infinite population), the appropriate model is the **binomial distribution**

- e.g., If a bag has 2 red balls and 1 blue ball, no matter what you pick, the probabilities for all the future trials ==stay the same== because you put the ball you picked back

> [!important] When we are sampling (*with* or *without* replacement) from an *infinite* population, we can use the **binomial distribution**

- Limiting distribution of the hypergeometric → the binomial distribution
- No matter what you pick, there are so many more units in the population that the probabilities for the future trials ==stay the same==
- For large sample sizes (e.g., population of Toronto), we can use the binomial distribution as a good *approximation* for the hypergeometric distribution

# Ex. Capture Recapture, Again

We can also use capture recapture methods for human populations, but instead of physically marking people, we can rely on administrative data from the hospital, Service Ontario, etc.

- 55% of the homeless population in BC have been recorded to use the emergency room in the past year
- A recorded visit to the ER can be thought of as a “mark”
- At a shelter, 30 unrelated individuals staying for the night are asked if they have used the ER within the past year

> [!question]- Is this sampling with or without replacement?
>
> - Sampling *without* replacement
> - Each person is only asked once, and cannot be “replaced” in the sample

> [!question]- Is it appropriate to use the binomial distribution to model the number of people who have been to the ER in the past year?
>
> - Yes, it is appropriate to use the binomial distribution in this case
> - The population (homeless individuals in BC) is large enough to be considered effectively infinite compared to the sample size (30)
> - Each person’s ER visit is independent of others
> - The probability of having visited the ER (55%) remains constant for each person

> [!note] Even though we’re sampling without replacement, the large population size allows us to use the binomial distribution as a good approximation of the hypergeometric distribution in this scenario.

# Ex. Tim Hortons

In 2021, Tim Horton’s ran a ‘Roll up the Rim’ contest where customers could win prizes by rolling up the rim of their coffee or tea cups.

- Every time you bought a coffee or tea, you could roll up the rim to see if you won a prize
- The chances of winning a coffee or food prize was about 1 in 6

> [!question]- Could we use a binomial distribution in this situation? What would we be modelling with it?
>
> - Yes, we could use a binomial distribution to model the number of prizes won
> - We would be modeling the number of successes (prizes won) in a fixed number of independent trials (cup purchases)
> - Each trial (cup purchase) has the same probability of success (winning a prize)
> - The trials are independent of each other

- The population (total number of cups) is very large compared to any individual’s purchases
- Each purchase is independent of others
- The probability of winning (1/6) remains constant for each purchase

<!-- break -->
- On average, how many drinks do you have to buy ==to win== a coffee or food prize?
    - To win $\implies X \sim \text{Geo}\left( \frac{1}{6} \right)$
    - $E(X) = \frac{1}{p} = 6$
- What is the probability of winning your first coffee or food prize on your second try?
    - $P(X = 2) = (1-p)p = \frac{5}{6} \times \frac{1}{6} = \frac{5}{36}$
- What is the probability that you will need to buy more than 3 drinks to win your first prize?
    - $P(\text{First 3 are failures}) = (1-p)^{3} = \left( \frac{5}{6} \right)^{3}$
