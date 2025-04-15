---
dg-publish: true
tags: ["#lecture", "#note", cs, university]
Course Code:
  - "[[CSC236]]"
Week: 6
Module:
  - "[[2 - Algorithm Analysis]]"
Chapter:
Lecture:
  - "10"
  - "11"
  - "12"
  - "13"
Slides/Notes:
  - https://share.goodnotes.com/s/kuoUxAbBof018vHtdKUoOl
Date: 2024-10-07
aliases: [Algorithm Correctness]
Date created: Tue., Oct. 8, 2024, 4:53:54 pm
Date modified: Mon., Dec. 9, 2024, 6:55:16 am
---

- **Precondition**
    - *Assumed* to be true before code runs
    - Intention: Describes what is a “*valid input*”
    - Wanted: *Weak* (few constraints)
        - If we had a strong constraint e.g., input must be 20, then what’s the point?
        - Want the minimum required
- **Postcondition**
    - Must *prove* after code runs
    - Intention: “*Expected*” output/outcome
    - Wanted: *Strong*
        - Want to say as much as possible

> [!important]+ Every piece of code you write has a *contract*
>
> - Only have to worry about inputs that meet the precondition
> - For all of those inputs, you need to make sure the ==post-condition is true== at the end

> [!def]+ Correctness
>
> - For *each* input that satisfies the precondition, the ==algorithm terminates==
>     - i.e., No infinite loop/recursion
> - And when it does, the ==post-condition *holds*==

# Example. QRT

