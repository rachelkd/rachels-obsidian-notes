---
dg-publish: true
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 3
Module:
  - 2 - Conditional Probability & Independence
Date: 2024-09-17
Date created: Thu., Sep. 19, 2024, 10:51:22 am
Date modified: Thu., Dec. 5, 2024, 5:48:37 pm
---

# Independence of Events

- General idea of independence between two events:
    - When knowing one event already happened, it does not influence whether or not the other event happens
    - e.g., Suppose experience: roll a dice and flip a coin
        - $A =$ Roll a 3
        - $B =$ Land on heads
        - Does knowing that I flipped a head with my coin give me any info (that I did not already have) about my chance that I will roll a 3?
            - i.e., ==$A$ and $B$ are independent==?
            - No ⇒ independent; no new information is added
    - Key to determining independence is this idea of ==providing information that did not already exist==

## Definition

> [!def]- Two definitions of **independence**
>
> 1. **Conditional probability**
>     - If knowing that $B$ happens does not add information about $A$ happening, then $$P(A \mid B) = P(A)$$
>     - Similarly, if knowing $A$ happens does not give information about chances of $B$ happening, then $$P(B \mid A) = P(B)$$
>     - See [[Conditional Probability]]
> 2. **Multiplication Rule for independent events**
>     - We can say that ==events $A$ and $B$ are *independent*== when $$P(A \cap B) = P(A) \times P(B)$$
>     - See [[Conditional Probability#General formula for finding $P(A cap B)$|General multiplication rule]]

- Two definitions of independence are equivalent
- Recall that a conditional probability can be written as:
    - $$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$
- $A$ and $B$ are independent $\implies P(A \mid B) = \cdots$
    - $$\begin{align*} P(A \mid B) &= \frac{P(A \cap B)}{P(B)} \\ &= \frac{P(A) \times P(B)}{P(B)} && \text{(by multiplication rule)} \\ &= P(A) \end{align*}$$

## Ex. Roll a Dice and Toss a Coin

# example

![](https://i.imgur.com/1Y2mCkJ.png)

# Mutually Exclusive Events

> [!warning] Mutually exclusive and independent are not the same thing

- Key difference between independent events and disjoint events:
    - What is going on in the ==overlap between the events==
    - i.e., $P(A \cap B)$
- Independent events:
    - Common outcomes in overlap, but can be split nicely into $P(A)P(B)$
- Mutually exclusive events:
    - ==No common outcomes== in overlap
    - $A \cap B = \emptyset$

<!-- break -->
- Can disjoint events ever be independent?
    - Disjoint: $P(A \cap B) = 0$
    - Independent: $P(A \cap B) = P(A)P(B)$
    - $\implies$ Need $P(A)P(B) = 0$
        - Happens when $P(A) = 0$ or $P(B) = 0$

## Ex. Blood Types

Canadian Blood Services says that about 46% of the Canadian population have Type O blood, 42% have Type A blood, 9% have Type B blood, and the rest, which is 3% have Type AB.

#### Assumptions We Make about *Mutually Exclusive Events* and *Independent Events*

> [!important] We always have to make *assumptions* whenever we work with data or probability models.

If you examine one person, are the events that the person is Type A and that the person is Type B mutually exclusive or independent or neither? Explain.

- Check for mutually exclusive:
    - Yes, because a person cannot have two different blood types
    - Mutually exclusive $\iff P(A \cap B) = 0$
- Check for independence:
    - $P(A \cap B) = P(A)P(B) = 0.42 \times 0.09 \neq 0$
    - Not independent

If you examine two people, are the events that the first is Type A and the second is Type B mutually exclusive or independent or neither? Explain.

- Independent
- One person’s blood type does not affect the other’s
- Not mutually exclusive $\iff P(A \cap B) \neq 0$

### a) We Randomly Pick 4 People. What’s the Probability All 4 Individuals Are Type O?

![](https://i.imgur.com/qrUk1rT.png)

### b) We Randomly Pick 4 People. What’s the Probability that no One is Type AB

![](https://i.imgur.com/j3ysOcq.png)

### c) We Randomly Pick 4 People. What’s the Probability at Least 1 Person is Type B

> [!tip]+ “At least” suggests that we should look at **complements**

![](https://i.imgur.com/Bd88KK4.png)

# What if We Have More than 2 Events?

- **Mutual independence**: For a general collections of events, *independence* means that
    - For every finite subcollection $A_{1}, \cdots, A_{k}$, $P(A_{1} \times \cdots \times P(A_{k})) = P(A_{1}) \times \cdots \times P(A_{k})$
    - Stronger condition than pairwise independence
- **Pairwise independence**: A collection of events is *pairwise independent* if $P(A_{i} A_{j}) = P(A_{i})P(A_{j})$ for all pairs of events
- A collection of mutually independent events is *pairwise independent*
    - Events that are pairwise independent are not necessarily *mutually independent*
    - i.e., ==Mutual independence $\implies$ pairwise independence==

## Ex. Two Coin Flips (Textbook 2.21)

# example

Flip two coins. Let A be the event that the first coin comes up heads; B the event that the second comes up heads; and C the event that both coins come up the same, either heads or tails.

- $P(A) = P(B) = P(C) = \frac{1}{2}$
- Are all three events mutually independent?
    - $P(A \cap B \cap C) = P(\text{First coin is heads, second coin is heads, both coins are the same}) = P(HH) = \frac{1}{4}$
    - However, $P(A)P(B)P(C) = \frac{1}{8} \implies$ not mutually independent!
- Pairwise independent?
    - $P(A \cap B) = P(A \cap C) = P(B \cap C) = P(HH) = \frac{1}{4} = P(A)P(B) = P(A)P(C) = P(B)P(C)$
    - i.e., If we know that two events happened, then we know the other event happened

![|300](https://i.imgur.com/rI6g4t3.png)

# Ex. Thrombosis

# example

A genetic test is used to determine if people have a predisposition for thrombosis (i.e. blood clotting that blocks blood flow). It is believed that 3% of people actually have this predisposition. The genetic test has probability 99% of giving positive result when person actually has it. It also has probability 98% of giving a negative result when the person does not have the predisposition. What is the probability that someone who tests positive for the predisposition actually has it?

- Defining events:
    - $T : \text{Having thrombosis}$
    - $G : \text{Genetic test being positive}$
- What are we solving for?
    - $P(T \mid G)$
- Tree diagram:
    - ![|300](https://i.imgur.com/PmgME1R.png)
    - Givens are highlighted in yellow
- Calculate conditional probability:
    - $$\begin{align*}
      P(T \mid G) &= \frac{P(T \cap G)}{P(G)} && \text{(Bayes' Rule)} \\
      &= \frac{\overbrace{P(T)P(G \mid T)}^{\text{Multiplication rule}}}{\underbrace{P(G \mid T)P(T) + P(G \mid T^{C})P(T^{C})}_{\text{Partition rule or law of total probability}}} \\\\
      &= \frac{0.03 \times 0.99}{0.03 \times 0.99 + 0.02 \times 0.97} \\\\
      &= 0.605
      \end{align*}$$
- Interpret $P(T \mid G)$:
    - If you have a positive genetic test, there is a 60.5% chance you have the disease
    - Disease is so rare; good chance you don’t have the disease
