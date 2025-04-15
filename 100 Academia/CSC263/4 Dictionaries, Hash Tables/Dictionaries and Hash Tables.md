---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Hashing]
Week: 6
Module:
  - "[[4 - Dictionaries, Hash Tables]]"
Lecture:
  - "11"
  - "12"
Chapter: 
Slides/Notes: 
Date: 2025-02-11
Date created: Tue., Feb. 11, 2025, 12:44:37 am
Date modified: Thu., Mar. 6, 2025, 9:30:14 pm
---

# Dictionaries and Hash Tables

## Discover Module

From [Quercus](https://q.utoronto.ca/courses/379913/pages/discover-hashing-2?module_item_id=6330526).

> [!example]+ Student numbers
> - A student number has nine digits. That gives us 1,000,000,000 possible different student numbers.
> - If we used **direct access** approach i.e., an array of size 1,000,000,000 for each student number, and there are only 2000 students, the remaining $10^{9}-2000$ students would be junk entries.
>     - → 99.9998% of the allocated space does not hold real data
>     - A lot of wasted space!

Consider using the last four digits, which usually uniquely identify a student.

- Suggests that maybe we can use the last four digits as an array index

| Name     | Student ID ($k$) | Index $h(k)$ |
| -------- | ---------------- | ------------ |
| Michelle | 987654321        | 4321         |
| George   | 564722842        | 2842         |
| Noah     | 432583925        | 3925         |
| Samar    | 335462095        | 2095         |
| Sam      | 234099837        | 9837         |
| Danny    | 333526321        | 6321         |

- Making an array with 10,000 entries and then using the last four digits of student number as the **key** for each student would be the same as applying this equation:
    - $\text{index}_{i} = \texttt{student\_number} \text{ mod } 10000$

<!-- break -->
- Let $k$ be the **key** for the **record** we want to store
    - i.e., Student number
- Let $h(k)$ be a function of the index at which we will store the record
    - $h(k) = k \text{ mod } 10000$

<!-- break -->
- **Hash function**
    - $h(k)$
- **Hash table**
    - Our array
- **Slots**
    - Elements in the hash table

> [!def] A hash function *maps* from the universe of all possible **keys** into **slots** in the **hash table**.

> [!thm]+ Division method
> Let $m$ represent the number of slots in our hash table.
>
> - Hash function $h(k) = k \text{ mod } m$ is a common choice
>     - Known as the **division method**

- Approach works fine with our example with six students
    - Each student maps to a unique slot in hash table
    - Only use 6 of 10,000 slots
        - → 99.94% of array will be unused
        - Still a lot of wasted space, but much better than earlier
- ? What if we make the table smaller, with 100 slots?
    - $h(k) = k \text{ mod } 100$
    - → Michelle and Danny hash to slot 21
        - Only space for one record in slot 21 in array!

<!-- break -->
- **Collision**
    - When two or more keys map to the same slot

## Dictionaries

Recall from [[Dictionaries and AVL Trees#Dictionary ADT]]:

> [!def]+ Dictionary ADT
> - Objects:
>     - A collection of **key-value pairs**
>     - Keys are unique
> - Operations:
>     - `Search(D, k)`
>         - Return $x$ in $D$ s.t. $x.key = k$, or NIL if no such $x$ is in $D$
>     - `Insert(D, x)`
>         - Insert $x$ in $D$
>         - If some $y$ in $D$ has $y.key$ equal to $x.key$, replace $y$ by $x$
>     - `Delete(D, x)`
>         - Remove $x$ in $D$

## Hash Tables

### Motivation

> [!example]+ Read a grade file, where grades are integers between 0 to 99. Keep track of number of occurrences of each grade.
>
> > [!check]+ Fastest solution
> > - Create an array $T$ of size $100$
> > - $T[i]$ stores the number of occurrences of grade $i$

> [!example]+ Read a data file, keep track of number of occurrences of each integer value (from 0 to $2^{32} - 1$).
>
> > [!check]+ Fastest solution
> > - Create array of size $2^{32}$
> > - ! Wasteful use of memory, especially when data are files relatively small

> [!example]+ Read a text file, keep track of number of occurrences of each word
> - ! Cannot use *keys as indices* anymore!

> [!goal]+ Goal: Hash functions
> 1. Be able to convert *any type* of **key** to an integer
> 2. Map the universe of keys into a small number of slots
>
> **Hash function** can do both!

### Definitions

> [!def]+ Universe $\mathcal{U}$
> - Set of all possible keys

> [!def]+ Hash function
> - A function from the set of all possible keys to integers between 0 and $m - 1$
> - $h : \mathcal{U} \to \{ 0, 1, \dots, m - 1 \}$

> [!def]+ Hash table
> - A **data structure** containing an **array of length $m$** and a **hash function $h : \mathcal{U} \to \{ 0, 1, \dots, m-1 \}$**
>     - $h(k)$ maps a key $k$ to one of the $m$ positions in hash table $T$
>         - i.e., $h(k)$ is the **index** at which the key $k$ is stored
> - Each array location is called a **slot** or **bucket**

![|333x315](https://i.imgur.com/yyWQvn7.png)

### Collisions

> [!thm]+ If $m \geq |\mathcal{U}|$, then there exists a hash function $h$ which maps each key to a unique slot.
> - No collisions
> - Such a function is called a **perfect hash function**
> - Typically, the number of possible keys is *much bigger* than the number of array slots

> [!thm] If $m < |\mathcal{U}|$, then at least one **collision** occurs.

> [!info]+ How do we handle collisions?
> - **Chaining** (Closed Addressing)
> - **Open Addressing**

### Chaining

> [!def]+ Chaining
> - Each *bucket* in the array points to a **linked list** of key-value pairs

![|708x438](https://i.imgur.com/hQBprom.png)

#### Implementing `HashInsert(k, v)`

1. Compute $h(k)$. Set $i = h(k)$
2. *Search* the linked list stored at $T[i]$ to check whether an element with key $k$ already exists
3. If so, replace existing value with $v$. Otherwise, insert new node to ==head== of list

![|708x278](https://i.imgur.com/rkb63U0.png)

#### Implementing `HashSearch(k)`

1. Compute $h(k)$. Set $i = h(k)$
2. Access index $i$ in the table
3. Search the linked list stored at $T[i]$


![|491x365](https://i.imgur.com/PsCX1tt.png)

- `HashSearch(flow)`:
    - $h(flow) = 0$ → Look in $T[0]$ ✅
- `HashSearch(hey)`:
    - $h(hey) = 2$ → Look in $T[2]$ → Not found! ❌

#### Implementing `HashDelete(k)`

1. Compute $h(k)$. Set $i = h(k)$.
2. Search linked list stored at $T[i]$
3. If an element with key $k$ is found, delete it from the list

![](https://i.imgur.com/t61kBLG.png)

#### Worst-Case Running Time

- ? What is the worst case for `Search`?
    - All elements in the table are hashed into the same bucket

Let $n$ be the total number of elements stored in the hash table.

> [!info]+ Worst-case running time
> - `HashSearch`: $\Theta(n)$
> - `HashInsert`: $+ \Theta(1)$
> - `HashDelete`: $+ \Theta(1)$
> - % Both `HashInsert` and `HashDelete` times are in addition to *searching* first
>     - Times are $\Theta(n)$ if we count search as part of `Insert` and `Delete`

> [!note]+ We assume that hash value $h(k)$ is computed in constant time.
> - i.e., Run time for computing $h(k)$ is $\Theta(1)$

- Hash tables work well in real-world applications
    - Worst case almost never happens
    - Average case performance is really efficient

#### Average-Case Running Time

> [!info]+ Recall: Analyzing Average-Case Running Time
> - Define a random variable $t_{n}(x)$ denoting the number of *steps* executed by algorithm on an input $x$ with size $n$
> - Compute $\mathbb{E}[t_{n}]$

- See [[Introduction and Analyzing Running Time (Course Notes)]] for more.

<!-- break -->
- Let $t_{m, n}(k)$ denote the number of steps executed by `HashSearch` to find $k$ in a hash table containing $m$ buckets and storing $n$ elements
- Assume we are equally likely to hash into any bucket.

> [!def]+ Simple Uniform Hashing Assumption
> - Any key is equally likely to hash to any bucket

$$
\mathbb{E}[t_{m, n}(k)] = \underbrace{ 1 }_{ \text{Time to hash } k } + \text{Expected runtime of searching for } k \text{ in } L_{i}
$$

![|264x183](https://i.imgur.com/UeEpE3m.png)

##### Expected Run Time in an *Unsuccessful* Search (under SUHA)

- ? Probability that key $k$ is hashed to a bucket $i$ i.e., $\text{Pr}[h(k) = i]$
    - $m$ candidate positions for $k$
    - $k$ is equally likely to hash to any of them
    - $$\text{Pr}[h(k) = i] = \frac{1}{m}$$
- ? Expected length of the linked list stored at bucket $i$
    - Expect an equal number of elements in each bucket
    - $$\mathbb{E}[len_{i}] = \frac{n}{m}$$
    - where $len_{i}$ denotes the length of the linked list stored in bucket $i$

$$
\mathbb{E}[t_{m, n}(k)] = 1 + \mathbb{E}[len_{i}] = 1 + \frac{n}{m}
$$

##### Load Factor

> [!def]+ **Load factor** of a hash table $T$
> - Ratio of number of keys — denoted by $n$ — stored in $T$ to the number of buckets of $T$ — denoted by $m$
> $$\alpha = \frac{n}{m}$$

- If $\frac{n}{m}$ is $\Theta(1)$, then unsuccessful search is also $\Theta(1)$
- If $\frac{n}{m}$ is $\Theta(n)$, then unsuccessful search is also $\Theta(n)$

##### Expected Run Time in a *Successful* Search (under SUHA)

- That is, $k$ is a key that *exists* in the hash table

Let $k_{1}, k_{2}, \dots, k_{n}$ be the **order of insertion** into the hash table.

- $k$ could be $k_{1}$, or $k_{2}$, or $\dots$, or $k_{n}$
- ? Probability that $k = k_{i}$ for $1 \leq i \leq n$
    - $$\frac{1}{n}$$


Let $S_{i}$ denote the ==expected number of steps== to find $k_{i}$.

- ? Expected number of steps to find $k$
    - $$\begin{align}\mathbb{E}[t_{m, n}(k)]  & = \frac{1}{n} S_{1} + \frac{1}{n} S_{2} + \dots + \frac{1}{n} S_{n} \\ & = \frac{1}{n} \sum\limits_{i=1}^{n} S_{i} \end{align}$$

> [!obs]+ The run time of search is the number of keys that were inserted after $k$ that hash to the *same* bucket.
>
> $$
> \begin{align}
> S_{i} & = \text{Expected number of steps to find } k_{i} \\
>  & = \text{Number of elements examined during search for } k_{i} \\
>  & = 1 + \text{ Number of elements BEFORE }k_{i}\text{ in the linked list stored at }h(k_{i}) \\
>  & = 1 + \text{Number of keys that hash SAME AS }k_{i}\text{ and are inserted AFTER }k_{i}
> \end{align}
> $$

Let $X_{i}$ be the expected number of keys that has the same as $k_{i}$.
Define indicator random variables for $X_{i}$:
$$
X_{i, j} = \begin{cases}
1 & \text{if }h(k_{i}) = h(k_{j}) \\
0 & \text{otherwise}
\end{cases}
$$
Then,
$$
\begin{align}
\mathbb{E}[X_{i, j}]  & = \text{Pr}[X_{i, j}]  &  &  (\text{Linearity of expectation}) \\
 & = \frac{1}{m}  &  & (\text{since we have } m \text{ buckets, and each key is equally likely to be in a bucket by SUHA})
\end{align}
$$
$$
\begin{align}
S_{i}  & = 1 + \text{ All keys inserted after }k_{i}\text{ that hash to same place as }k_{i} \\
 & = 1 + \sum\limits_{j=i + 1}^{n} E[X_{i, j}] \\
 & = 1 + \sum\limits_{j=i + 1}^{n} \frac{1}{m}
\end{align}
$$
$$
\begin{align}
\mathbb{E}[t_{m, n}(k)]  & = \frac{1}{n} \sum\limits_{i=1}^{n} S_{i} \\
 & = \frac{1}{n} \sum\limits_{i=1}^{n} \left( 1 + \sum\limits_{j=i + 1}^{n} \frac{1}{m} \right)  \\
 & = \frac{1}{n} \sum_{i = 1}^{n} \left( 1 + \frac{n-i}{m} \right)  \\
 & = \frac{1}{n} \left( n + \frac{1}{m} \sum\limits_{i=1}^{n} n - i\right) \\
 & = \frac{1}{n} \left( n + \frac{1}{m} \sum\limits_{j=0}^{n-1} j \right) &  &  (\text{Let }j = n - i) \\
 & = \frac{1}{n} \left( n + \frac{1}{m} \frac{n(n-1)}{2} \right)  \\
 & = \frac{1}{n} \left( n + \frac{n(n-1)}{2m} \right)  \\
 & = 1 + \frac{n-1}{2m} \\
 & = 1 + \frac{n}{2m} - \frac{1}{2m} \\
 & = 1 + \frac{\alpha}{2} - \underbrace{ \frac{\alpha}{2n} }_{ \text{goes to 0 as }n \to \infty } \\
 & \in \Theta(1 + \alpha)
\end{align}
$$

##### Summary

> [!info]+ Average-case Running Time
> - `HashSearch`
>     - $\Theta(1 + \alpha)$, where $\alpha = \frac{n}{m}$
> - `HashInsert`
>     - Search $+ \Theta(1)$
> - `HashDelete`
>     - Search $+ \Theta(1)$
> - So, `HashInsert` and `HashDelete` are $\Theta(1 + \alpha)$

> [!important]+ If the number of hash-table slots is *at least proportional* to the number of elements in the table, then $\alpha \in \Theta(1)$
> - i.e., All dictionary operations can be implemented with **constant** average run time

> [!note]+ SUHA is also a property of inputs, not just hash functions
> - Maybe we design a great hash function, but maybe we only get the same input over and over again
> - Then, hash function does not satisfy SUHA property
> - → Need to redo hash function

### Open Addressing

> [!def]+ Open addressing
> - Store all items directly in $T$
>     - No chaining
> - If a collision occurred, look for another free spot in some *==systematic manner==* called **probing**

> [!important]+ Implication
> -  The number of keys stored in the hash table cannot exceed the length of the array

#### Insertion

> [!example]+
> - Insert “tree”, where $h(\text{tree}) = 2$
>     - $T[2]$ ✔️
> - Insert “hey”, where $h(\text{hey}) = 3$
>     - $T[3]$ ✔️
> - Insert “hello”, where $h(\text{hello}) = 2$
>     - $T[2]$ ❌
>     - $T[3]$ ❌
>     - $T[4]$ ✔️
>
> ![|304x328](https://i.imgur.com/YIq5KBq.png)

> [!def]+ Probe sequence
> - Sequence of buckets to try

- In the above example, the probe sequence is:
    - $2, 3, 4$

#### Search

- Follow the same probing approach used for insertion
- Search returns `None` when encounters the first bucket that stores `None`

> [!important]+ Implication
> - Searching for an item requires examining more than just one spot

> [!example]+
> ![|191x311](https://i.imgur.com/u2fXhVi.png)
> - Picture also shows `Delete(three)`
>     - Search for `three` first in $T$, then remove it
> - Search for “hey”, where $h(\text{hey}) = 3$
>     - $T[3]$ ❌
>     - $T[4]$ ❌
>     - $T[5]$ ✔️
> - Search for “hello”, where $h(\text{hello}) = 2$
>     - $T[2]$ ❌
>     - $T[3]$ ❌
>     - $T[4]$ ❌
>     - $T[5]$ ❌
>     - $T[6]$ → Not there!
> - Probe sequences
>     - For `hey`: $3, 4, 5$
>     - For `hello`: $2, 3, 4, 5, 6$

#### Linear Probing

> [!def]+ Linear probing
> - Examine a linear sequence of slots
> - Probe sequence:
>     - $\big(h(k) + i \big) \text{ mod } m,$ for an integer $i \geq 0$

- Example probe sequence:
    - $h(k), h(k) + 1, h(k) + 2, h(k) + 3, \dots$

> [!danger]+ Problem
> - Contiguous blocks of occupied locations called **clusters** are created
> - → Further insertions of keys into any of these locations take a long time

#### Quadratic Probing

> [!def]+ Quadratic probing
> - Examine a non-linear sequence of slots
> - Probe sequence:
>     - $\big( h(k) + c_{1} \times i + c_{2} \times i^{2} \big) \text{ mod } m,$ for an integer $i \geq 0$

- Example probe sequence:
    - $h(k), h(k) + 2, h(k) + 6, h(k) + 12$
        - $c_{1} = 1, c_{2} = 1$

##### Example

![|250x391](https://i.imgur.com/g9b2IQk.png)

Let $c_{1}, c_{2} = 1$.

> [!example]+ Insert “hello,” where $h(\text{hello}) = 2$
> - Try $i = 0$
>     - [c] $\implies T[2]$
> - Try $i = 1$
>     - $\implies \Big(h(\text{hello}) + c_{1} \cdot 1 + c_{2} \cdot 1^{2} \Big) \text{ mod } 7 = 2 + 1 + 1 = 4$
>     - [c] $T[4]$
> - Try $i = 2$
>     - $\implies \Big(h(\text{hello}) + c_{1} \cdot 2 + c_{2} \cdot 2^{2} \Big) \text{ mod } 7 = (2 + 2 + 4) \text{ mod } 7 = 1$
>     - [p] $T[1]$
>
> ![|200](https://i.imgur.com/J8GAZvM.png)
>
> > [!note]+ Probe sequence
> > $$2, 4, 1$$

##### Problems with Quadratic Probing

> [!danger]+ Problems
> - Collisions still cause a *milder* form of **clustering**
> - ! Need to be careful with the values of $c_{1}, c_{2}$
>     - Could jump in such a way that some of the buckets are never *reachable*

#### Double Hashing

> [!def]+ Double hashing
> - Use a *second* hash function to generate step values that depends on the key
> - Probe sequence:
>     - $\big( h_{1}(k) + i \times h_{2}(k) \big) \text{ mod } m$ for an integer $i \geq 0$

- Example probe sequence:
    - $\underbrace{ h_{1}(k) }_{ i = 0 },\; \underbrace{ h_{1}(k) + h_{2}(k) }_{ i = 1 },\; \underbrace{ h_{1}(k) + 2 h_{2}(k) }_{ i = 2 },\; \underbrace{ h_{1}(k) + 3h_{2}(k) }_{ i = 3 }$

##### Example

![|200](https://i.imgur.com/My6rN6P.png)

> [!example]+ Insert “hello,” where $h_{1}(\text{hello}) = 2$ and $h_{2}(\text{hello}) = 3$
>
> - $i = 0$
>     - $h_{1}(\text{hello}) = 2$
>     - [c] $T[2]$
> - $i = 1$
>     - $2 + 3 = 5$
>     - [c] $T[5]$
> - $i = 2$
>     - $(2 + 2 \cdot 3) \text{ mod } 7 = 1$
>     - [p] $T[1]$
>
> ![|200x258](https://i.imgur.com/RiS9tNZ.png)
>
> > [!note]+ Probe sequence
> > $$2, 5, 1$$

#### Open Addressing: Running Time

Under **Simple Uniform Hashing Assumption** (SUHA):

> [!summary]+ Running times
> - Average-case number of probes in an *unsuccessful* search:
>     - $$\frac{1}{1-\alpha}$$
> - Average-case number of probes in an *successful* search:
>     - $$\frac{1}{\alpha} \ln \left( \frac{1}{1-\alpha} \right) $$

> [!question]- What is the worst case?
> $$\Theta(n)$$
> - Maximum number of probes required is determined by the size of the hash table, $m$
> $$\alpha = \frac{n}{m} \implies m = \frac{n}{\alpha}$$
> - $\alpha$ is a constant $< 1$, so $$\Theta(m) = \Theta(n)$$
> - Probing all $m$ spots is equivalent to $\Theta(n)$
>
> - Unsuccessful search:
>     - Worst case is when probe checks all $m - 1$ spots before reaching the single empty slot
>     - Requires $\Theta(m) = \Theta(n)$ probes
> - Successful search:
>     - Worst case arises when the target key is the last in its probe sequence
>     - Traverses $\Theta(n)$ slots

> [!note]+ In *practice*, open addressing works best when $\alpha < 0.5$.
> $$m \gg n$$
> Suppose $\alpha = \frac{1}{4}$.
> In an unsuccessful search, the average number of probes is $$\frac{1}{1-\alpha} = \frac{1}{3 / 4} = \frac{4}{3}$$

##### Proof from CLRS

> [!thm]+ Theorem 11.6
> Given an open-address hash table with load factor $\alpha = \frac{n}{m} < 1$, the expected number of probes in an *unsuccessful* search is at most $$\frac{1}{(1-\alpha)},$$
> assuming independent uniform permutation hashing and no deletions.

> [!info]- Proof
> ![|1084x911](https://i.imgur.com/WpVDtoI.png)
> ![](https://i.imgur.com/idmdKCb.png)
> ![](https://i.imgur.com/aXP1r7e.png)

> [!thm]+ Corallary 11.7
> Inserting an element into an open-address hash table with load factor $\alpha$ where $\alpha < 1$, requires at most $\frac{1}{1-\alpha}$ probes on average, assuming independent uniform permutation hashing and no deletions.

> [!info]- Proof
> An element is inserted only if there is room in the table. Thus $\alpha < 1$.
> Inserting a key requires an unsuccessful search followed by placing the key into the first empty slot found.
> Thus, the expected number of probes is at most $\frac{1}{1-\alpha}$.
> <div class="right-align">■</div>

> [!thm]+ Theorem 11.8
> Given an open-address hash table with load factor $\alpha < 1$, the expected number of probes in a successful search is at most
> $$\frac{1}{\alpha} \ln \left( \frac{1}{1-\alpha} \right) $$
> assuming independent uniform permutation hashing with no deletions and assuming that each key in the table is equally likely to be searched for.

> [!info]- Proof
> A search for a key $k$ reproduces the same probe sequence as when the element with $k$ was inserted.
> If $k$ was the $(i + 1)$st key inserted into the hash table, then the load factor at the time it was inserted was $\frac{i}{m}$, and so by Corrally 11.7, the expected number of probes made in a search for $k$ is at most ${1} / {\left( 1-\frac{i}{m} \right)} = m / (m - i)$.
> Averaging over all $n$ keys in the hash table gives us the expected number of probes in a successful search:
>
> ![](https://i.imgur.com/wtkuD4b.png)

### Hash Functions

> [!info]+ A *good* hash function should:
> - Ensure **simple uniform hashing**
> - $h(x)$ should depend on every part/bit of $x$
>     - Even for complex objects
> - Should spread out values
> - Should be possible to be computed in constant time i.e., $\Theta(1)$

- % In practice, it is difficult to get all these properties, but
    - There are some **heuristics** that work well

> [!note]+ Important note
> - In quizzes and tests, you can assume that a good hash function exists
>     - Unless question explicitly says otherwise

#### Ideas for Implementing Hash Functions

> [!idea]+ Intuition
> - Interpreting keys as integers

- Every object stored in a computer can be represented by a bit-string
    - Strings of 0‘s and 1’s
- Bit-string can be considered as base 2 representation of a non-negative integer
    - i.e., any type of key can be converted to an integer
- However, integer value is larger than the number of buckets

> [!important]+ A hash function needs to *map* larger integers to a small set of integers $\{ 0, 1, \dots, m-1 \}$
> - Set contains the indices of the buckets

### Implementing Hash Functions

#### Division Method

> [!def]+ Division method
> $$h(k) = k \text{ mod } m$$

- Pitfall:
    - ! Sensitive to the value of $m$
- Examples of this pitfall:
    - $k \text{ mod } 10$ depends only on the last decimal digit of $k$
        - $1782302\bbox[ gray ]{ 5 }$
    - $k \text{ mod } 8$ depends only on the last 3 bits of $k$
        - $10110011110101111000\bbox[ gray ]{ 101 }$
    - $k \text{ mod } 2^{p}$ depends only on the last $p$ bits of $k$

> [!important]+ Implication
> - Keys are not spread out

> [!info]+ What is a good choice for $m$?
> - A prime not too close to an exact power of 2
> - i.e., Size of hash table should be a prime
> - ! Pitfall: This constrains the table size

#### Multiplication Method

> [!def]+ Multiplication method
> 1. Multiply $k$ by a real constant $0 < A < 1$
>     - Mess up $k$ by multiplying by $A$
> 2. Let $x$ be the fractional part of $k \times A$
>     - Then $0 < x < 1$ since we take the fractional part of the mess
> 3. $h(k) = \lfloor m \times x \rfloor$
>     - Multiply by $m$ to make sure the result is in-between $0$ and $m - 1$

- Example:
    - Let $k = 3178825$
    - Let $A = 0.437$
    - $k \times A = 92413.\bbox[ gray ]{ 5192 }$
    - $x = 0.5192$
    - Let $m = 100$
        - i.e., Size of hash table is $100$
    - $h(k) = \lfloor 100 \times 0.5192 \rfloor = 51$

- The optimal choice for $A$ depends on the characteristics of the data being hashed
- Tends to evenly distribute the hash values
    - Because of the mess-up
- Not sensitive to the value of $m$
    - Unlike division method

## After Lecture

- Exercises in Chapter 4 of the course notes
- Problems 11.1-1, 11.2-1, 11.2-2 in CLRS
- Optional Readings: CLRS Sections 11.1, 11.2, 11.3 (except 11.3.3)

## Pseudocode

### `HashInsert` Using Open Addressing

From CLRS 11.4.

`HashInsert` procedure below assumes that the element sin the hash table $T$ are keys with no satellite information.

- Key $k$ is identical to the element containing key $k$
- Each slot contains either a key or `NIL` (if the slot is empty)
- `HashInsert` takes as input:
    - Hash table $T$
    - Key $k$ that is assumed to not already be present in the hash table
- `HashInsert` returns:
    - Slot number where it stores $k$, or
    - Flags an error because hash table is already full

```python
HashInsert(T, k):
    i = 0
    repeat
        q = h(k, i)
        if T[q] == NIL
            T[q] = k
            return q
        else i = i + 1
    until i == m
    error "hash table overflow"
```

> [!note]- David Liu’s notes
>
> ```python
> def Insert(hash_table, key, value):
>     i = 0
>     while hash_table[h(key, i)] is not None:
>         i = i + 1
>     hash_table[h(key, i)].key = key
>     hash_table[h(key, i)].value = value
> ```
> - For simplicity:
>     - Omitted bounds check $i < m$
>     - Necessary in practice to ensure that code does not enter into an infinite loop

### `HashSearch`

- Inputs:
    - Hash table $T$
    - Key $k$
- Returns:
    - $q$ if it finds that slot $q$ contains key $k$, or
    - `NIL` if key $k$ is not present in table $T$

```python
HashSearch(T, k):
    i = 0
    repeat
        q = h(k, i)
        if T[q] == k
            return q
        i = i + 1
    until T[q] == NIL or i == m
    return NIL
```

- Algorithm for searching for key $k$ probes the same sequence of slots that insertion algorithm examined when key $k$ was inserted
    - Search can terminate (unsuccessfully) when it finds an empty slot
        - Since $k$ would have been inserted there and not later in its probe sequence

## Notes on `HashDelete`

Open-addressing:

- When you delete a key from slot $q$:
    - ! Mistake to mark slot as empty by storing `NIL` in it
    - Might be unable to retrieve any key $k$ for which slot $q$ was probed and found occupied when $k$ was inserted

> [!success]+ Solution
> - Mark the slot
>     - Store a special value `Deleted` instead of `NIL`

- `HashInsert` treats such a slot as empty
    - Can insert a new key there
- `HashSearch` passes over `Deleted` values while searching
    - Since slots containing `Deleted` were filled when the key being searched for was inserted

> [!important]+ Search times no longer depend on the load factor $\alpha$!
> - For this reason, *chaining* is frequently selected as a collision resolution technique when keys must be deleted