Implementing the Quotient-Remainder Theorem from [[Well-Ordering#Example. Quotient Remainder Theorem|well-ordering]].

```python title:"Quotient-Remainder Theorem"
# Pre(n, d): n in naturals, d in positive naturals
# Returns (q, r) s.t. Post(n, d, q, r):
#    (1) q, r in naturals
#    AND (2) r < d
#    AND (3) n = qd + r
def QR(n, d):
    q, r = 0, n  # Starting point: Satisfies (1) and (3) of Post
    while r >= d:  # Make r smaller
        q = q + 1
        r = r - d
    return (q, r)
```

- Implemented this idea we had from the proof of the Quotient-Remainder Theorem
    - Can always start from remainder = $n$
    - Then $(0, n)$ satisfy $n = qd + r$ because $d \neq 0$
    - Idea in the proof was:
        - If remainder $r \geq d$, can subtract $d$ from $r$ and adding $1$ to $q$
        - Carry out this process until we hit the condition that we want
- “I have an algorithm, and I know where it came from. I’m doing this because I know it makes sense. How do I formalize what it means for this algorithm to be correct, and how do I prove that?”
    - State clear pre/post-conditions

> [!question]+ Why does this work? How do we prove?
>
> - Proof will overlap proof in [[Well-Ordering|quotient-remainder theorem]]
> - Difference:
>     - In QRT, we were proving that $q, r$ *exist*
>     - Here, we have to not only prove that $q, r$ exist, but $q, r$ that we found *works*
>     - ==Make sure $q, r$ are correct==

# Tool: Loop Invariants

- A **loop invariant** is a way to keep track of what you are doing, and so on, that helps you write a correct loop

> [!def]+ Loop invariant
>
> - Any statement about or predicate of the program variables that is true
>     - Just before every loop iteration begins, or equivalently,
>     - Just after each loop iteration ends

```python title:"Quotient-Remainder Theorem"
# Pre(n, d): n in naturals, d in positive naturals
# Returns (q, r) s.t. Post(n, d, q, r):
#    (1) q, r in naturals
#    AND (2) r < d
#    AND (3) n = qd + r
def QR(n, d):
    q, r = 0, n

    # I(n, d, q, r) - We assert is true at [*]
    while [*] r >= d:
        q = q + 1
        r = r - d
    return (q, r)
```

- & `I(n, d, q, r)`
    - Something that depends on all program variables
    - Something that we assert at the beginning of the loop
- In `q = q + 1`, `r = r - d`:
    - Each variable is being used in two different ways
    - Once to *get* value of variable
    - Once to *update* value of variable
- ==When you have a loop → You have a sequence==
    - More than one value that the loop variables take on

# Loops and Sequences

- `q, r = 0, n`
- Is `r >= d`?
- If it is → Run loop body:
    - `q = q + 1`
    - `r = r - d`
- Then, go back to condition: `r >= d`?
- If it is → Run loop body:
    - `q = q + 1`
    - `r = r - d`
- `r >= d`?

> [!tip]+ Index these variables
>
> - Something that you make up for your own convenience
> - Not apart of the code!
> - Part of how you keep track of and refer to the value of a variable at different points of the execution
>     - Value changes → Cannot say `r = …`
>     - Which `r` are you talking about?
>     - → ==Use indexing==

![](https://i.imgur.com/IQOPuiJ.png)

- [*] $r_{k} =$ value of $r$ at the end of $k$ complete iterations
    - Special case $k = 0$ means before you start the loop
    - $r_{0}$ is before you do the loop body once, and
    - $r_{1}$ is before you do the loop body a second time, and so on

> [!obs] This gives us notation we can use to refer to the values of variables at different points during the execution

![](https://i.imgur.com/dFFhe2B.png)

> [!question]+ How do we figure out what is true at the point of the loop invariant?
>
> - ==Look at pre and post conditions==
> - Whole point of invariant is to prove the post conditions
> - Not going to be able to grab the whole post-condition and make it the invariant
>     - Are things that are true at end of algorithm that are not true half way through
>     - e.g., $r < d$, or else the loop would be pointless (condition would always be false)
>         - Whole point of loop is to step through various possible remainders to make them smaller and smaller until eventually we reach $r < d$
>         - But not part of the invariant

## Coming up with *Good* Invariants

1. Look to post condition
    - Invariant *supports* proof of Post
2. Trace
    - ![|400](https://i.imgur.com/w6aJMPj.png)
    - Did not include `d` and `n` because they are constant/do not change
    - Tracing expressions vs. tracing values
        - Did not instantiate values here; more abstract

- Want to say $n = qd + r \implies r = n - qd$
    - Which is what we found when we traced
- *and* $q, r \in \mathbb{N}$
    - If we didn’t include, there is no formal way to prove that this is true after some number of iterations
    - Need to talk about each iteration → Is an invariant

- & Define $I(n, d, q, r): q, r \in \mathbb{N} \wedge n = qd + r$:

```python title:"Quotient-Remainder Theorem"
# Pre(n, d): n in naturals, d in positive naturals
# Returns (q, r) s.t. Post(n, d, q, r):
#    (1) q, r in naturals
#    AND (2) r < d
#    AND (3) n = qd + r
def QR(n, d):
    q_0, r_0 = 0, n

    # I(n, d, q, r): q, r are in naturals AND n = qd + r
    while [*] r_k >= d:
        q_{k+1} = q_k + 1
        r_{k+1} = r_k - d
    return (q, r)
```

> [!question]+ How do we know if this is a good loop variant?
> A good loop invariant…
>
> 1. Helps prove the post condition
>     - when the loop ends (==assumption==)
> 2. Is invariant → Check that it is actually *is* invariant
>     - Otherwise, proving something based on fallacy (bad!)
>
> - Doesn’t matter what order this is proved in, but recommended to check if it proves Post first
>     - Typically, that is where you find out you are missing something in invariant → Don’t know enough to conclude Post
>     - Want to find that out before you spend time proving it is invariant

# Proof

#### Proof of 1. (Post condition)

**Proof.**
Assume loop ends.

When the loop ends, we know:

- $r < d$ (from negation of loop condition), <u>and</u>
- $I: q, r \in \mathbb{N} \wedge n = qd + r$ (from loop invariant)

$\therefore Post(n, d, q, r)$

#### Proof of 2. (Invariant)

**Rough work.**
Check that it actually *is* invariant:
![](https://i.imgur.com/XfyX9rj.png)

- At the very beginning, $I_{0}$ is true
- If you can prove $I_{k + 1}$, then you are done
- Whenever $I_{k}$ is true and there is one more condition ($r_{k} \geq d$), $I$ ends still true
- Coming back for the next iteration, $I$ is still true
- This is [[Simple Induction]]

**Proof of Invariance.**

- $I_{0}$ holds (see comments)
- Let $k \in \mathbb{N}$. Assume $I_{k}$ and $r_{k} \geq d$ (i.e., there is one more iteration)
- Then,
    $$\begin{align*}
    q_{k+1} &= q_{k} + 1 \\
    r_{k+1} &= r_{k} - d
    \end{align*}$$
- So:
    - $q_{k + 1} \in \mathbb{N}$ because $I_{k}$ (says $q_{k} \in \mathbb{N}, r_{k} \in \mathbb{N}, n = q_{k}d + r_{k}$)
    - $r_{k+1} \in \mathbb{Z}$ and $r_{k} \geq d \implies r_{k+1} \in \mathbb{N}$
    - $$\begin{align*} n &= q_{k}d + r_{k} \\ &= (q_{k}+1)d + (r_{k}-d) \\ &= q_{k+1}d + r_{k+1} \end{align*}$$

> [!warning]+ Conclusion: $\forall k \in \mathbb{N}, I_{k}$
>
> - Saying that $I_{k}$ is true no matter how many iterations you make
> - Don’t want the code to run to infinity; ==want loop to stop!==
> - Some point where $I_{k}$ is not defined
> - **Missing:** $I_{k}$ is *really* $$(\text{There are at least } k \text{ complete iterations}) \implies I(n, d, q_{k}, r_{k})$$

So, the inductive step proves $$( k \text{ iter} \implies I_{k}) \implies ((k + 1 \text{ iter}) \implies I_{k+1})$$

## Why Does the Loop Stop? How Do We *Formalize* This?

> [!def]+ Loop Variant
>
> - $V(n, d, q, r):$ expression that *bounds* the amount of work remaining

> [!thm]+ Formally, a **variant** satisfies:
>
> 1. $\forall k, V_{k} \in \mathbb{N}$
> 2. $\forall k, V_{k+1} < V_{k}$
>
> - “By WOP, the loop terminates”

> [!note] $V_{k}$ is the value of $V$ just before it executes for the $k + 1$-st time

- Intuition:
    - $r$ keeps getting smaller → Eventually going to be $<d$

> [!tip]+ We pick $V = r$
>
> - $V_{k} \in \mathbb{N} \because r_{k} \in \mathbb{N}$ is in invariant
>     - Likely want to include something about natural numbers in invariant to use in variant
> - $r_{k+1} = r_{k} - d < r_{k} \because d > 0$

- Conclusion: The loop stops
- This is well-ordering
    - We know it cannot go on forever
    - If variant starts off a natural number ($V_{0}$), and every iteration makes it smaller, whatever the value is at the beginning, there cannot be more than that many iterations, because there are only that many values between 0 and that natural number
    - → Loop terminates!

**Proof.**

Formally, $V_{0} > V_{1} > V_{2}, > \dots$

- $S = \{ V_{k} : \text{for each iteration } k \}$
- $S \neq \emptyset \because V_{0} \in S$
    - There is always at least one look at loop condition
    - $k = 0$ is *before* the loop starts
- By [[Well-Ordering|WOP]], $S$ has a minimum $V_{m}$
- $\therefore m$ is the last iteration
- Otherwise, $V_{m + 1} < V_{m}$ (by property 2.) contradicts that $V_{m}$ is the smallest element

> [!note]+ The number of elements in $S \neq$ the number of iterations in loop
>
> - It is equal to the number of iterations in loop + 1

# Invariant vs. Variant

> [!important]+ Invariant vs. Variant
>
> - **Loop variant**
>     - A quantity that is supposed to change from one iteration to the next
>     - Use to ==prove termination==
> - **Loop invariant**
>     - A set of statements about your algorithm that always holds
>     - Use to ==prove correctness==

# Example. Finding the Minimum of a List

```python
# Pre(A, b): A is a non-empty list of comparable elements
#            ∧ b in naturals ∧ b < len(A)
# Return m s.t. Post(A, b, m):
# m in naturals ∧ b <= m < len(A)
# ∧ "m is the index of a minimum element in A[b: len(A)]"
# ≡ A[m] <= A[b:len(A)] (shorthand; new notation!)
# ≡ ∀i, b <= i < len(A) => A[m] <= A[i]
def find_min(A, b):
    m = b
    for i in range(b + 1, len(A)):
        if A[i] < A[m]: m = i
    return m
```

> [!note]+ $A[m] \leq A[b:len(A)]$
>
> - New notation
> - Shorthand for $$\begin{align*} A[m] \leq A[b] \wedge A[m] \leq A[b+1] \wedge \dots \wedge A[m] \leq A[len(A) - 1] \\ \equiv \forall i, b \leq i < len(A) \implies A[m] \leq A[i]\end{align*}$$

# Partial Correctness and Termination

> [!def]+ Partial Correctness
> For each input $(A,b)$,
> $$\text{Pre}(A,b) \wedge \text{find\_{min}}(A,b) \text{ terminates} \implies \text{Post}(A, b, \text{find\_{min}}(A, b))$$

> [!def]+ Termination
> For each input $(A, b)$,
> $$\text{Pre}(A, b) \implies \text{find\_{min}}(A, b) \text{ terminates}$$

- Why not combine and prove $\text{Pre}(A, b) \implies \text{find\_{min}}(A, b) \text{ terminates} \wedge \text{Post}(A, b, \text{find\_{min}}(A, b))$?
    - In practice, with loop invariant, helpful to do partial correctness first, then prove termination through the use of variant

# Turn For Loops into While Loops

- While loop makes ==explicit sum of the steps== that are implicit in the for loop

```python
def find_min(A, b):
    m = b
    i = b + 1

    # I(m, i) : m, i in naturals ∧ b <= m < i <= len(A)
    while i < len(A):
        if A[i] < A[m]: m = i
        i = i + 1
    return m
```

- ? Why do we have $i \leq \text{len}(A)$ vs. $i < \text{len}(A)$?
    - Loop stops when $i = \text{len}(A)$
    - We add 1 to $i$ at the end of every iteration
- ? Is $I(m , i)$ useful?
    - i.e., Can you use $I$ to prove Post?
    - No: When loop ends, we know:
        - $i \geq len(A)$ (from loop condition)
        - $m, i \in \mathbb{N} \wedge b \leq m < i \leq len(A)$
        - $\therefore i = len(A)$
    - Does $A[m] \leq A[b:len(A)]$ hold?

# What is the Loop Doing?

![|500](https://i.imgur.com/RsEEdXk.png)

- $i$ keeps track of everything that we have looked at
- ==$m$ is the minimum so far==
- **Missing**: $A[m] \leq A[b:i]$
    - $i$ and not $i + 1$ since we add 1 to $i$ at the *end* of every iteration

<!-- break -->
- At the end, we get:
    - $A[m] \leq A[b:i] = A[b:len(A)]$
- Post is proved *if* $I(m, i)$ holds
- Next step: Prove $I(m, i)$

# Proof of Correctness

- Define $$I(m, i) : m, i \in \mathbb{N} \wedge b \leq m \leq len(A) \wedge A[m] \leq A[m:i]$$
- ==Use a dummy variable== (e.g., $k$) that is not used in program to refer to variables whose values change multiple times

![](https://i.imgur.com/Iv4O489.png)

- ? Why don’t we put the stronger condition $i < len(A)$ in invariant $I$?
    - Need to make sure it is true for every iteration ==including the final comparison that is false!==
    - If $i < len(A)$ every time you’re about to evaluate the condition $\implies$ Claiming loop is infinite
    - ! Invariant should never claim anything that implies the loop condition is always true
