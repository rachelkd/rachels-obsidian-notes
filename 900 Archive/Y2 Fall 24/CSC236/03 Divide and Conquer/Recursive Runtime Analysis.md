---
dg-publish: true
tags: [lecture, note, university]
Course Code:
  - "[[CSC236]]"
Week: 8
Module:
  - "[[3 - Divide-and-Conquer Algorithms]]"
Lecture:
  - "15"
  - "16"
  - "17"
  - "18"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/kdza4J6PUXrLzhbGXzqr4p
  - https://share.goodnotes.com/s/ZYm424vKu7tRGG6QJoEBs1
Date: 2024-11-04
Date created: Thu., Nov. 7, 2024, 12:48:56 pm
Date modified: Tue., Nov. 19, 2024, 4:06:58 pm
---

# Review

- We have an *algorithm $A$*
    - with inputs $x \in \mathcal{I}, \mathcal{I} = \{ \text{all inputs for } A \}$
- **Runtime function**
    - $RT_{A}(x) =$ number of “steps” to run $A$
    - or $RT(x)$ if there is only one algorithm

![](https://i.imgur.com/zQSjm3E.png)

> [!warning]+ This is not a function!
> For every “size” of input, there are *many* times for different inputs of the ==same size==

- Turn into function of *input size*:
    - **Worst case**: $WC(n) = \text{max}\{ RT(x) : x \in \mathcal{I} \wedge \text{size}(x) = n \}$
    - **Best case**: $BC(n) = \text{min}\{ RT(x) : x \in \mathcal{I} \wedge \text{size}(x) = n \}$
    - **Average case**: $AC(n) = \frac{\sum RT(x) \text{ over the set}}{\text{\# inputs of size }n}$
        - Often, average case is calculated using [[Introduction to Continuous Random Variables|probability distribution functions]] and its expectation

> [!tip] In practice, finding the *exact* expression for $WC(n)$ is difficult and not useful!

- **Asymptotic Notation**:
    - $\mathcal{O} :$ upper bound $\neq$ worst case
    - $\Omega :$ lower bound $\neq$ best case
    - $\Theta :$ tight bounds $\neq$ average case

> [!info]+ $\mathcal{O}, \Omega, \Theta$ are **properties** of a function
> - Worst/best/average case *specifies* a function
> - Can prove an upper bound and lower bound on BOTH worst case and best case

---

# Recursive Runtime

```python title:"Factorial and Mergesort Algorithms"
# Pre(n): n ∈ ℕ
def F(n):
    if n == 0: return 1
    return n * F(n - 1)

# Pre(A): A is a list of comparables
def MS(A):
    if len(A) > 1:
        F = A[0 : len(A) // 2]
        S = [len(A) // 2 : len(A)]
        MS(F)
        MS(S)
        MERGE(F, S, A)
        # Assumption: Merge takes WC-runtime θ(len(A))
```

> [!question] What is the runtime of these algorithms?

## Intuition

> [!obs]+ Recursion Trees
> - Trace execution on small inputs

- There is ==work being done at each individual level==
    - Work being done when making recursive call
        - Calculate $n-1$
    - Work being done on the way back
        - Need to take result of recursive call $\times n$ (local variable)
    - When you call $F(3)$, need to check if $3 = 0$

![](https://i.imgur.com/cBEuydJ.png)

- Work is set up so that at each level, it is carried out in the same way
    - $\implies$ Pattern
- In this recursion tree,
    - Only one place in the code where you do the work of the base case ($F(0)$)
    - Everyone else: Do the work at each call of the function that is going to make a recursive call

> [!tip]+ How to get the total amount of work
> - Figure out how much work is being done at each recursive call
> - Add up for each call *plus* the work of the base case

- How much work do you do if $n$ is a base case?
    - Constant
- How much work gets done at *one* recursive call?
    - Compare `n == 0` (not 0, since recursive call)
    - Calculate `n - 1`, make recursive call
    - When it comes back, do `n * F(n-1)` and return
    - Constant amount of work outside of the recursive call
- How big is the recursion tree?
    - Linear
    - One node for each $n$ all the way down to zero
    - Expect runtime to be some linear function
    - & This gives us the intuition!

> [!important] Recall
> - Worst case runtime is a function that takes *size* as input
> - Mergesort takes a list

### Mergesort

1. <mark style="background: #D2B3FFA6;">Define input size</mark>
    - $n = len(A)$
    - $MS(n = len(A))$

When you call mergesort on an input of size 8:

![](https://i.imgur.com/POgstZB.png)

- Once you call mergesort on some size sublist of 1
    - Mergesort goes: size of list is not greater than 1 $\implies$ Return

![](https://i.imgur.com/J1TLX7f.png)

- Note that this is not the order that the recursive calls make, but
- Order doesn’t matter if all we care about is total amount of work done

> [!summary]+
> - There is one leaf in recursion tree for each base case that recursion is eventually going to encounter
> - If we have 8 elements,
>     - Each little sublist with 1 of the elements in sublist is going to be one of the base cases
> - One internal node for each place that you make a recursive call

> [!question] How much work gets done at each internal node?

# Formalization

- Work on the algorithm itself
- Annotate how much work gets done for different parts of the algorithm

## Factorial

```python
# Pre(n): n ∈ ℕ
def F(n):
    if n == 0: return 1
    return n * F(n - 1)
```

- Total about of work in base case (when $n = 0$)
    - $\Theta (1)$ steps
- **Steps** is a unit
    - Important to keep track of what is a step and what is another quantity
    - Various different quantities keeping track of when tracing
        - e.g., steps and input size
        - Both are natural numbers, but represent different things → Important to keep them clear!
- Total amount of work when not in base case
    - $\Theta(1) + \text{recursive call}$

![](https://i.imgur.com/stdtiRC.png)

## Mergesort

```python
# Pre(A): A is a list of comparables
def MS(A):
    if len(A) > 1:
        F = A[0 : len(A) // 2]
        S = [len(A) // 2 : len(A)]
        MS(F)
        MS(S)
        MERGE(F, S, A)
        # Assumption: Merge takes WC-runtime θ(len(A))
```

![](https://i.imgur.com/RfQUBz2.png)

- Leaf takes $\Theta(1)$ steps
- Internal node takes $\Theta(1) + \Theta(n) + \Theta(n) + \Theta(n) + \text{recursive calls}$
    - Some linear function

## Insight

> [!obs]+ Insight
> - Define $T(n) =$ worst-case runtime

For $F$:
- $T_{F}(0) = \underbrace{\Theta(1)}_{\text{base case}}$
- For $n > 0$:
    - $T_{F}(n) = \underbrace{\Theta(1)}_{\text{non-recursive work}} + \underbrace{T_{F}(n - 1)}_{\text{recursive call}}$

> [!tip] By giving a name for the worst-case runtime, we can use that function in our definition

> [!summary]+
> 1. Define $n$ (input size)
> 2. Calculate # steps *outside* recursive calls in $\Theta$ terms
> 3. Calculate *exact* steps input size for each recursive call
> 4. Create **recurrence relation**

## Recurrence Relation for Factorial

$$T_{F}(n) = \begin{cases}
\colorbox{DarkSeaGreen}{1} && n = 0  \\
T_{F}(n - 1) + \colorbox{DarkSeaGreen}{1} && n > 0
\end{cases}$$
- Note that the highlighted terms are in $\Theta$-terms
    - We really mean $\Theta (1)$
    - $\Theta$ is implicit

## Recurrence Relation for Mergesort

For input size $n = len(A)$:
- $T_{MS}(0) = 1$
    - Base case when $len(A) = 0$
- $T_{MS}(1) = 1$
    - Base case when $len(A) = 1$
- For each $n > 1$,
    - $T_{MS}(n) = \colorbox{DarkSeaGreen}{n} + T_{MS}\left( \left\lceil  \frac{n}{2}  \right\rceil \right) + T_{MS}\left(\left\lfloor  \frac{n}{2}  \right\rfloor\right)$
    - $\Theta(n)$ term includes:
        - Cost of slicing the array
        - Cost of merging the sorted halves
        - Other constant work at each recursive level
    - Ceiling and floor functions handle odd-length lists:
        - When $n$ is odd, splits into roughly two halves
        - One half rounds up, other rounds down


$$T_{MS}(n) = \begin{cases}
1 && n = 0,1 \\
\underbrace{T_{MS}(\lceil\frac{n}{2}\rceil)}_{\text{first half}} + \underbrace{T_{MS}(\lfloor\frac{n}{2}\rfloor)}_{\text{second half}} + n && n > 1
\end{cases}$$

## Closed-Form

> [!question]+ How do we get a **closed-form** definition?
> - Algebraic expression for $T(n)$
> - No recursion
> - No sum
>     - Sum = loop

> [!def]+ Unrolling
> Process of expanding a recursive definition by repeatedly substituting the definition into itself to see the pattern of computation

### Unrolling “Bottom-Up” Approach

> [!example]+ Factorial Example
> Looking at values of $T_F(n)$ for small $n$:
> 
> | $n$ | 0 | 1 | 2 | 3 | ... |
> |-----|---|---|---|---|-----|
> |$T_F$| 1 | 2 | 3 | 4 | ... |
> 
> From this pattern, we might guess: $T_F(n) = n + 1$
> 
> > [!warning] This is just a “guess” based on pattern matching!
> > Need formal proof to verify correctness

> [!example]+ Mergesort Example
> Looking at values of $T_{MS}(n)$ for small $n$:
> 
> | $n$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | ... |
> |-----|---|---|---|---|---|---|---|-----|
> |$T_{MS}$| 1 | 1 | 4 | 8 | 12 | 17 | 22 | ... |
> 
> Finding $T_{MS}(n)$ is more complex — pattern not immediately obvious

### <mark style="background: #D2B3FFA6;">Unrolling “Top-Down”</mark> Approach

- Also known as “repeated substitution”

> [!tip]+ Strategy Choices
> When unrolling recurrences:
> 1. Leave as sum (keep expanding)
> 2. Simplify as you go
> 
> Choice depends on:
> - Complexity of recurrence
> - Pattern visibility
> - Final form needed

> [!example]+ Factorial Example
> Assume $n$ is very large:
> 
> $$\begin{align*}
> T_F(n) &= T_F(n-1) + 1 \\
> &= (T_F(n-2) + 1) + 1 \\
> &= (T_F(n-3) + 1 + 1) + 1 \\
> &\vdots
> \end{align*}$$
> 
> After $k$ steps:
> $$T_F(n) = T_F(n-k) + \underbrace{1 + 1 + ... + 1}_{k \text{ times}}$$
> 
> > [!obs]+ Closed Form Solution
> > - Reaches base case when $n-k = 0$ (i.e., $k = n$)
> > - Therefore: $T_F(n) = T_F(0) + n = 1 + n$
> > - Confirms our bottom-up pattern!

> [!example]+ Mergesort Example
> Assume $n$ is very large:
> $$\begin{align*}
> T_{MS}(n) &= T_{MS}\left(\left\lceil\frac{n}{2}\right\rceil\right) + T_{MS}\left(\left\lfloor\frac{n}{2}\right\rfloor\right) + n \\
> &= T_{MS}\left(\left\lceil\frac{\left\lceil n/2 \right\rceil}{2}\right\rceil\right) + T_{MS}\left(\left\lfloor\frac{\left\lceil n/2 \right\rceil}{2}\right\rfloor\right) + \left\lceil\frac{n}{2}\right\rceil + \\
> &\quad T_{MS}\left(\left\lceil\frac{\left\lfloor n/2 \right\rfloor}{2}\right\rceil\right) + T_{MS}\left(\left\lfloor\frac{\left\lfloor n/2 \right\rfloor}{2}\right\rfloor\right) + \left\lfloor\frac{n}{2}\right\rfloor + n \\
> &= \dots ?
> \end{align*}$$

### Insight. Mergesort

> [!obs]+ Insight
> - Assume $n, \frac{n}{2}, \frac{n}{4}, \frac{n}{8}, \dots$ are all even
> - $\implies \exists k, n = 2^{k}$

For $n = 2^k$, after each step:
$$\begin{align*}
T(n) &= T\left( \frac{n}{2} \right) + T\left( \frac{n}{2} \right) + n =  2\colorbox{LightPink}{$T\left(\frac{n}{2}\right)$} + n && [1\text{ step}] \\
&= 2\colorbox{LightPink}{$\left(2T\left(\frac{n}{4}\right) + \frac{n}{2}\right)$} + n = 4\colorbox{LightBlue}{$T\left(\frac{n}{4}\right)$} + 2n && [2\text{ steps}] \\
&= 4\colorbox{LightBlue}{$\left(2T\left(\frac{n}{8}\right) + \frac{n}{4}\right)$} + 2n = 8T\left(\frac{n}{8}\right) + 3n && [3\text{ steps}] \\
&= 16T\left(\frac{n}{16}\right) + 4n && [4\text{ steps}]
\end{align*}$$

> [!obs]+ Guess. Pattern After $m$ Steps
> $$T(n) = 2^mT\left(\frac{n}{2^m}\right) + m \cdot n$$

> [!question]+ How do we know this is always true?
> - Proof (by induction)

We want $T(n)$ to not depend on $T$

Note: $T(\frac{n}{2^k})$ “goes away” when $\frac{n}{2^m} = 0$ or $\frac{n}{2^m} = 1$ (base cases)

> [!warning]- $\frac{n}{2^m} = 0$ is not possible
> - If you start with non-empty list that calls itself recursively, then $n \geq 2$, so it will split in half and be list of at least size 1
> - Impossible to create an empty sublist
> - Don’t have to justify/prove this case, just pick one that works

> [!success]+ Solving for $m$ (# of substitution steps)
> For $n = 2^k$:
> $$\begin{align*}
> n = 2^m &\implies m = \lg n \\
> [n = 2^k &\implies m = k]
> \end{align*}$$
> 
> Then:
> $$\begin{align*}
> T(n) &= 2^m T\left(\frac{n}{2^m}\right) + m \cdot n \\
> &= n \cdot T(1) + (\lg n) \cdot n \\
> &= n(\lg n + 1)
> \end{align*}$$
> 
> Equivalently: $$T(2^k) = 2^k(k+1)$$

> [!note]+ Notation
> $$\lg n = \log_2 n$$

> [!question]+ Prove $\forall k, T(2^{k}) = 2^{k}\cdot (k + 1)$

Define $P(k) : T(2^{k}) = 2^{k} \cdot (k + 1)$ for all $k \in \mathbb{N}$.
Principle of Simple Induction:
$$
\Big[P(0) \wedge \forall k, P(k) \implies P(k+1) \Big] \implies \forall k, P(k)
$$
**Base case.** ($k = 0$)
$$
T(2^{0}) = T(1) = 1 = 2^{0}(0 + 1)
$$
**Inductive step.**
Let $k \in \mathbb{N}$.
Assume $T(2^{k}) = 2^{k}(k + 1)$ i.e., $P(k)$.
(WTP $T(2^{k+1}) = 2^{k+1}(k + 2)$)

Then,
$$
\begin{align*}
T(2^{k+1}) &= T\left( \left\lceil  \frac{2^{k+1}}{2}  \right\rceil  \right) + T\left( \left\lfloor  \frac{2^{k+1}}{2}  \right\rfloor  \right) + 2^{k+1} && (\text{by recurrence relation}) \\
&= T(2^{k}) + T(2^{k}) + 2^{k + 1} \\
&= 2(2^{k}(k+1)) + 2^{k + 1} && (\text{by IH}) \\
&= 2^{k+1}(k+1) + 2^{k+1} \\
&= 2^{k+1}(k + 1 + 1) \\
&= 2^{k+1}(k+2)
\end{align*}
$$
Thus, $P(k + 1)$ holds.

# Finding Worst Case Runtime for Mergesort

From above, 
- We know runtime for powers of 2

![](https://i.imgur.com/UToIoRM.png)

> [!obs]+ Guess: $T$ is non-decreasing.
> $$(\heartsuit) \quad \forall n, \forall m \leq n, T(m) \leq T(n)$$

- This needs to be proved

> [!thm]+ From above, we also know
> $$(\diamondsuit) \quad \forall k, T(2^{k}) = 2^k (k+1)$$

- Prove this (based on recurrence relation)
- Proof by (simple) induction

## Intuition

![](https://i.imgur.com/uCx26Ux.png)

- We know $T$ fits in these boxes
    - Since non-decreasing
- & Can use this information to prove a $\Theta$-bound

## Formalizing Intuition

**Want to Prove:** $T \in \Theta(n\lg n)$

Let $n \in \mathbb{N}$. 

### Case 1: When $n$ is a Power of 2

If $\exists k, n = 2^k$, then $T(n) = n(\lg n + 1)$ (by $\diamondsuit$)

This gives us bounds:
$$n\lg n \leq n(\lg n + 1) \leq n(\lg n + \lg n) = 2n\lg n \quad (n \geq 2)$$

### Case 2: When $n$ is not a Power of 2

For any $n$, there exists $k$ where:
$$2^{k-1} < n < 2^k$$
where $2^k$ is the smallest power of 2 greater than $n$

By $\heartsuit$ (non-decreasing property):
$$T(2^{k-1}) \leq T(n) \leq T(2^k)$$

Therefore:
$$2^{k-1}(k-1+1) \leq T(n) \leq 2^k(k+1)$$
- However, we want this in terms of $n$ for our $\Theta$-bound

> [!obs]+ $\frac{n}{2} < 2^{k-1} < n < 2^k < 2n$
> 1. We start with knowing that $2^{k-1} < n < 2^k$ where $2^k$ is the smallest power of 2 greater than $n$
> 
> 2. Break this down step by step:
>    - $2^{k-1} < n < 2^k$ (given)
>    - Since $2^k = 2 \cdot 2^{k-1}$, we know $2^k < 2n$ 
>      - Because if $2^k \geq 2n$, then $2^{k-1} \geq n$, contradicting $2^{k-1} < n$
>    
> 3. For the leftmost inequality:
>    - We know $2^{k-1} < n$
>    - Therefore $2^{k-1} = \frac{2^k}{2} > \frac{n}{2}$
> 
> 4. Putting it all together:
>    $$\frac{n}{2} < 2^{k-1} < n < 2^k < 2n$$
> 
> This chain of inequalities helps us establish bounds on $k$ in terms of $\lg n$:
> $$\lg n - 1 < k-1 < \lg n < k < \lg n + 1$$
> 

Therefore:
$$\begin{align*}
2^{k}(k + 1) &< 2n(\lg n + 2) \\
&\leq 2n(\lg n + \lg n) && (n \geq 4) \\
&= 4n \lg n
\end{align*}$$
and
$$\begin{align*}
2^{k-1}k &> \frac{n}{2} (\lg n) \\
&= \frac{1}{2} n \lg n
\end{align*}$$
$$\therefore \frac{1}{2} n \lg n \leq T(n) \leq 4n \lg n \quad \text{if } n \geq 4$$
### Conclusion

Since we have both upper and lower bounds in terms of $n\lg n$:
$$T(n) \in \Theta(n\lg n)$$
# Recap and Outline

From Lecture 18.

- We begin with a **recursive algorithm**
    - ==Write **recurrence relation** for worst-case runtime==
    - e.g., Mergesort: $T(n) = T\left( \left\lceil  \frac{n}{2}  \right\rceil \right) + T\left( \left\lfloor  \frac{n}{2}  \right\rfloor \right) + n$
- ==Simplify:==
    - Find “nice” values for $n$
    - Nice values that make $\lfloor \cdots \rfloor, \lceil \cdots \rceil$ unnecessary
        - e.g., “Nice” $n$ = powers of 2 ($\exists k, n = 2^k$)
        - Then, $T(n) = 2T\left( \frac{n}{2} \right) + n$
- ==**Unroll**==; find **closed form**
    - *Prove* the closed form (by induction)
    - e.g., $T(n) = n(\lg n + 1)$
- Prove that $T$ is non-decreasing
    - See practice problems #5
- ==**Interpolate**==
    - Prove $T \in \Theta(\cdots)$ for all $n$
        - e.g., $T \in \Theta(n \lg n)$
    - Use non-decreasing fact and closed-form

See Section L0101 for binary search algorithm.

---

> [!def]+ Divide-and-conquer algorithm
> - An algorithm that is recursive, that recurses in a specific pattern, where the recursion is always:
>     - Take the input,
>     - Break it up into inputs for similar subproblems
>     - Solve those recursively
>     - Take those results and combine them into a solution for the overall problem
> - e.g., Mergesort

> [!thm]+ Claim
> Most **divide-and-conquer** algorithms have worst-case runtimes that satisfy recurrences with general term of the form:
> $$T(n) = a \cdot T\left( \frac{n}{b} \right) + \Theta(n^{d}) \tag{1}$$
> 
> Where:
> - $a$ = number of recursive calls
> - $\frac{n}{b}$ = input size for each recursive call
> - $\Theta(n^d)$ = total non-recursive number of steps
>
> > [!note]+ Parameter Constraints
> > - $a \in \mathbb{N}^+$
> > - $b \in \mathbb{R}, b > 1$
> >     - Input needs to be smaller for each recursive call
> > - $d \in \mathbb{R}^{\geq 0}$
> >     - Otherwise, as $n \to \infty$, runtime is faster (no such algorithm exists)
> > - Ignore $\lfloor \cdots \rfloor, \lceil \cdots \rceil, \pm 1, \pm$ any constant in $\frac{n}{b}$

- One thing conspicuously missing from (1):
    - If $n$ is not a multiple of $b$, then $\frac{n}{b}$ is not going to be an integer
    - Doesn’t matter
    - Mathematically, going to work out to the same thing in $\Theta$ terms
    - Anything $\frac{n}{b}$ no more than $\pm$ some constant, we count as $\frac{n}{b}$
- e.g., **Mergesort**: $T(n) = 2T\left( \frac{n}{2} \right) + \Theta(n^1)$
    - $a= 2, b = 2, d = 1$
