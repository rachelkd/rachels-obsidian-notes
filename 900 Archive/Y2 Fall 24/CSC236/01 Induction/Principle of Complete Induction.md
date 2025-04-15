---
dg-publish: true
tags: ["#lecture", "#note", cs, university]
Course Code:
  - "[[CSC236]]"
Week: 2
Module:
  - "[[1 - Induction]]"
Date: 2024-09-16
Date created: Tue., Sep. 17, 2024, 1:25:15 pm
Date modified: Tue., Nov. 19, 2024, 8:30:42 pm
---

> [!note]+ Observation.
> - Have stated many different induction principles
>     - e.g., Simple induction:
>         - $P(0) \; \land \; (\forall n, P(n) \implies P(n + 1)) \implies \forall n, P(n)$
>     - Induction for coin problem ([[Principle of Simple Induction.pdf]]):
>         - $P(12) \; \land \; P(13) \; \land \; P(14) \; \land \; (\forall n \geq 15, P(n-3) \implies P(n))$
>         - or, we could use $\forall n \geq 12, P(n) \implies P(n + 3)$
>     - Practice problems 1:
>         - $\lfloor \sqrt{n} \rfloor \to n$
>         - $((n - 2) \; \land \; (n - 1)) \to n$
> - With recursion, there is really just *one* kind of recursion (in terms of structure)
>     - For each recursive call,
>         - Input is smaller
>         - Input ==satisfies the precondition==
>         - This is the **Principle of Complete Induction**
>     - Therefore, it is a valid recursive structure $\implies$ valid inductive structure indirectly

# Principle of Complete Induction

- Also known as “strong” induction
    - Strong, because you assume a lot

> [!def]- What is the **principle of complete induction**?
> For any predicate $P : \mathbb{N} \to \{T, F\}$, $$\underbrace{\bigg[ \forall n, \bigg(\forall k < n, P(k) \bigg) \implies P(n) \bigg] }_{\text{prove this}}  \implies \underbrace{\forall n, P(n)}_{\text{conclude this}}$$
> - Can’t write $k \leq n$ because $P(n)$ would be one of our assumptions → thus, $P(n)$ true trivially

## Where is the Base Case?

- Typically think of base case as something you prove directly — without relying on induction
- Analogy:

  ```python
    # Pre(n): n in naturals
    def s(n):
        r = n
        for k in range(n):
            # Valid recursive structure:
            # k take values 0 to n - 1, inclusive,
            # so k is natural because n is natural
            # and each call is smaller than n
            r += s(k)  
        return r
    ```

- There has to be a base case if recursive structure is correct
- From analogy, there is a smallest value $n$ such that the for loop doesn’t run
    - i.e., at least one input where no recursive call happens
    - When $n = 0$, `range(n) = range(0)`, which is empty → loop doesn’t run → no recursive call
    - So, $n = 0$ is a base case
- Base case is implicit

> [!important]+ For complete induction, WTP: $(\forall k < 0, P(k) \implies P(0))$
> $$\begin{align*} 
> &\equiv \bigg( \forall k \in \mathbb{N}, 
> \underbrace{k < 0}_{\text{always False}} \underbrace{\implies}_{
> \substack{\text{always} \\ \text{True (vacuously)}}} P(k) \bigg) \implies P(0) \\
> &\equiv True \implies P(0) \\
> &\equiv P(0)
> \end{align*}$$

- So, you just need to prove $P(0)$ in the example, which is the *base case* (implicit)

# Prove that Every Integer ≥ 2 Can Be Written as a Product of Primes (incl. Edge case of a Single prime)

#example

