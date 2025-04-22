---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 1
Module:
  - "[[1 - Introduction and Analyzing Running Time]]"
Lecture:
  - "2"
Chapter: "1"
Slides/Notes:
  - "[[Algorithm Analysis]]"
Date: 2025-01-09
Date created: Sat., Jan. 11, 2025, 3:32:18 pm
Date modified: Wed., Apr. 16, 2025, 5:27:21 pm
---

# Introduction and Analyzing Running Time

The reason we study data structures at all, spending time inventing and refining more complex ones, is largely because of the ==performance improvements== we can hope to gain over their more primitive counterparts.

## How Do We Measure Running Time?

- We focus on one central measure of performance:
    - Relationship between an algorithm’s **input size**, and
    - **Number of basic operations** the algorithm performs
- Categorize ==how the number grows== relative to the size of the algorithm’s input

> [!note]+ The functions $n + 1$, $3n - 10$, $0.001n + \log n$ have the same growth behaviour as $n$ gets large.
>
> - All grow roughly linearly with $n$

- Despite different slopes, we ignore these constants and simply say that these functions are $\mathcal{O}(n)$
    - Big-Oh of $n$
- **Asymptotic analysis**
    - Deals with long-term behaviour of a function

> [!important]+ Two important facts
>
> 1. Target of the analysis is always a ==relationship== between the *size of the input* and the *number of basic operations* performed
> 2. Result of analysis is a *qualitative rate of growth*
>     - Not an exact number or even exact function

## Three Different Symbols

> [!def]+ Big-Oh
>
> - $f = \mathcal{O}(g)$
> - Function $f(x)$ grows *slower or at the same rate* as $g(x)$
>     - i.e., $g$ is an *upper bound* on the rate of growth of $f$

- $x^{2} + x = \mathcal{O}(x^{2})$
    - Also correct to write $x^{2} + x = \mathcal{O}(x^{100})$, or $x^{2} + x = \mathcal{O}(2^{x})$

> [!def]+ Omega
>
> - $f = \Omega (g)$
> - Function $f(x)$ grows *faster or at the same rate* as $g(x)$
>     - i.e., $g$ is a *lower bound* on the rate of growth of $f$

- $x^{2} + x = \Omega(x^{2})$
    - Also correct to write $x^{2} + x = \Omega(x)$, or $x^{2} + x = \Omega(\log \log x)$

> [!def]+ Theta
>
> - $f = \Theta(g)$
> - Function $f(x)$ grows *at the same rate* as $g(x)$
>     - i.e., $g$ has the same growth as $f$

- $x^{2} + x = \Theta(x^{2})$
    - Equivalent to saying that $f = \mathcal{ O }(g)$ and $f = \Omega(g)$
    - Theta is really an AND of Big-Oh and Omega
- ==Not correct== to say $x^{2} + x = \Theta(2^{x})$ nor $x^{2} + x = \Theta(x)$

> [!abstract]+ Reminder
>
> - Not the case in this course that the obvious upper bound will always be the *actual* rate of growth

## Worse-Case Analysis

- Given any exact mathematical function, it is always possible to determine its qualitative rate of growth
    - i.e., corresponding Theta expression

> [!question]+ Then, why do we need Big-Oh and Omega at all?
>
> - Cannot always take a piece of code and come up with an exact expression for the number of basic operations performed

- Consider a function that takes a list and returns whether this list contains the number 263:
    - If function loops through list starting at the front and stops immediately if it finds an occurrence of 263, then the function depends on:
        - How ==long== the list is, and
        - ==Whether and where it has any 263 entries==

- Asymptotic notations alone cannot help solve this problem
    - Help clarify *how* we are counting, but
    - Have a problem of *what* exactly are we counting

> [!info] This problem is why **asymptotic analysis** is typically specialized to **worst-case analysis**.

> [!def]+ Worst-case analysis
>
> - Studies only the relationship between input size of *maximum possible* running time

- Rather than asking the question: “What is the running time of this algorithm for an input size $n$?”,
    - Aim to answer the question “What is the ==maximum possible== running time of this algorithm for an input size $n$?”
- First question’s answer might be a whole range of values
- Second question’s answer can only be a single number
    - → Get a function involving $n$

