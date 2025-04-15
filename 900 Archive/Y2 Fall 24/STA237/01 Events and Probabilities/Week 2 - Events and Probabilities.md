---
dg-publish: false
tags: ["#lecture", "#note", university]
Course Code:
  - "[[STA237]]"
Week: 2
Module:
  - 1 - First Principles of Probability
Date: 2024-09-11
Quercus Page: https://q.utoronto.ca/courses/354355/pages/week-2-sept-9-15-first-principles-of-probability
Slides/Notes:
  - "[[W2-1.pdf]]"
  - https://share.goodnotes.com/s/irjWsKKuWY5stLppgyFEWl
Date created: Tue., Sep. 10, 2024, 3:59:57 pm
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

> [!goal]- Learning Objectives
>
> 1. Use set notation and Venn diagrams to describe events in a given sample space
> 2. Explain probability models and basic probability and set theory vocabulary
> 3. Solve problems applying event operations, properties of probability, and in the case of equally-likely outcomes, counting methods
> 4. Use R to simulate basic experiments and estimate the probability of an outcome
>     - e.g., coin flipping, rolling rice

> [!info]- Week Info
>
> ##### Slides
>
> `$= dv.current().slides ? dv.current().slides.forEach(slide => dv.paragraph("- " + slide)) : "No link to slides"` > `$= dv.current()['Quercus Page'] ? "- [Quercus Page](" + dv.current()['Quercus Page'] + ")" : "No link to Quercus page"`

---

# Review

From [[Week 1 - Course Introduction, Outcomes, Events, and Probabilities]]:

- Suppose you flip a coin 3 times. What is the sample space?
    - $\Omega = \{ HHH, HHT, HTH, HTT, THH, TTH, TTT \}$
    - 8 outcomes
- You are interested in the event that you get 3 heads. How would you write this? How many outcomes does this event contain?
    - $A = \{ \text{get the heads} \} = \{HHH\}$
- You are interested in the event you get at most 1 heads. How would you write this? How many outcomes does this event contain?
    - $B = \{ \text{get at most 1 heads}\} = \{ HTT, THT, TTH, TTT\}$
    - 4 outcomes

# Lecture Notes

- [[Probability Functions and Properties of Probabilities]]
- [[Equally Likely Outcomes]]
- [[Counting Techniques]]
    - includes: Sampling with and without replacement
- [[Problem Solving Strategies]]
    - incl. De Morgan’s Law, Partition Rule, Inclusion-Exclusion
