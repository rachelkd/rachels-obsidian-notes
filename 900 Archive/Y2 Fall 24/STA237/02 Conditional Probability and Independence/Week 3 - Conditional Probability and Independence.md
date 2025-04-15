---
dg-publish: true
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 3
Module:
  - 2 - Conditional Probability & Independence
Date: 2024-09-17
Quercus Page: https://q.utoronto.ca/courses/354355/pages/week-3-sept-16-22-conditional-probability-and-independence
Slides/Notes:
  - "[[W3-1.pdf]]"
  - "[[W3-2.pdf]]"
Date created: Tue., Sep. 17, 2024, 4:54:30 pm
Date modified: Wed., Nov. 27, 2024, 11:28:43 am
---

> [!goal]- Learning Objectives
>
> 1. Define conditional probability and describe the difference between P(A|B) and P(B|A).
> 2. Distinguish between mutually exclusive (disjoint) and independent events.
> 3. Assess whether or not events are independent.
> 4. Represent an event as several disjoint subsets to compute its probability using the law of total probability.
> 5. Recognize the difference between conditional and unconditional events and use appropriate rules (e.g., multiplication rule, Bayes’ theorem) to solve problems.

---

# Review and Some Notes

- [[Counting Techniques]]
- ==Draw sample space== when drawing Venn diagrams to represent events

# Conditional Probability

- [[Conditional Probability]]

# Bayes’ Rule and Inverting a Conditional Probability

- [[Bayes’ Rule and Inverting a Conditional Probability]]

# Independence and Dependence

- [[Independence and Dependence]]

# Summary

- Conditional probability formula
    - $$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$
- General multiplication rule
    - $$P(A \cap B) = P(A \mid B)P(B) = P(B \mid A)P(A)$$
- Law of total probability
    - $$P(A) = P(A \mid B)P(B) + P(A \mid B^{C})P(B^{C})$$
- Bayes’ Formula
    - $$P(B \mid A) = \frac{P(A \mid B)P(B)}{P(A \mid B)P(B) + P(A \mid B^{C})P(B^{C})}$$
- Independent events:
    - Events $A, B$ are independent $\iff P(A \mid B) = P(A)$
    - $\iff P(A \cap B) = P(A)P(B)$
- Mutual independence
    - $P(A_{1}, \cdots , A_{k}) = P(A_{1}) \times \cdots \times P(A_{k})$ for every finite subcollection $A_{i}, \cdots, A_{k}$
- Pairwise independence
    - $P(A_{i} \cap A_{j}) = P(A_{i})P(A_{j})$ for all pairs of events