> [!note]+ Notation
>
> - $T(n)$ represents the ==maximum possible running time== as a function of $n$, the input size
> - Result can be something like $T(n) = \Theta(n)$
>     - Worst-case running time of our algorithm grows linearly with the input size

### Bounding the Worst Case

- Even with restricted focus on worst-case running time:
    - Not always possible to calculate an exact expression for this function
- Usually easier to calculate an *upper bound* on the maximum number of operations
    - e.g., “The loop will run at most $n$ times” when searching for 263 in a list of length $n$
    - Gives a pessimistic outlook on the ==most== number of operations that count theoretically happen
    - Results in an exact count (e.g., $n + 1$) which is an *upper bound* on the maximum number of operations
    - From this analysis, we can conclude that $T(n)$ worst-case running time is $\mathcal{ O }(n)$ in this example
- ! Cannot conclude that $T(n) = \Omega(n)$
    - Have no way of knowing that the upper bound we obtain by being pessimistic in our operation counting is actually ==achievable==

> [!question]+ How do we show that whatever upper bound we get on the maximum is actually achievable?
>
> - Rarely try to show that the *exact* upper bound is achievable; doesn’t actually matter
> - & We try to show that an ==asymptotic lower bound $\Omega$ is achievable==

> [!tip]+ To show that the maximum running time grows *at least as quickly* as some function $f$,
>
> - Find a family of inputs
>     - One for each input size $n$
>     - Whose running time has a lower bound of $f(n)$

- e.g., For our problem of searching for 263 in a list:
    - Could say that family of inputs is “lists that contain only o’s”
    - Running search algorithm on such a list of length $n$ requires checking each element
    - Conclude that the *maximum* possible running time is $\Omega(n)$

> [!summary] Performing a complete worst-case analysis to get a tight Theta bound on the worst-case running time
>
> 1. Give a pessimistic *upper bound* on the number of basic operations that could occur for any input of a fixed size $n$
>     - Obtain corresponding Big-Oh expression i.e., $T(n) = \mathcal{ O }(f)$
> 2. Give a *family of inputs* — one for each input size — and give a *lower bound* on the number of basic operations that occurs for this particular family of inputs
>     - Obtain corresponding Omega expression i.e., $T(n) = \Omega(f)$
>
> - Good analysis → Will find that Big-Oh and Omega expressions involve the same function $f$ → Conclude that worst-case running time is $T(n) = \Theta(f)$

## Average Case Analysis

- In practice, worst-case algorithm analysis can be misleading
    - A variety of algorithms and data structures having a poor worst-case performance can still perform well on the majority of possible inputs

> [!obs] Focusing on the maximum of a set of numbers (like running times) says very little about the typical number in that set, or the ==distribution of numbers within that set==.

### Example. Evens Are Bad

```python title:"Evens Are Bad"
def evens_are_bad(lst):
    if every number in lst is even:
        repeat lst.length times:
            calculate and print the sum of lst
        return 1
    else:
        return 0
```

^4b51c8

- Let $n$ represent the length of the input list `lst`
- Suppose that `lst` contains only even numbers:
    - Initial check on line 2 takes $\Omega(n)$ time
        - Checking if every number is even
    - Computation in the `if` branch takes $\Omega(n^{2})$ time
        - Each number is accessed $n$ times, once per time the sum is computed
    - → Worst-case running time of algorithm is $\Omega(n^{2})$
    - Prove the matching upper bound → Worst case running time of algorithm is $\Theta(n^{2})$

> [!obs]+ Loop only executes when every number in `lst` is even
>
> - When just one number is odd, the running time is $\mathcal{ O }(n)$
>     - Maximum possible running time of executing the all-even check
>     - Executing the check might abort quickly if it finds an odd number early in the list
>     - → Use pessimistic upper bound of $\mathcal{ O }(n)$ rather than $\Theta(n)$
> - & Intuitively: Seems more likely that *not* every number in `lst` is even
>     - Expect the more “typical case” to have running time bounded above by $\mathcal{ O }(n)$
>     - Only rarely have a running time of $\Theta(n^{2})$

> [!def]+ Average-case running time
> $$T_{avg}(n)$$
>
> - Takes a number $n$
> - Returns the ==weighted average== of the algorithm’s running time for all inputs of size $n$

