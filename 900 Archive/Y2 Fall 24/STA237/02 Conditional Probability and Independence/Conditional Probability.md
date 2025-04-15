---
dg-publish: false
tags: ["#lecture", "#note", stats, university]
Course Code:
  - "[[STA237]]"
Week: 3
Module:
  - 2 - Conditional Probability & Independence
Date: 2024-09-17
Date created: Thu., Sep. 19, 2024, 10:50:10 am
Date modified: Sun., Nov. 17, 2024, 7:16:34 pm
---

# Contingency Tables

Consider a survey that asks:

- Do you drink coffee? Yes/No
- Do you drink tea? Yes/No

<!-- break -->
- Relationship between two of the outcomes of two experiments with a finite sample sample can be shown using a **contingency table**

![|500](https://i.imgur.com/crFyylk.jpeg)

- **Joint distribution**
    - Where we consider both coffee and tea variables together (i.e., jointly)
- **Marginal distribution**
    - Consider only one variable
        - As if other variable was not even there
    - Working only with outside/margins of table

![|400](https://i.imgur.com/87231fo.jpeg)

# Joint and Marginal Distributions

- What proportion of people drink tea *and* don’t drink coffee?
    - *and* ⇒ joint
    - $P(T \cap C^{C})= \frac{40}{158}$
- What proportion of people drink tea?
    - Coffee is not mentioned ⇒ marginal
    - $P(T) = \frac{85}{158}$

# Conditional Distributions

> [!def] **Conditional distribution**
>
> - Condition on one value of one variable and consider what happens with other variable for only these people
> - (e.g., non-coffee drinkers)

- Out of non-coffee drinkers, how many drink tea?
    - ![|400](https://i.imgur.com/7FiJha8.jpeg)
    - $P(T|C^{C})= \frac{40}{84}$
    - “$T|C^{C}$” Tea drinkers *given* non-coffee drinkers

<!-- break -->
- When we find a *conditional probability*, we are actually dealing with two events:
    - $A$ = Event that we are interested in (e.g., Tea drinkers)
    - $B$ = Event that we already know has happened and want to condition on (e.g., non-coffee drinkers)
- Since we already know that event B happened, we can ignore anything that is not B
- Venn diagram: If we know we are in the circle that represents B, what is the probability of being in A?
- We do this by **conditioning on B**
    - ==Remove== anything in our collection of outcomes of our experiment that is not included in B
    - ==Think of B as our new sample space==

# Notation – Conditional Probability

The probability of A given B:
$$P(A \mid B) = \frac{P(A \cap B)}{{P(B)}}$$where $P(B) > 0$.

- Conditioning on B → Only considering how many times A happens ==out of the number of times B happens==
    - i.e., Out of the outcomes in B, how many of them are also outcomes in A?
- Look at all the outcomes that are in *both A and B* → Find their probability (out of everything)
- Look at outcomes in B → Find that probability out of everything

# Venn Diagram of Conditional Probability

- Conditional probability: We have new information that changes the sample space
    - $$\Omega' \to B$$

![|600](https://i.imgur.com/0kS2LPV.png)

# Example: Coin Flipping

Let’s flip a coin 3 times. Then, we have 8 possible outcomes:
$$\Omega = \{HHH, HHT, HTH, HTT, THH, THT, TTH, TTT\}$$

- Suppose we are interested in the event $A = \text{Two or more heads}$. What is $P(A)$?
    - $$P(A) = \frac{4}{8} = 0.5$$
- Let event $B = \text{Heads on first toss}$.
- Probability of at least two heads, $P(A)$, will now *change* if we include the fact that we ==already know== we got a head on the first toss
    - If we condition on $B$, then we remove all outcomes that don’t have a head on the first toss
        - This lets us calculate $P(A \mid B)$:
            - $$\Omega' = \{\bbox[pink]{HHH, HHT, HTH}, HTT, \cancel{THH, THT, TTH, TTT}\}$$
            - $P(A \mid B) = \frac{3}{4}$

**Using formula.**
$$
\begin{align*}
P(A \cap B) &= \frac{3}{8} \\
P(B) &= \frac{4}{8} \\\\
P(A \mid B) &= \frac{P(A \cap B)}{P(B)} = \frac{\frac{3}{8}}{\frac{4}{8}} = \frac{3}{4}
\end{align*}
$$
**Using contigency table.**
![](https://i.imgur.com/tidnwn2.jpeg)

# ==Conditional Probability is a Probability function==

- Recall:
    - $0 \leq P(\omega) \leq 1$ for all $\omega \in \Omega$
    - $1 = P(\Omega) = \sum\limits_{\omega \in \Omega} P(\omega)$
    - For all events $A \subseteq \Omega, \sum\limits_{\omega \in A} P(\omega) = P(A)$
    - Note that $P(\omega) = P(\omega \mid \Omega)$
- Similarly:
    - $0 \leq P(b \mid B) \leq 1$ for all $b \in B$
    - $1 = P(B \mid B) = \sum\limits_{b \in B} P(b \mid B)$
    - For all events $A \subseteq B, \sum\limits_{b \in A} P(b \mid B) = P(A \mid B)$
- *Rescaling* sample space from $\Omega \to B$

# General Formula for Finding $P(A \cap B)$

Also known as **General Multiplication Rule**

$$P(A \cap B) = P(A \mid B) P(B)$$

- Can be extended to more events, e.g.,
    - $P(A_{1} \cap A_{2} \cap A_{3}) = P(A_{3} \mid A_{1} \cap A_{2}) P(A_{2} \mid A_{1}) P(A_{1})$
    - $P(A_{1} \cap \dots \cap A_{k}) = P(A_{k} \mid A_{1} \cap \dots \cap A_{k-1}) P(A_{k-1} \mid A_{1} \cap \dots \cap A_{k-2}) \dots P(A_{2} \mid A_{1}) P(A_{1})$

# Law of Total Probability

Recall **Partition Rule**:

$$\begin{align*}
P(A) &= P(A \cap B) + P(A \cap B^{C}) \\
P(A) &= P(A \cap B_{1}) + P(A \cap B_{2}) + \cdots + P(A \cap B_{k})
\end{align*}$$
where $\forall n \leq k, B_{n}$ makes up the entire sample space and are mutually exclusive.

- Combine the partition rule and general multiplication rule: $$\begin{align*} P(A) &= P(A \mid B)P(B) + P(A \mid B^{C}) P(B^{C}) \\ P(A) &= P(A \mid B_{1}) P(B_{1}) + \cdots + P(A \mid B_{k}) P(B_{k}) \end{align*}$$where $\forall n \leq k, B_{n}$ ==make up the entire sample space== and are ==mutually exclusive==.

### Ex. Smoking
# example

We know 60% of the population smokes, and 50% of the population is male. Given that someone is male, there is a 40% chance that they are a smoker. What is the probability of a female being a smoker?
$$P(S \mid F)$$

**Step 1.** Set up notation for events and list all the probabilities we know.
$$\begin{align*}
P(F) &= 0.5 \\
P(F^{C}) &= 0.5 \\
P(S \mid F^{C}) &= 0.4 \\
P(S) &= 0.6
\end{align*}$$
**Step 2.** Use law of total probability
$$\begin{align*}
P(S) &= P(S \cap F) + P(S \cap F^{C}) \\
&= P(S \mid F)P(F) + P(S \mid F^{C})P(F^{C})
\end{align*}$$
$$\begin{align*}
0.6 &= P(S \mid F)(0.5) + (0.4)(0.5)\\
\implies P(S \mid F) &= 0.8
\end{align*}$$
![](https://i.imgur.com/MWdAywT.png)
