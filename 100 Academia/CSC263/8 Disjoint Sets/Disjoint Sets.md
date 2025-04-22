---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 11
Module:
  - "[[8 - Disjoint Sets]]"
Lecture:
  - "21"
  - "22"
Chapter:
Slides/Notes:
Date: 2025-03-25
Date created: Tue., Mar. 25, 2025, 2:59:20 am
Date modified: Wed., Apr. 16, 2025, 4:54:24 am
---

# Disjoint Sets

Recall: [[Connected Components and Spanning Trees|Kruskal's algorithm]]

- Led to the desire for a new data type that essentially represents partitions of a set of objects
    - Supports membership tests
        - Which partition has this element?
    - Merging partitions
- & Formalize this in the **Disjoint Set ADT**
- **Disjoint**
    - Every element appears in exactly one set

## Disjoint Set ADT

> [!def]+ Disjoint Sets
>
> > [!summary]+ Objects
> >
> > - A collection of non-empty **disjoint sets** $DS = \{ S_{1}, S_{2}, \dots, S_{k} \}$
> > - Each set is identified by a *unique* element called its **representative**
> >
> > > [!note]+ Disjoint
> > >
> > > - Two sets are **disjoint** iff $S_{i} \cap S_{j} = \emptyset$
> > > - That is, for each pair $S_{i}, S_{ij} \in DS (1 \leq i, j \leq k)$, we have $S_{i} \cap S_{j} = \emptyset$
>
> > [!summary]+ Operations
> >
> > - `Make-Set(DS, v)`
> >     - Given element $v$ that does not already belong to one of the sets, create a new set $\{ v \}$
> >     - The new item is the **representative** element of the new set
> > - `Find-Set(DS, x)`
> >     - Return the **representative** element of the (unique) set containing $x$
> >     - % We know the set contains $x$; we just need to find its **representative**
> > - `Union(DS, x, y)`
> >     - Given two items $x$ and $y$, merge the sets that contain these items
> >     - The representative of the new set might be one of the two representatives of the original set, one of $x$ or $y$, or something completely different
> >     - % If both $x$ and $y$ belong to the same set already, operation has no effect

![](https://i.imgur.com/nvOiAAc.png)

### Example

![](https://i.imgur.com/k4CSEj7.png)

$$DS = \{ S_{1}, S_{2}, S_{3} \}$$

- `Find-Set(DS, S)` returns:
    - $B$
- `Find-Set(DS, R)` returns:
    - $P$
- `Make-Set(DS, T)`
    - ![](https://i.imgur.com/sfpes0h.png)

> [!note]+ Note
> Assume we are adding a new, unique thing into our sets when calling `Make-Set`.

- `Union(DS, C, R)`
    - ![](https://i.imgur.com/DmyG3El.png)
    - $DS = \{ S_{3}, S_{4} \}$

> [!note]+ Note
>
> - ADT does not tell us what the new representative is in new union set
> - e.g., can pick anything as representative, so pick $C$

## Kruskal’s Algorithm Pseudocode

```python
MST-Kruskal(G = (V, E), w: E -> R)
    T <- {}
    sort edges so w(e_1) <= w(e_2) <= ... < w(e_m)

    for all v in V:
        Make-Set(v)

    for i <- 1 to m:
        let (u_i, v_i) = (e_i)
        # if u_i, v_i in different connected components of T:
        if Find-Set(u_i) != Find-Set(v_i):
            Union(u_i, v_i)
            T <- T ∪ {e_i}  # Add this edge to T
```

## Implementations

### Circularly-Linked List

> [!thm]+ Circularly-linked lists
>
> - One circularly-linked list for each set
> - *Head* of the linked list is the **representative** of the set

![|400](https://i.imgur.com/ioP6juE.png)

> [!info]+ `Make-Set(DS, v)`
>
> - Create a new linked list with a node $x$ storing element $v$
> - Set $x.next = x$
>
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
>
> - Follow the links until reaching the head node $r$
> - Return $r$
> - Start somewhere, go around, if it has not found the element, go to another set
> - Worst case:
>     - $$\Theta(n)$$
>     - $n$ is the number of elements in DS

> [!info]+ `Union(DS, x, y)`
>
> - Locate the head of each list by calling:
>     - $l_{1} = \texttt{Find-Set(DS, x)}$
>     - $l_{2} = \texttt{Find-Set(DS, y)}$
> - Exchange $l_{1}.next$ and $l_{2}.next$
>
> ![](https://i.imgur.com/GWDSgbi.png)

#### Circularly-Linked Lists: Worst-Case Run Time Analysis

- `Find-Set(DS, x)`
    - $\Theta(L)$ where $L$ is the length of the list containing $x$
- `Union(DS, x, y)`
    - $\Theta(L_{1} + L_{2})$
    - Worst-case: $n = L_{1} + L_{2}$

#### Circularly-Linked Lists: Amortized Run Time Analysis

- Amortized analysis:
    - Find worst possible sequence
    - Find average time by dividing total time by $m$ operations

Consider a *bad* sequence of length $m$:

- $\frac{m}{4}$ `Make-Set` with unique elements
- $\frac{m}{4} - 1$ `Union`
    - Result is one list with $\frac{m}{4}$ elements
- $\frac{m}{2} + 1$ `Find-Set`

<!-- break -->
- Sequence length is $\frac{m}{4} + \left( \frac{m}{4} - 1 \right) + \left( \frac{m}{2} + 1 \right)$
- Time for $\frac{m}{4}$ `Make-Set`s
    - $\frac{m}{4}$ since $\Theta(1)$
- After $\frac{m}{4}$ `Make-Set`s and $\frac{m}{4} - 1$ `Union`s:
    - Size of $\frac{m}{4}$
- For each `Find-Set`, runtime is:
    - $\Theta\left( \frac{m}{4} \right)$
- For $\frac{m}{2} + 1$ `Find-Set`s, runtime is:
    - $$\Theta \left( \left( \frac{m}{2} + 1 \right) \times \left( \frac{m}{4} \right)  \right) \in \Theta(m^{2})$$
- For $\frac{m}{4} - 1$ `Union`s, runtime is:
    - $$\left( 1 + 1 \right) + (2 + 1) + (3 + 1) + \dots$$
    - $$= 2 + 3 + 4 + \dots + \left( \frac{m}{4} - 1 + 1 \right) $$
    - $$= \Theta(m^{2})$$

$$
\begin{align}
T_{m}^{sq} & = \frac{m}{4} + \Theta(m^{2}) + \Theta(m^{2}) \\
 & = \Theta(m^{2}) \\
 & = \Theta(n^{2})
\end{align}
$$

> [!note]+ Note
> $n = \frac{m}{4}$ at the end of these operations.

$$
\frac{T_{m}^{sq}}{m} = \Theta\left( \frac{m^{2}}{m} \right) = \Theta(m) = \Theta(n)
$$

> [!important]+ Amortized cost of every operation is $\Theta(n)$.

### Linked Lists with ==Pointer to Head==

> [!thm]+ Linked Lists with Pointer to Head
>
> - One linked list for each set
> - All nodes in a list have a *pointer to the head* of the list
>     - Except the head node
> - *Head* of the linked list is the **representative** of the set
> - Head node has a pointer to the *tail* of the list
>     - % Cheaper for `Union`
>         - So you do not have to loop through entire list to get to the end

![|500](https://i.imgur.com/WB5mUKs.png)

> [!info]+ `Make-Set(DS, x)`
>
> - Create a new linked list with a node $x$ storing element $v$
>     - $x.rep = x$
>     - $x.tail = x$
>
> Worst-case run time:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
>
> - Return $x.rep$
>
> Worst-case run time:
> $$\Theta(1)$$
> since we have a pointer to the representative.

> [!info]+ `Union(DS, x, y)`
>
> - Append one list to the tail of the other
> - Update the tail pointer
> - Update the pointers to the head
>
> WCRT:
> $$\Theta(\text{max}(L_{1}, L_{2}))$$
>
> - $L_{1}$ is the length of the linked list containing $x$, and
> - $L_{2}$ is the length of the linked list containing $y$
> - % Expensive operation: update pointers to head
>
> ![](https://i.imgur.com/ZeySDjl.png)

#### Linked Lists with Pointer to Head: Amortized Run Time Analysis

- `Union` is expensive
    - Especially if appending a long list
- & A bad sequence includes many `Union`s

Consider a *bad* sequence where you have $\frac{m}{2} + 1$ `Make-Set` with different elements, then $\frac{m}{2} - 1$ `Union`s, always appending longer list tot he end of single-element list.

> [!info]+ Time for $\frac{m}{2} - 1$ `Union`s
> $$
> \begin{align}
> & 1 + 2 + 3 + \dots + \left( \frac{m}{2} - 1 \right)  &  & (\text{taking max}(L_{1}, L_{2})) \\
> & = \frac{\left( \left( \frac{m}{2} - 1 \right) \left( \frac{m}{2} \right) \right)}{2} \\
>  & \in \Theta(m^{2})
> \end{align}
> $$

So,
$$
\begin{align}
T_{m}^{sq} & = \frac{m}{2} + 1 + \Theta(m^{2}) \\
 & = \Theta(m^{2}) \\
\implies \frac{T_{m}^{sq}}{m} & = \Theta(m) \\
 & = \Theta(n)  &  & \left( n = \frac{m}{2} + 1 \right)
\end{align}
$$

> [!obs]+ We saved on `Find-Set`, but it did not help for `Union`.

### Linked Lists with Pointer to Head and ==Union-by-Weight==

> [!thm]+ Linked lists with pointer to head and union-by-weight
>
> - One linked list for each set
> - All nodes in a list, except the head node, have a *pointer to the head* of the list
>     - *Head* of the linked list is the **representative** of the set
> - Head node has a pointer to the *tail* of the list
> - & *Head* node stores the **size** of each list
>     - % `Union` will pick the list with minimum length to update that list’s pointers

![](https://i.imgur.com/w7GzHIl.png)

> [!info]+ `Make-Set(DS, v)`
>
> - Create a new linked list with a node $x$ storing element $v$
> - $x.rep = x$
> - $x.tail = x$
> - $x.size = 1$
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
>
> - Return $x.rep$
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Union(DS, x, y)`
>
> - Append the *shorter* list to the longer list
> - Update the tail pointer
> - Update the size of the new list
> - Update the pointers to the head
>
> WCRT:
> $$\Theta(\text{min}(L_{1}, L_{2}))$$
>
> - % Since we append the smaller list to the end (and update the smaller list’s pointers to head)
>
> ![](https://i.imgur.com/ZrnTmsI.png)

#### Extra Pointer and Union-by-Weight: Amortized Analysis

- Consider a sequence of $m$ operations
- Let $n$ be the number of `Make-Set` operations in the sequence
    - Never more than $n$ elements in total
- For some arbitrary element $x$:
    - @ Want to prove an **upper bound** on the number of times that $x.rep$ can be updated

<!-- break -->
- $x.rep$ gets updated only when the set that contains $x$ is *union-ed* with a set that is ==not smaller==
    - Each time $x.rep$ is updated:
        - & Size of resulting set must be at least *double* the size of the list containing $x$
        - i.e., Every time $x.rep$ is updated, the set size *doubles*
- There are only $n$ elements in total
    - & → Can double at most $\log n$ times
    - & → $x.rep$ cannot be updated more than $\log n$ times

> [!info]+ Total number of head pointer updates
> At most
> $$\mathcal{O}(n\log n)$$

> [!info]+ Total running time for all other operations
> $$\mathcal{O}(m)$$

For a sequence of $\frac{m}{2} + 1$ `Make-Set`s, then $\frac{m}{2} - 1$ `Union`s:

$$
\begin{align}
\implies T_{m}^{sq}  & \in \Theta(n \log n + m) \\
 & \in \Theta(m \log m) \\
 & \in \Theta(n \log n) &  & \left( n = \frac{m}{2} + 1 \right) \\
\frac{T_{m}^{sq}}{m} & = \Theta(\log m) \\
 & = \Theta(\log n)
\end{align}
$$

### Trees

> [!info]+ Trees
>
> - One *inverted* tree for each set
>     - Points towards the root, not away
> - Each element points to its *parent* only
>     - Or to itself if it is the root
> - *Root* is the **representative** of the set
>     - Points to itself
>     - % We know that it is the root because its parent points to itself

> [!note]+ Trees are *not* necessarily binary; number of children of a node can be arbitrary.

![|300](https://i.imgur.com/B07coqr.png)

> [!info]+ `Make-Set(DS, v)`
>
> - Create a tree with a node $x$ storing element $v$
> - $x.p = x$
>
> ![](https://i.imgur.com/dKMDGeZ.png)
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
>
> - Trace up the parent pointer until the root $r$ is reached
> - Return $r$
>
> WCRT:
> $$\Theta(h)$$
>
> - % $h$ is the height of the tree containing $x$
> - In the worst-case, $h = n$, so $\Theta(n)$

> [!info]+ `Union(DS, x, y)`
>
> - Locate the head of each list by calling:
>     - $l_{1} =$ `Find-Set(DS, x)`
>     - $l_{2} =$ `Find-Set(DS, y)`
> - Let one tree’s root point to the other tree’s root:
>     - $l_{1}.p = l_{2}$
>
> WCRT:
> $$\Theta(\underbrace{ h_{1} }_{ \text{to locate first} } + \underbrace{ h_{2} }_{ \text{to locate second} })$$
>
> - % $\Theta(1)$ to change pointers
>
> ![|400](https://i.imgur.com/9ifqypn.png)

#### Trees: Amortized Run Time Analysis

- $ `Find-Set` (and so `Union`) are expensive
- A *bad* sequence creates a tree that is just one long chain with $\frac{m}{4}$ elements

Consider a sequence of $\frac{m}{4}$ `Make-Set`, $\frac{m}{4} - 1$ `Union`s, so that one long chain with $\frac{m}{4}$ elements is created.
Then, $\frac{m}{2} + 1$ `Find-Set`s:

- Time for $\frac{m}{4}$ `Make-Set`s: $\Theta(\frac{m}{4}) = \Theta(m)$ since each takes $\Theta(1)$
- For $\frac{m}{4} - 1$ `Union`s, each costs $\Theta(h_1 + h_2)$:
    - After $i$ `Union`s, height can be $i+1$
    - Total cost: $\Theta((1+1) + (1+2) + (1+3) + … + \frac{m}{4} - 1 + 1) = \Theta(m^2)$
- After the `Union` operations, we have a chain of length $\frac{m}{4}$
- For the `Find-Set` operations:
    - The $i$-th element from the bottom needs $i$ steps to reach the root
    - For $\frac{m}{2} + 1$ `Find-Set`s on the chain, the worst case is:
    $$\Theta\left(\left(\frac{m}{2} + 1\right) \times \frac{m}{4}\right) = \Theta(m^2)$$

Therefore:
$$
\begin{align}
T_{m}^{sq} &= \Theta(m) + \Theta(m^2) + \Theta(m^2) \\
&= \Theta(m^2) \\
\end{align}
$$

The amortized cost per operation is:
$$
\frac{T_{m}^{sq}}{m} = \Theta\left(\frac{m^2}{m}\right) = \Theta(m)
$$

### Trees with ==Union-by-Weight==

> [!idea]+ Intuition
> - `Find-Set` takes $\Theta(h)$
>     - $h$ is the height of the tree
> - Append *smaller* tree to larger one during `Union` to keep the union-ed tree’s height small
>     - % Note that when we append smaller tree to larger tree, the height of the union-ed tree is $\text{max}(h_{L}, h_{S} + 1)$
>         - $h_{L}$ is the height of the larger tree
>         - $h_{S}$ is the height of the smaller tree

> [!thm]+ Trees with Union-by-Weight
> - One inverted tree for each set
> - Each element points to its parent only
> - *Root* of the tree is the **representative** of the set and points to itself
> - & *Root* stores the **weight** of the tree
>     - i.e., the number of elements

> [!info]+ `Make-Set(DS, v)`
> - Create a tree with a node $x$ storing element $v$
> - $x.weight = 1$
> - $x.p = x$
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
> - Trace up the parent pointer until the root $r$ is reached
> - Return $r$
>
> WCRT:
> $$\Theta(h)$$

> [!info]+ `Union(DS, x, y)`
> - Locate the head of each list by calling:
>     - $l_{1} =$ `Find-Set(DS, x)`
>     - $l_{2} =$ `Find-Set(DS, y)`
> - & Let the root with the lower weight point to the root with the higher weight
> - If two roots have the same weight, choose either root as the new root
>     - $weight = weight_{1} + weight_{2}$
>
> WCRT:
> $$\Theta(h_{1} + h_{2})$$
>
> ![](https://i.imgur.com/zUN7B9z.png)

#### Trees with Union-by-Weight: Amortized Run Time Analysis

> [!thm]+ Claim
> Let $T$ be a tree generated by a series of `Make-Set` and `Union` operations using the union-by-weight heuristic.
> Let $r$ be the weight of $T$ and $n$ be the number of nodes in $T$.
> Then, $2^{r} \leq n \implies r \leq \log n$

Consider a sequence of $m$ operations: $\frac{m}{4}$ `Make-Set`, then $\frac{m}{4} - 1$ `Union` and $\frac{m}{2} + 1$ `Find-Set`:

Each operation takes at most $\log n$.

$$
\begin{align}
\implies T_{m}^{sq} & \in \Theta(m\log n) \\
\implies \frac{T_{m}^{sq}}{m} & = \Theta(\log m) \\
 & = \Theta(\log n)
\end{align}
$$

### Trees with ==Path-Compression==

> [!idea]+ Idea
> - When calling `Find-Set(DS, x)` for some $x$:
>     - & Keep track of nodes visited on path from $x$ to the root
>         - Use stack, queue, or write `Find-Set` recursively
> - Once root is found:
>     - & Make each visited node to point *directly* to the root
> - ^ Can speed up *future* operations considerably

![](https://i.imgur.com/5qG0CCK.png)

> [!info]+ `Make-Set(DS, v)`
> - Create a tree with a node $x$ storing element $v$
> - $x.p = x$
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
> - Trace up the parent pointer until the root $r$ is reached
> - Once the root is found:
>     - Make each visited node point directly to the root
> - Return $r$
>
> WCRT:
> From $\Theta(n)$ to $\Theta(1)$

> [!info]+ `Union(DS, x, y)`
> - Locate the head of each list by calling:
>     - $l_{1} =$ `Find-Set(DS, x)`
>     - $l_{2} =$ `Find-Set(DS, y)`
> - Let one tree’s root point to the other tree’s root
>
> WCRT:
> $$\Theta(h_{1} + h_{2})$$

#### Trees with Path-Compression: Amortized Run Time Analysis

For a sequence of $n$ `Make-Set` (and hence at most $n - 1$ `Union`s), and $f$ `Find-Set`s:

$$
T_{m}^{sq} \begin{cases}
 & \in \Theta\left(\frac{f\log n}{\log\left( 1 + \frac{f}{n} \right)} \right) && \text{if } f\geq n, \\
 & \in \Theta(n + f\log n) && \text{if } f < n
\end{cases}
$$

### Trees with ==Union-by-Rank== and Path-Compression

> [!idea]+ Idea
> - & Path compression happens in `Find-Set`
> - & Union-by-rank happens in `Union`
>     - Outside `Find-Set`

> [!danger]+ Small issue
> - Path compression does *not* maintain height info
>     - Updating ranks would be *too expensive*
>
> > [!check]+ Solution
> > - A node’s **rank**
> >     - *Upper-bound* on its height
> > - % Can be proved that comparing ranks (and not the heights) does *not* affect the efficiency of the algorithm considerably

> [!info]+ `Make-Set(DS, v)`
> - Create a tree with a node $x$ storing element $v$
> - $x.rank = 0$
> - $x.p = x$
>
> WCRT:
> $$\Theta(1)$$

> [!info]+ `Find-Set(DS, x)`
> - Trace up the parent pointer until the root $r$ is reached
> - Once the root $r$ is found:
>     - Make each visited node point directly to $r$
> - Return r
>
> WCRT:
> From $\Theta(n)$ to $\Theta(1)$

> [!info]+ `Union(DS, x, y)`
> - Locate the head of each list by calling:
>     - $l_{1} =$ `Find-Set(DS, x)`
>     - $l_{2} =$ `Find-Set(DS, y)`
> - Let the root with lower rank point to the root with higher rank
> - If two roots have the same rank, choose either root are the new root
>     - Increment the rank of the new root by *one*

![](https://i.imgur.com/bnuC0g7.png)

> [!note]+ Complete implementation is in Section 21.3 of CLRS

#### Union-by-Rank and Path-Compression: Amortized Analysis

For a sequence of $m$ operations with $n$ Make-Set (so at most $n - 1$ Unions):

$$T_{m}^{sq} \in \mathcal{O}(m \times \alpha(n)) \approx \Theta(m \times \log ^{*}n)$$

where $\alpha(n)$ is the **inverse Ackerman function**.

- Inverse Ackerman function $\alpha(n)$
    - Grows really, really, really slowly
    - Can basically treat it as **constant**

So,
$$T_{m}^{sq} \in \mathcal{O}(m)$$

> [!note]+ Inverse Ackerman function
> $$
> \begin{align}
> \alpha\left(10^{80}\right) & = 4 \\
> \alpha \left( 2^{65536} \right) & = 5
> \end{align}
> $$