#### Setting Up the Context of Our Analysis

> [!tldr]+ Setting Up the Context of Our Analysis
>
> - What inputs are we considering, and
> - How are we measuring runtime?

- Fix some input size $n$
    - Want to compute average of all running times over all inputs of length $n$
- First, ==define the possible set of inputs==
    - e.g., The lists whose elements are between 1 and 5, inclusive
    - → Simplifies the calculations we need to perform when computing a weighted average
- We need to be precise about what “basic operations” we are counting
    - e.g., Count only the ==number of times a list element is accessed==
        - Either check whether it is even, or when computing the sum of the list
        - Step is synonymous with “list access”

> [!note]+ It is fairly realistic to focus solely on operations of a particular type in a runtime analysis.
>
> - Typically choose the operation that happens most frequently
>     - As in this case, or
> - Operation that is the most expensive
>     - Requires that we are intimately knowledgeable about low-level details of our computing environment

#### Computing the Average Running Time

- Final step is to ==compute average running time over inputs of length $n$==
    - Requires some calculation

- To simplify calculations even further:
    - Assume that all-evens check on line 2 always accesses all $n$ elements
    - In the loop, there are $n^{2}$ steps

> [!obs]+ Two possibilities
>
> - Lists that have all even numbers will run in $n^{2} + n$ steps
> - All other lists will run in $n$ steps

> [!question]+ How many of each type of list are there?
>
> - For each position there are two possible even numbers
>     - 2 and 4
> - Number of lists of length $n$ with every element being even is $2^{n} = 2 \text{ options} \times 2 \text{ options} \dots \times 2$
> - There are five possible values per element → $5^{n}$ possible inputs in all

- $2^{n}$ possible inputs have all even numbers
    - Take $n^{2} + n$ steps
- Remaining $5^{n} - 2^{n}$ inputs take $n$ steps

$$
\begin{align}
T_{avg} (n) & = \frac{{2^{n}(n^{2} + n) + (5^{n} - 2^{n})n}}{5^{n}} && (5^{n} \text{ inputs total}) \\
 & = \frac{2^{n}n^{2}}{5^{n}} + n \\
 & = \left( \frac{2}{5} \right) ^{n} n^{2} + n \\
 & = \Theta (n)
\end{align}
$$

> [!note] Any exponential grows faster than any polynomials
>
> - First term goes to 0 as $n$ goes to infinity

- The average-case running time of this algorithm is ==$\Theta(n)$==
- We computed an exact expression for the average number of steps
    - → Could convert this directly into a Theta expression

### The Probabilistic View

- Analysis in example was done through counting the different kinds of inputs and then computing the average of their running times
- Instead:
    - & Treat the algorithm’s running time as a *random variable* $T$, defined over the set of possible inputs of size $n$

> [!summary]+ Average-case analysis
>
> 1. Define the set of possible inputs and a **probability distribution** over this set
>     - e.g., Our set is all lists of length $n$ that contain only elements in the range 1-5
>         - Probability distribution is the **uniform distribution** i.e., equal probability for each element in set
> 2. Define how we are measuring runtime
>     - Unchanged from previous analysis
> 3. Define random variable $T$ over this probability space to represent the running time of algorithm
>     - e.g., We have the definition $$T(n) = \begin{cases} n^{2} + n, && \text{input contains only even numbers}\\ n, && \text{otherwise} \end{cases}$$
> 4. Compute the **expected value of $T$**, using the chosen probability distribution and formula for $T$
>     - $$\mathbb{E}[T] = \sum_{t} t \cdot \text{Pr}[T = t]$$
>     - In our example, there are only two possible values for $T$:
>
>         $$
>         \begin{align}
>         \mathbb{E}[T] & = (n^{2} + n) \cdot \text{Pr}[\text{the list contains only even numbers}] \\
>         & + n \cdot \text{Pr}[\text{the list contains at least one odd number}] \\
>          & = (n^{2} + n) \cdot \left( \frac{2}{5} \right)^{n} + n \cdot \left( 1 - \left( \frac{2}{5} \right)^{n} \right)  \\
>          & = n^{2} \cdot \left( \frac{2}{5} \right) ^{n} + n \\
>          & = \Theta(n)
>         \end{align}
>         $$

