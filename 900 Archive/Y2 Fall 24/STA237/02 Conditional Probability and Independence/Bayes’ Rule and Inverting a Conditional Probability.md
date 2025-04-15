---
dg-publish: true
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 3
Module:
  - 2 - Conditional Probability & Independence
Date: 2024-09-17
Date created: Thu., Sep. 19, 2024, 10:48:55 am
Date modified: Thu., Dec. 5, 2024, 5:06:49 pm
---

# Bayes’ Rule

> [!def]+ Bayes Rule
> - Also known as: Bayes’ Formula
> - Lets us use conditional probabilities that we already know ⇒ *reverse* them to find a probability of interest
> $$P(A \mid B) = \frac{P(B \mid A)P(A)}{P(B \mid A) P(A) + P(B \mid A^{C}) P(A^{C})}$$

- Direct extension of [[#Conditional Probability|definition of conditional probability]], which is:
    - $$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$
    - Instead of being given a value for $P(B)$, ned to find it from other probabilities
- In [[#Ex. Smoking]], we were interested in the conditional probability of being a smoker given gender
- In Bayes’, interested in probability of being a male, given that you are a smoker
    - e.g., What is the probability that a random smoker is male?

Recall:
1. Law of total probability: $P(B) \stackrel{\text{(partition rule)}}{=} P(B \cap A) + P(B \cap A^{C}) \stackrel{\text{(multiplication rule)}}{=} \bbox[lavender]{P(B \mid A) P(A) + P(B \mid A^{C}) P(A^{C})}$
2. General multiplication rule: $P(A \cap B) = \bbox[lavender]{P(B \mid A)P(A)}$

# Tree Diagrams

- **Tree diagrams**
    - Tool you can use when you deal with conditional probabilities
        - Multiply as you go left to right (i.e., as you go across branch)
        - Add as you go up and down (within same column)
    - ![|400](https://i.imgur.com/XXAVshH.png)
- Lets us see how many combinations of events there are
    - Recall [[Counting Techniques|Multiplication Principal]]

## Ex. Gender of Children

#example

- There is 50% chance of having a female firstborn child
    - $\iff P(F_{1}) = 0.5$
- *If* we have a female firstborn, then the probability the second child is female is $\frac{1}{3}$
    - $\iff P(F_{2} \mid F_{1}) = \frac{1}{3}$
- *If* we have a male firstborn, then the probability the second child is female is $\frac{2}{5}$
    - $\iff P(F_{2} \mid F_{1}^{C}) = \frac{2}{5}$
- Set up notation and list all the probabilities we have
    - $P(F_{1}^{C}) = 0.5$
    - $P(F_{2}) = ?$

> [!important] Whenever you see the word “if” or “given,” you should ==consider conditional probabilities==

### Ex. Tree Diagram

![](https://i.imgur.com/zJ76D2k.png)
*Highlighted colours add up to 1 (by properties of conditional probability).*

### What is the Probability of a Female First Child given that We Have a Female Second Child?

- What are we solving for?
    - $P(F_{1} \mid F_{2})$
    - $$=\frac{P(F_{1} \cap F_{2})}{P(F_{2})} \stackrel{\text{(from tree diagram)}}{=} \frac{\frac{1}{6}}{P(F_{2})}$$
- Solving for $P(F_{2})$
    - Can use partition rule: $P(F_{2}) = P(F_{1} \cap F_{2}) + P(F_{1}^{C} \cap F_{2})$
    - Law of total probability: $$P(F_{2}) = P(F_{2} \mid F_{1})P(F_{1}) + P(F_{2} \mid F_{1}^{C}) P(F_{1}^{C})$$
- Information we have:
    - $P(F_{2} \mid F_{1}) = \frac{1}{3}$
    - $P(F_{2} \mid F_{1}^{C}) = \frac{2}{5}$
    - $P(F_{1}) = \frac{1}{2}$
    - $P(F_{1}^{C}) = 0.5$

$$\begin{align*}
P(F_{1} \mid F_{2}) \\
&= \frac{P(F_{1} \cap F_{2})}{P(F_{2})} \\
&= \frac{\frac{1}{6}}{P(F_{2} \mid F_{1})P(F_{1}) + P(F_{2} \mid F_{1}^{C}) P(F_{1}^{C})} \\
&= \frac{\frac{1}{6}}{\frac{1}{3} \cdot \frac{1}{2} + \frac{2}{5} \cdot \frac{1}{2}} \\
&= \frac{11}{30} \\
&= \frac{5}{11}
\end{align*}$$
