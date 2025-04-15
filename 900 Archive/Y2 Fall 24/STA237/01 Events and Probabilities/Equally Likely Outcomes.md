---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 2
Module:
  - 1 - First Principles of Probability
Date: 2024-09-10
Chapter: 1.5
Slides/Notes: 
Date created: Tue., Sep. 10, 2024, 1:58:50 pm
Date modified: Wed., Nov. 27, 2024, 4:29:01 am
---

> [!def]- Probability model for an equally likely outcome
> If $\Omega$ has $k$ elements, then $$P(\omega) = \frac{1}{k}$$for all $\omega \in \Omega$

# Computing Probabilities for Equally Likely Outcomes

Suppose $A$ is an event with $s$ elements, with $s \leq k$.
$\implies P(A)$ is the sum of the probabilities of all the outcomes contained in $A$.

$$P(A) = \sum_{\omega \in A} P(\omega) = \sum_{\omega \in A} \frac{1}{k} = \frac{s}{k} = \frac{\text{\# of outcomes of }A}{\text{\# of outcomes of } \Omega}$$

### Example. Palindrome

Pick a three-letter “word” at random choosing from D, O, or G for each letter. What is the probability that the resulting word is a palindrome? (Words in this context do not need to be real words in English, e.g., OGO is a palindrome.)

- Total number of words:
    - 27 (Three possibilities for each of the three letters)
- Total number of palindromes:
    - 9
    - Do by listing the palindromes (and later use counting techniques)
        - $3C1 + 3C3 + 3C1 = 9$