> [!tip]+ Indicator Variables
> In general, when computing average-case running times, we want to compute $\mathbb{E}[X]$ where $X$ *counts* the number of times certain operations are executed. Trying to come up with exact expressions for $Pr[X = i]$ is generally difficult and messy.
>
> - Try to break up $X$ in to a number of indicator random variables $X_{1}, X_{2}, \dots, X_{m}$
>     - Each one of which *counts* only part of the total value
> - Define $X_{i}$‘s appropriately so two conditions hold:
>     - & $X = X_{1} + X_{2} + \dots + X_{m}$
>     - & Each $X_{i}$ has only two possible values: 0 or 1
> - Compute $\mathbb{E}[X]$ as follows
>
> $$
> \begin{align}
> \mathbb{E}[X]  & = \mathbb{E}[X_{1} + \dots + X_{m}] \\
>  & = \mathbb{E}[X_{1}] + \dots + \mathbb{E}[X_{m}] & & (\text{by linearity of expectation}) \\
>  & = Pr[X_{1} = 1] + \dots + Pr[X_{m} = 1]
> \end{align}
> $$
>
> > [!note]- The last equality holds because each $X_{i}$ can only be equal to 0 or 1.
> > By definition,
> > $$
> > \begin{align}
> > \mathbb{E}[X_{i}] & = 0 \times Pr[X_{i} = 0] + 1 \times Pr[X_{i} = 1] \\
> >  & = Pr[X_{i} = 1]
> > \end{align}
> > $$

^0d2672

## Exercises

