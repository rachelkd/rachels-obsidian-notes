---
dg-publish: false
tags: [lecture, note, stats, university]
Course Code:
  - "[[STA237]]"
Week: 2
Module:
  - 1 - First Principles of Probability
Lecture:
  - "2"
Chapter: 1.8
Slides/Notes: 
Date: 2024-09-10
Date created: Tue., Oct. 15, 2024, 4:08:22 am
Date modified: Wed., Nov. 27, 2024, 4:35:48 am
---

# De Morgan’s Laws

> [!thm]+ De Morgan’s Laws
> - & $\overline{A \cup B} = \overline{A} \cap \overline{B}$
>     - Neither A nor B occurs
> - & $\overline{A \cap B} = \overline{A} \cup \overline{B}$
>     - At most one of the two events occur
>     - Not both
> - Can be extended to infinite sequences:
>      - $$\left(\bigcup_{i = 1}^{\infty} A_{i}\right)^{C} = \bigcap_{i = 1}^{\infty} A_{i}^{C}$$
>      - $$\left( \bigcap_{i=1}^{\infty} A_{i} \right) ^{C} = \bigcup _{i=1}^{\infty} A_{i}^{C}$$

- When moving the complement inside, flip the sign
- Visualize using Venn diagrams
    - ![](https://i.imgur.com/N6Mk87Z.png)
    - ![](https://i.imgur.com/IdPGPlP.png)

# Partition Rule

- Used to solve for the probability of one group at a time

> [!thm]+ Partition Rule
> $$P(A) = P(A \cap B) + P(A \cap B^{C})$$

- e.g., Let $A = \{ \text{is a smoker} \}, B = \{ \text{male} \}$

> [!tip]+ Partition Rule can be expanded to ==multiple partitions==.
> - e.g., Let $A = \{ \text{is a smoker} \}, B_{1} = \{ \text{Age under 13} \}, B_{2} = \{  \text{Age between 14-19} \}, \dots$
>     - $P(A) = P(A \cap B_{1}) + P(A \cap B_{2}) + \dots$, where the ==$B$’s make up the entire sample space and are mutually exclusive==

![](https://i.imgur.com/15CEIEi.png)

*Visual of the Partition Rule (multiple partitions) where $B_{i}$ makes up the whole sample space.*

### Example. Partition Rule & Smokers

We know that 60% of the population smokes, and 50% of the population is male. We also know that 20% of the population are male smokers.

- ? What percentage of the population are female smokers?
- Set up notation for events and list all the probabilities we know
    - $A = \{ \text{smokers} \}, B = \{ male \}$
    - $P(A \cap B) = 0.2$
    - $P(A) = 0.6$
    - $P(B) = 0.5$
- Use Partition Rule
    - $P(A) = P(A \cap B) + \overbrace{P(A \cap B^{C})}^{\text{female smokers}}$
    - $P(A \cap B^{C}) = P(A) - P(A \cap B) = 0.6 - 0.2 = 0.4$
    - 40%

# Inclusion-Exclusion

- An addition to the addition rule

> [!thm]+ Inclusion-Exclusion
> For an arbitrary number of events $\{ A_{1}, A_{2}, \dots, A_{n} \}$,
> $$P( A_{1} \cup \dots \cup A_{n}) = \sum_{i} P(A_{i}) - \sum_{i < j} P(A_{i} \cap A_{j}) + \sum_{i<j<k} P(A_{i} \cap A_{j} \cap A_{k}) - \dots + (-1)^{n+1}P(A_{1} \cap \dots \cap A_{n})$$

- Draw out a Venn diagram for cases where there are not too many events
    - ![](https://i.imgur.com/mYxxV3t.png)
