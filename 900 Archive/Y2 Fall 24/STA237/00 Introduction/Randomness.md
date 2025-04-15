---
dg-publish: true
tags:
  - "#lecture"
  - "#note"
  - "#stats"
  - university
Course Code:
  - "[[STA237]]"
Week: 1
Module:
  - "[[900 Archive/Y2 Fall 24/STA237/00 Introduction/0 - Introduction|0 - Introduction]]"
Date: 2024-09-05
Date created: Thu., Sep. 5, 2024, 10:31:11 am
Date modified: Sun., Nov. 24, 2024, 6:25:42 pm
---

> [!important] Randomness
> - Randomness is why we need to talk about [[Probability|probability]]
> - Never know definitively what will happen in random experiment
> - Individual outcomes are *uncertain*
>     - But there is *structure* to how often outcomes occur in very large numbers of repetitions

#### Examples of Random Experiments

1. Result of rolling a 6-sided die
2. How many people will benefit from a new drug
3. Time that a bus arrives at the bus stop
4. Height of the next person who walks into the room
5. Sex of the next baby born in a hospital
6. Maternal mortality rate in Canada next year

# Events and Outcomes

- When we run a **random experiment** → may be interested in individual **outcomes** of the experiment
- Outcome:
    - Result of a random experiment
- Use $\omega$ to denote a single **outcome**
    - i.e., “omega”
- Often, we are interested in some ==*subset* of the outcomes in an experiment== → an **Event**
    - i.e., a ==collection of outcomes== $\implies$ outcome is an element of a sample space
- Use capital letters to refer to an **event** we are interested in
    - e.g., $A = \{H\}$, where $H$ is an outcome
    - Notice that $A \subseteq \Omega$ (see [[#Sample Space]])

# Sample Space

- A ==set of *possible* outcomes== of a random experiment
    - Notation is $\Omega$
- Subsets of the **sample space** are **events**
- e.g., Rolling a die: $\Omega = \{1,2,3,4,5,6\}$

### e.g., Flipping Two Coins (random experiment)

- Sample space: $\Omega = \{ HH, HT, TH, TT \}$
- Outcome: $\omega = HH$
- Event: $A = \{ \text{Getting at least one head in two coin flips} \} =  \{ TH, HH, HT \}$
    - Further examples,
      $B = \{\text{We get two heads}\} = \{HH\}$
      $C = \{ \text{We get one head, one tail} \} = \{HT, TH\}$

> [!question] What if we roll a die and then flip a coin? What is our sample space?
> - $\Omega = \{ (1H), (1T), (2H), (2T), …, (6H), (6T) \}$
> - 12 outcomes

# How Do We Tell how Big Our Sample Space Is?

- Use multiplication principal (Ch. 1.6)

# Sample Space and Event Can Have *infinite outcomes*

e.g., Flip a coin until you get a heads. You are interested in the event you need at least 3 flips (Textbook Ex. 1.4)

- What is the **sample space**?
    - Set of all sequences of coin flips with one head preceded by some number of tails
    - $\Omega = \{H, TH, TTH, TTTH, … \}$
- What is the **event**?
    - $A = \{TTH, TTTH, TTTTH, …\}$
- Notice that the *sample space* and *event $A$* are infinite → contain an infinite number of outcomes

# e.g., Class President (Textbook Ex. 1.3)

Yolanda and Zach are running for president of the student association. One thousand students will be voting, and each voter will pick one of the two candidates. We will eventually ask questions like, What is the probability that Yolanda wins the election over Zach by at least 100 votes? But before actually finding this probability, first identify (i) the sample space and (ii) the event that Yolanda beats Zach by at least 100 votes.

- Denote the outcome of the vote:
    - $(x, 1000-x)$, where $x$ is the number of votes for Yolanda, and $1000-x$ is the number of votes for Zach
- Sample space:
    - $\Omega = \{(0, 1000), (1,999), (2,998), …, (999,1), (1000,0)\}$
- Event:
    - Let $A$ be the event that Yolanda beats Zach by at least 100 votes
    - $A$ consists of all outcomes in which $x-(1000-x) \geq 100$ or $550 \leq x \leq 1000$
    - $A = \{(550, 450), (551, 449), …, (999, 1), (1000, 0)\}$