![[#^4b51c8]]

Disclaimer: The following answers are my own and not provided.

> [!question]+ Prove that the `evens_are_bad` algorithm has a worst-case running time of $\mathcal{ O }(n^{2})$, where $n$ is the length of the input list.
>
> - Let $n$ be the length of the input list `lst`.
> - The check on line 2 takes at most $n$ steps.
> - The loop inside the `if` branch iterates at most $n$ times
>     - The calculation of the sum on line 4 either executes or does not execute and takes at most $n$ steps
> - The return statement on line 5 is constant time.
> - So, $T(n) = n^{2} + n + 1 = \mathcal{ O }(n^{2})$

> [!question]+ Consider this alternate input space for `evens_are_bad`: Each element in the list is even with probability $\frac{99}{100}$, independent for all other elements. Prove that under this distribution, the average-case running time is still $\Theta(n)$.
>
> - The set of possible inputs is all lists of length $n$ with each element in the list is even with probability $\frac{99}{100}$ and each element is odd with probability $\frac{1}{100} = 1 - \frac{99}{100}$
> - We count only the number of times a list element is accessed
> - Let $T$ be a random variable with definition
>     $$
>     T = \begin{cases}
>     n^{2} + n, && \text{input contains only even numbers} \\
>     n, && \text{otherwise}
>     \end{cases}
>     $$
> - Then the probability that a list of size $n$ contains only even numbers is $\left( \frac{99}{100} \right)^{n}$
> - The probability that a list of size $n$ contains at least one odd number is $1 - \left( \frac{99}{100} \right)^{n}$
>
> $$
> \begin{align}
> \mathbb{E}[T]  & = (n^{2} + n) \cdot \left( \frac{99}{100} \right) ^{n} + n \cdot \left( 1 - \left( \frac{99}{100} \right)^{n} \right) \\
>  & = n^{2} \cdot \left( \frac{99}{100} \right)^{n} + n \cdot \left( \frac{99}{100} \right)^{n} + n - n \cdot \left( \frac{99}{100} \right)^{n} \\
>  & = n^{2} \cdot \left( \frac{99}{100} \right)^{n} + n \\
>  & = \Theta(n)
> \end{align}
> $$

> [!question]+ Suppose we all the “all-evens” check on line 2 of `evens_are_bad` to stop immediately after it finds an odd element. Perform a new average-case analysis for this mildly-optimized version of the algorithm using the the same input distribution as in the notes.
>
> > [!hint] Part of the running time $T$ now can take on any value between 1 and $n$. First compute the probability of getting each of these values, and then use the expected value formula.

## Quicksort is Fast on Average

Sometimes, an algorithm can have a significantly better average case than worst case, but it is nearly not as obvious.

Recall the *quicksort* algorithm:

- Takes a list and sorts it by choosing one element to be the *pivot*
    - In our implementation: first element
- Partitions the remaining elements into two parts:
    - Those less than (or equal to in our implementation) the pivot
    - Those greater than the pivot
- Recursively sorts each part, then
- Combines the results

```python title:Quicksort
def quicksort(array):
    if array.length < 2:
        return
    else:
        pivot = array[0]
        smaller, bigger = partition(array[1:], pivot)
        quicksort(smaller)
        quicksort(bigger)
        array = smaller + [pivot] + bigger  # array concatenation

def partition(array, pivot):
    smaller = []
    bigger = []
    for item in array:
        if item <= pivot:
            smaller.append(item)
        else:
            bigger.append(item)
    return smaller, bigger
```

> [!note] This version of `quicksort` uses linear size auxiliary storage.
>
> - Simpler to analyze
> - In-place version of quicksort has the same running time behaviour
>     - Just uses less space

> [!obs]+ The choice of pivot is crucial.
>
> - Determines the size of each of the two partitions
> - Best case: Pivot is the median
>     - Remaining elements are split into partitions of roughly equal size
>     - → Running time of $\Theta(n \log n)$, where $n$ is the size of the list
> - Worst case: Pivot is always chosen to be the maximum element
>     - Algorithm recurses on a partition that is only one element smaller than the original
>     - → Running time of $\Theta(n^{2})$

> [!goal] What is the average-case running time?

### Finding the Average-Case Running Time of Quicksort

- Let $n$ be the length of the list.
- Our set of inputs is all possible permutations of the numbers 1 to $n$, inclusive.
- Assume any of these $n!$ permutations are equally likely
    - i.e., We will use uniform distribution over all possible permutations of $\{ 1, \dots, n \}$.
    - → Analysis will therefore assume lists have no duplicates
- We will measure runtime by ==counting the number of comparisons between elements in the list in the `partition` function==.
<!-- break -->
- Let $T$ be the random variable counting the number of comparisons made.
- For each $i, j \in \{ 1, \dots, n \}$ with $i < j$, let $X_{ij}$ be the **indicator random variable** defined as:
    $$
    X_{ij} = \begin{cases}
    1, && \text{if } i\text{ and } j \text{ are compared} \\
    0, && \text{otherwise}
    \end{cases}
    $$
    - (Define additional random variables)

> [!obs]+ Each pair is compared at most once.
> We obtain the total number of comparisons by adding these up:
>
> $$
> T = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} X_{ij}
> $$

> [!thm]+ Linearity of Expectation
> If $X, Y, Z$ are random variables and $X = Y + Z$,
>
> $$
> \mathbb{E}[X] = \mathbb{E}[Y] + \mathbb{E}[Z]
> $$

- We decompose $T$ into a sum of simpler random variables → Apply the *linearity of expectation*

$$
\begin{align}
\mathbb{E}[T]  & = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} \mathbb{E}[X_{ij}] \\
 & = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} \text{Pr}[i \text{ and } j \text{ are compared}]
\end{align}
$$

> [!note] Recall that for an indicator random variable, its expected value is the probability that it takes on the value 1.

#### Investigating the Probability that $i$ and $j$ Are Compared

> [!goal] We need to investigate the probability that $i$ and $j$ are compared when we run quicksort on a random array.

> [!thm]+ **Proposition 1.1.**
> Let $1 \leq i < j \leq n$.
> The probability that $i$ and $j$ are compared when running quicksort on a random permutation of $\{ 1, \dots, n \}$ is $\frac{2}{j - i + 1}$.

*Proof.*

> [!info]+ Quicksort
>
> - Selects a pivot element, then
> - Compares the pivot to every other item to create two partitions, and then
> - Recurses on each partition separately
>
> > [!obs]+ Two observations
> >
> > 1. Every comparison must involve the “current” pivot.
> > 2. The pivot is only compared to the items that have always been placed into the same partition as it by all previous pivots.
> >     - Once two items have been put into different partitions, they will never be compared.

- In order for $i$ and $j$ to be compared:
    - & One of them must be chosen as the pivot, while
    - & The other number is in the same partition