![](https://i.imgur.com/zGcILeE.png)

**Fact.** $$\overbrace{\forall n \geq 2, n \text{ is not prime}}^{\text{Pre(n)}} \implies \underbrace{\exists a,b \in \mathbb{N}, 2 \leq a < n \; \land \; 2 \leq b < n \; \land \;n = ab}_{\substack{\text{Post(n,a,b)} \\ \text{i.e., There exists two factors of } n \text{ that are both greater or equal to 2}}}$$
Conclusion i.e., there exists two factors of $n$ that are both greater or equal to 2.

### Formalize, as Code

```python
# Pre(n): n in natural, and n >= 2, and n is NOT prime
# Return (a, b) s.t. Post(n, a, b): a, b in naturals,
# and 2 <= a < n and 2 <= b < n and n = a * b
def two_factor(n):
    # ...omitted...

# Pre(n): n in naturals and n >= 2
# Return (p1, ..., pm) s.t. Post(n, p1, ..., pm):
#    m >= 1 and p1, ..., pm in naturals, and
#    p1, ..., pm are each prime, and
#    n = p1 * ... * pm
def factor(n):
    if prime(n):  return (n)
    else:  # n is NOT prime
        # Pre of two_factor holds, so:
        (a, b) = two_factor(n)  # Insight: n has factors
        
        # From Post of two_factor:
        # a, b in naturals and 2 <= a < n and 2 <= b < n
        # and n = ab
        # a, b satisfy Pre(n) of factor
        # Smaller problem from Post of two_factor:
        # a < n and b < n
        as = factor(a)
        # as = (p1, ..., pm) and a = p1 * ... * pm
        bs = factor(b)
        # bs = (q1, ..., ql) and b = q1 * ... * ql
        # Now we know:
        #    a = product of all as
        #    b = product of all_bs
        # from Post of factor
        # So, n = ab = p1 * ... * pm * q1 * ... * ql
        return all_as + all_bs
```

### As Proof

#### Define Predicate

$\forall n \in \mathbb{N}$, let $P(n)$ be:
- [English] $n$ has a prime factorization
- [symbolic] $\exists m \geq 1, \exists p_{1}, \cdots, p_{m} \in \text{primes}, n = p_{1} \times \cdots \times p_{m}$

Note: We only need one definition from English or symbolic.

#### WTP

$$\forall n \geq 2, P(n)$$
Start with induction principle **from the code**:

$$\begin{align*}
[\forall n \geq 2, n \text{ is prime} \implies P(n)] \; \land \; \\
[\forall n \geq 2, n \text{ is not prime} \; \land \; \overbrace{(P(2) \; \land \; \cdots \; \land \; P(n-1))} ^ {\text{From Post of \texttt{two\_{factor}}}} \implies P(n)] \\
\implies \forall n \geq 2, P(n)
\end{align*}$$
- Which recursive calls do you make?
    - Calling $P(k)$ on factors of $n$
    - Is there a simple expression for the factors of $n$?
        - Not really
        - Look at algorithm for `factor`
        - Only thing we need to know is that the factors are $< n$, $\geq 2$, and $\in \mathbb{N}$
            - from `Post` of `two_factor`
        - Algorithm may not need to use all those numbers, but is relying on the fact that it can call `two_factor` on $2 \leq a < n$ for all $a \in \mathbb{N}$
- This proof bares a similar structure to complete induction
    - To do what you want to do, you need to rely on potentially all previous values
    - In any particular instance of proof, only two values are needed
        - But cannot say ahead of time which values e.g., $n-2$ or $n-3$
        - No particular pattern

#### Simplified Induction Principle.

$$\bigg[\forall n \geq 2, \underbrace{\Big( \forall k \in \{ 2, \cdots, n-1 \} \Big)}_{\text{expands to }P(2), \cdots , P(n-1)}, P(k) \implies P(n) \bigg] \implies \forall n \geq 2, P(n)$$
- This is an equivalent statement as before
    - If you can prove $P(n)$ when $n$ is prime, then you can prove $P(n)$ when $n$ is prime *and* $P(2), \cdots, P(n-1)$
        - i.e., $[\forall n \geq 2, n \text{ is prime} \implies P(n)] \implies [\forall n \geq 2, n \text{ is prime} \; \land \; P(2) \; \land \; \cdots \; \land \; P(n-1) \implies P(n)]$
    - Extra assumption; allowed to assume more than you strictly need

#### Proof.
Let $n \in \mathbb{N}$ and $n \geq 2$.
Assume [IH] $P(2) \wedge \cdots \wedge P(n-1)$.
$$\begin{align*} &\equiv \forall k \in \{2, \cdots, n-1\}, P(k) \\ &\equiv \forall k \in \mathbb{N}, 2 \leq k < n \implies P(k) \\ &\equiv \bigwedge\limits_{k=2}^{n-1} P(i) \end{align*}$$
Then, either $n$ is prime or not prime.

**Case 1.** If $n$ is prime, then $n = n$ is a prime factorization.

**Case 2.** If $n$ is not prime, then:
<u>External fact:</u>
$$\begin{align*} &\overbrace{n \in N \wedge n \geq 2 \wedge n \text{ is not prime}}^{\text{Pre of \texttt{two\_factor}}} \\ \implies &\exists a, b \in \mathbb{N}, \\ &\underbrace{2 \leq a < n \wedge 2 \leq b < n \wedge n = ab}_\text{Post of \texttt{two\_factor}} \end{align*}
$$
Let $a, b \in \mathbb{N}$ witness the fact.
(A “witness” is typically a specific instance of an existentially quantified formula.)

- By $IH(a)$: $\exists m \geq 1, \exists p_{1}, \cdots, p_{m} \in \text{primes}, a = p_{1} \times \cdots \times p_{m}$
- By $IH(b)$: $\exists l \geq 1, \exists q_{1}, \cdots, q_{l} \in \text{primes}, b = q_{1} \times \cdots \times q_{l}$
- $\therefore n = a \times b = (p_{1} \times \cdots \times p_{m}) \times (q_{1} \times \cdots \times q_{l})$
- So, $p_{1}, \cdots, p_{m}, q_{1}, \cdots, q_{l}$ witness $P(n)$.

$\therefore$ By the principle, $\forall n \geq 2, P(n)$.

#### Notes

- There is no explicit base case.
- Would it be wrong to single out $n = 2$?
    - No.
    - $n = 2$: not singled out as a base case, but still is (because 2 is prime)
    - Would be okay to have an explicit “base case: $n = 2 \dots$”
        - Yes

# Review

**Principle of Complete Induction** (PCI)
$$\underbrace{\forall P : \mathbb{N} \to \{T, F\}}_{\text{implicit}}, \overbrace{\bbox[pink]{\bigg[  \forall n, (\forall k < n, P(k)) \implies P(n) \bigg]}}^{\text{antecendent}} \implies \overbrace{\bbox[lavender]{\forall n, P(n)}}^{\text{conclusion}}$$
- “Principle:” supposed to be *obvious*
Expand the antecedent:
$$\begin{align*}
\bigg( &\Big(\forall k < 0, P(k) \Big) \implies P(0) \bigg) &&\wedge \\
\bigg( &\Big(\forall k < 1, P(k) \Big) \implies P(1) \bigg) &&\wedge \\
\bigg( &\Big(\forall k < 2, P(k) \Big) \implies P(2) \bigg) &&\wedge \\
&\vdots
\end{align*}$$
$$\stackrel{\text{(expanding the IH)}}{\equiv} \bigg( \bbox[pink]{()} \implies P(0) \bigg) \wedge \bigg( P(0) \implies P(1) \bigg) \wedge \bigg( (P(0) \wedge P(1)) \implies P(2) \bigg) \wedge \cdots$$
- What does $()$ mean?
    - $()$ expands to $\forall k < 0, P(k)$
    - $\equiv \forall k \in \mathbb{N}, \bbox[pink]{k<0} \implies P(k)$
    - Antecedent always false
    - Implication is always true i.e., $k < 0 \implies P(k) \equiv True$
- Thus, $\bigg(() \implies P(0)\bigg) \equiv \bigg(True \implies P(0)\bigg) \equiv P(0)$
- i.e., $P(0)$ must be proved directly (implicit base case)

# Twist. Think back to Proof of Prime Factorization

- What we proved:
    - $\forall n \bbox[pink]{\geq 2}, \bbox[lavender]{(\forall k, \bbox[pink]{2 \leq k} < n \implies P(k))} \implies P(n)$
    - Pink: modified complete induction
    - Purple: $IH(n)$

## How Do We Apply “pure PCI” to Conclude $\forall N \geq 2, P(n)$?

- Start with $IH(n)$:
    - $$\begin{align*}
      && \forall k, 2 \leq k < n \implies P(k) \\
      \equiv && \forall k, 2 \leq k \wedge k < n \implies P(k) \\
      \equiv && \forall k, k < n \wedge 2 \leq k \implies P(k) \\
      \equiv && \forall k, k < n \implies \bigg(2 \leq k \implies P(k)\bigg) \\ 
      \equiv && \forall k < n, \bigg(k \geq 2 \implies P(k) \bigg)
      \end{align*}$$
- Now, for the whole WWP:
    - $$\begin{align*}
      && \forall n \geq 2, IH(n) \implies P(n) \\
      \equiv && \forall n, n \geq 2 \implies \bigg(IH(n) \implies P(n) \bigg) \\
      \equiv && \forall n, IH(n) \implies \bigg(n \geq 2 \implies P(n) \bigg)
      \end{align*}$$
Expanding $IH(n)$,
$$\forall n, \bigg( \forall k < n, \Big( \underbrace{k \geq 2 \implies P(k)}_{Q(k)} \Big) \bigg) \implies \bigg(\underbrace{n \geq 2 \implies P(n)}_{Q(n)} \bigg)$$
$\therefore$ By PCI for $Q$,
$$\begin{align*}
  \forall n, Q(n) &\equiv \forall n, n \geq 2 \implies P(n) \\ 
  &\equiv \forall n \geq 2, P(n)
  \end{align*}$$

- Is this the only way to use PCI to prove $\forall n \geq 2, P(n)$?
    - No

> [!important]+ Complete induction is the only induction you need.
> - Allows you to prove anything you want by adding appropriate conditions

- There is going to be at least one case that you need to prove **directly**
    - You do not need to start proof off with base case, though!