- ? What could cause $i$ and $j$ to be in different partitions?
    - Only happens if one of the numbers between $i$ and $j$ (exclusive) is ==chosen as a pivot before $i$ or $j$ is chosen==
        - Exclusive because of our implementation: `if item <= pivot: smaller.append(item)`
- & $i$ and $j$ are compared by quicksort $\iff$ One of them is selected to be the pivot first out of the set of numbers $\{ i, i+1, \dots, j \}$

<!-- break -->
- Our implementation:
    - Chooses the first item to be the pivot
    - → Item that is chosen first as pivot must be the one that appears first in the random input permutation
- Since we choose permutation uniformly at random → Each item in the set $\{ 1, \dots, j \}$ is equally likely to appear first
- → Equally likely to be chosen first as pivot

<!-- break -->
- There are $j - i + 1$ numbers in the set → Probability that either $i$ or $j$ is chosen first is $\frac{2}{j - i + 1}$
    - Also the probability that $i$ and $j$ are compared

<div class="right-align"> ■ </div>

#### Calculating the Expected Value

> [!thm]+ **Theorem 1.2** (Quicksort average-case runtime).
> The average number of comparisons made by quicksort on a uniformly chosen random permutation of $\{ 1, \dots, n \}$ is $\Theta(n \log n)$.

*Proof.*

Let $T$ be the random variable representing the running time of quicksort.

$$
\begin{align}
\mathbb{E}[T]  & = \sum\limits_{i=1}^{n} \sum\limits_{j=i+1}^{n} \text{Pr}[i \text{ and } j \text{ are compared}] \\
 & = \sum\limits_{i=1}^{n} \sum\limits_{j=i + 1}^{n} \frac{2}{j-i+1} \\
 & = \sum\limits_{i=1}^{n} \sum\limits_{j'=1}^{n-i} \frac{2}{j' + 1} && \text{(change of index } j' = j - i) \\
 & = 2 \sum\limits_{i=1}^{n}  \sum\limits_{j'=1}^{n - i}  \frac{1}{j' + 1}
\end{align}
$$

- % We change the bound from $n$ to $n - i$
    - Result when $n$ witnesses $j$ in $j - i$
- Individual terms of inner summation do not depend on $i$
    - Only bound depends on $i$
- First term $j' = 1$ occurs when $1 \leq i \leq n - 1$
    - $n - 1$ times in total
    - (Literally substitute $i = n -1$ into $n - i$ to see that bound becomes $1$, which is the minimum bound for the inner sum to have at least one term)
        - i.e., Solve $n - i = 1$
- Second term $j' = 2$ occurs when $1 \leq i \leq n - 2$
- In general, $j' = k$ term appears $n - k$ times
    - ![|341](https://i.imgur.com/nYfQ3Kv.png)

$$
\begin{align}
\mathbb{E}[T]  & = 2 \sum\limits_{j'=1}^{n - 1} \frac{n-j'}{j' + 1} \\
 & = 2 \sum\limits_{j'=1}^{n - 1}  \frac{n-j' + 1 - 1}{j' + 1} \\
& = 2 \sum\limits_{j'=1}^{n - 1}  \frac{n+ 1}{j' + 1} + \frac{-j'-1}{j' + 1} \\
& = 2 \sum\limits_{j'=1}^{n - 1}  \frac{n+ 1}{j' + 1} + \frac{-(j'+1)}{j' + 1} \\
 & = 2 \sum\limits_{j'=1}^{n - 1} \left( \frac{{n+1}}{j' + 1} - 1 \right)  \\
 & = 2 \sum\limits_{j'=1}^{n - 1} \left( \frac{{n + 1}}{j' + 1} \right) - 2 \sum\limits_{j'=1}^{n - 1} 1 && \text{(Distributive Rule)} \\
 & = 2(n + 1) \sum\limits_{j'=1}^{n - 1} \frac{1}{j' + 1} - 2(n - 1)
\end{align}
$$

> [!thm]+ Fact. Harmonic Series is $\Theta(\log n)$
> $$
> \sum\limits_{i=1}^{n - 1}  \frac{1}{i+1} = \Theta (\log n)
> $$
> (Harmonic series)

By our fact, then $\mathbb{E}[T] = \Theta(n \log n)$
<div class="right-align">■</div>

## Exercises

![](https://i.imgur.com/jCv62zL.png)
