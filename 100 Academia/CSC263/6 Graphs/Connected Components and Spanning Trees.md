---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Connected components, Disjoint set, Disjoint set ADT, Prim's algorithm, Kruskal's algorithm, Spanning tree, Spanning trees, Strongly connected]
Week: 9
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "17"
  - "18"
Chapter: 
Slides/Notes: 
Date: 2025-03-11
Date created: Tue., Mar. 11, 2025, 1:10:21 pm
Date modified: Wed., Mar. 26, 2025, 12:07:38 am
---

# Strongly Connected Graphs

## Strongly Connected Directed Graph

- **Connected**
    - An *undirected* graph is **connected** if:
        - Every vertex $v$ is reachable from every other vertex
        - For every pair of vertices $\{ u, v \}$, there is a path that goes from $u \leadsto v$ and $v \leadsto u$

> [!def]+ Strongly connected component
> A **strongly connected component** in a *directed* graph $G = (V, E)$ is:
> - A maximal set of vertices $C \subseteq V$ such that for every pair of vertices $u, v \in C$:
>     - $v$ is reachable from $u$, and
>         - $u \leadsto v$
>     - $u$ is reachable from $v$
>         - $v \leadsto u$

> [!def]+ Strongly connected graph
> A directed graph is **strongly connected** if it has only *one* strongly connected component.

![](https://i.imgur.com/erMRm0f.png)

> [!goal]+ How do you write an efficient algorithm given a graph to decide if its strongly connected?
> - Obvious answers seem to be to do a [[Breadth-First Search|BFS]] or [[Depth-First Search|DFS]]

### Algorithm to Test

1. Pick an arbitrary vertex $s$
2. `DFS(G, s)`
    - If $f[s] = 2|V|$, then every vertex is reachable from $s$
        - Finish time will be double the number of nodes
    - @ Need to show that from anywhere, we can get to $s$
3. Make a new graph $G'$
    - For every edge $(u, v)$ in $G$, put $(v, u)$ in $G'$
        - Take every edge in $G$ and turn it around
4. Run `DFS(G', s)`
    - If $f[s] == 2|V|$ in $G'$, then every vertex is reachable from $s$ in $G'$
    - $\implies$ $s$ is reachable from every vertex in $G$

## Minimum Spanning Trees

- **Minimum spanning tree**
    - Minimum: edge weights are minimized
    - Spanning: touches/involves every vertex in $G$
    - Tree: no cycles

> [!question] How do we find an algorithm that gives us a minimum spanning tree?

- Two options:
    - Take a graph and remove expensive edges until we have a MST
        - Will not use this approach
    - & Start with nothing but the vertices, and add edges from graph, making a forest or tree as we go, until we have a MST
        - This algorithm is more efficient

### Greedy MST

```pseudocode
Greedy-MST(G = (V, E), w: E -> R):
    T <- {}  # Invariant: T is a subset of some MST of G
    while T is not a spanning tree:
        find edge e "safe for T"
        T <- T ∪ {e}
```

- **Greedy algorithm**
    - A problem-solving approach that chooses the best option at each step
        - Without considering the future
    - Often used to solve optimization problems
- % Greedy algorithms do not always work, but it does for MST
    - Do not always work because they make local, immediate decisions without considering the global optimal solution
    - Fails when locally optimal $\neq$ globally optimal
    - Algorithm does not backtrack or reconsider past choices, which can be a problem in cases where an earlier suboptimal decision affects the final outcome
- ? How do we know when $T$ is not a spanning tree?
    - $\underbrace{ |T| }_{ \text{set of edges} } < |V| - 1$
        - $n - 1$ edges

> [!info]+ Proof of loop invariant (CLRS 21.1)
> ![](https://i.imgur.com/8f8pVKP.png)

> [!note]+ CLRS uses slightly different notation in `Greedy-MST` than the one provided in lecture notes.
> ![](https://i.imgur.com/Sw7YB2P.png)

### Theorem: “Safe for $T$”

- “Safe for $T$”
    - We are sure that edge is in the MST

> [!thm]+ Theorem
> If:
> 1. $G$ is a connected, undirected, weighted graph,
> 2. $T$ is a subset of some MST of $G$, and
> 3. $e$ is an edge of minimum weight whose end points are in different connected components of $T$
>
> Then:
> - $e$ is *safe* for $T$

![|400](https://i.imgur.com/if5Ldhz.png)

- Two different connected components of $T$
    - In green and red boxes
- $e$ is the edge of minimal weight out of the yellow, highlighted edges

## Prim’s Algorithm

> [!idea]+
> - Pick a *root vertex*, and
> - Grow $T$ by connecting an isolated vertex to $T$
> - At each step:
>     - Connect the vertex not currently in $T$ that has the *cheapest* edge connecting into $T$

![|400](https://i.imgur.com/57gPgRB.png)

- Start at vertex $r$
- Look at all edges and pick the cheapest edge
    - Has to $2.5$
- Repeat for new node $p$ in $T$
    - Pick $\{ p, s \}$
- Pick $\{ q, r \}$
- Pick $\{ q, t \}$
- Can stop now that we have 4 edges ($n - 1$ edges)

> [!tldr]+ CLRS Section 21.2
> - % CLRS uses a set $A$ instead of $T$ in `Greedy-MST`
>     - See [[#Greedy MST]]
> - Prim’s algorithm has the property that the edges in the set $A$ always form a single tree
> - Tree starts from an arbitrary root vertex $r$, and
> - Grows until it spans all the vertices in $V$
> - Each step adds to the tree $A$ a *light* edge that connects $A$ to an isolated vertex
>     - One on which no edge of $A$ is incident
>     - → Rule adds only edges that are safe for $A$
>     - → When algorithm terminates, edges in $A$ form a MST

### Prim’s Algorithm: Examples

![](https://i.imgur.com/vehmAyO.png)

![](https://i.imgur.com/EXl0Eu9.png)

### Pseudocode

- Inputs to the algorithm:
    - Connected graph $G$
    - Root $r$ of the MST to be grown
    - A function $w: E \to \mathbb{R}$ that assigns weights to edges
- & Algorithm maintains a **[[Priority Queue and Heaps|min-priority heap]]** $Q$ of all vertices that are *not* in the tree
    - Based on a $key$ attribute
    - Attribute $v.key$
        - The *minimum weight of any edge connecting $v$ to a vertex in the tree* for each vertex $v$
        - $v.key = \infty$ if there is no such edge
- Attribute $v.\pi$
    - Names the parent of $v$ in the tree

<!-- break -->
- Algorithm implicit maintains set $A$ from `Generic-MST` as:
    - $$A = \{ (v, v.\pi) : v \in V - \{ r \} - Q \}$$
    - Interpret vertices in $Q$ as forming a set
- When algorithm terminates:
    - Min-priority queue $Q$ is empty
    - Minimum spanning tree $A$ for $G$ is $$A = \{ (v, v.\pi) : v \in V - \{ r \} \}$$

![](https://i.imgur.com/IbjSIQ8.jpeg)

A slightly cleaner implementation from CLRS 21.2:

```pseudocode
MST-Prim(G = (V, E), w: E -> R, r):
    for each vertex u ∈ G.V
        u.key = ∞
        u.π = NIL
    
    r.key = 0
    
    Q = ∅
    for each vertex u ∈ G.V
        Insert(Q, u)
    
    while Q ≠ ∅
        u = Extract-Min(Q)  // add u to the tree
        for each vertex v in G.Adj[u]  // update keys of u’s non-tree neighbors
            if v ∈ Q and w(u, v) < v.key
                v.π = u
                v.key = w(u, v)
                Decrease-Key(Q, v, w(u, v))
```

### Prim’s Algorithm: Running Time

We implement $Q$ with a binary [[Priority Queue and Heaps|min-heap]].

- `Build-Min-Heap`
    - Lines 8-10
    - $O(|V|)$ time
    - % There is no need to call `Build-Min-Heap`
        - Can just put key of $r$ at the root of the min-heap
        - All other keys are $\infty$, so they can go anywhere else in the min-heap
- Body of the while loop executes $|V|$ times
    - Each `Extract-Min` operation is $\mathcal{O}(\log |V|)$
    - ![|500](https://i.imgur.com/oDjZxsl.png)
    - & Total time for all calls to `Extract-Min` is $\mathcal{O}(|V| \log |V|)$
- & For loop in lines 14-18 executes $\mathcal{O}(|E|)$ times altogether
    - Since sums of the lengths for all adjacency lists is $2|E|$
- Within for loop:
    - Test for membership in $Q$ in line 15:
        - Can take constant time if you keep a bit for each vertex that indicates whether it belongs to $Q$ and update the bit when vertex is removed from $Q$
        - [[Dictionaries and Hash Tables|Hash table]]
    - & Each call to `Decrease-Key` takes $\mathcal{O}(\log|V|)$ time
        - Need to bubble up

> [!info]+ Total time for Prim’s algorithm
> $$\mathcal{O}(V \log V + E \log V) = \mathcal{O}(E \log V)$$
> - Since $|E| \geq n - 1$ because it is *connected*

## Kruskal’s Algorithm

> [!idea]+
> - Do not build a tree until the end
> - Instead, build a *forest*
> - At each step:
>     - Pick the cheapest edges that does not make a cycle in $T$

- **Kruskal’s algorithm**
    - Finds a safe edge to add to the growing forest
        - Finding an edge $(u, v)$ with the lowest weight
            - Of all the edges that connect any two trees in the forest

Let $C_{1}, C_{2}$ denote the two trees that are connected by $(u, v)$.

- $(u, v)$ must be a light edge connecting $C_{1}$ to some other tree
    - → $(u, v)$ is a *safe* edge for $C_{1}$

### Kruskal’s Algorithm: Examples

![](https://i.imgur.com/EvLOBCf.png)

![](https://i.imgur.com/8Wa60k4.png)

![](https://i.imgur.com/eq65Mrm.png)

### Pseudocode

```pseudocode
MST-Kruskal(G = (V, E), w: E -> R):
    T <- {}
    sort edges so w(e_1) <= w(e_2) <= ... <= w(e_m)
    
    for i <- 1 to m:
        let (u_i, v_i) = (e_i)
        if ui, vi in different connected components of T:
            T <- T ∪ {e_i}
```

^da815c

### Kruskal’s Algorithm: Complexity

- ? How many steps to sort the edges?
    - Using heapsort → $m \log m$

> [!note]- Kruskal’s algorithm is $\mathcal{O}(E \lg V)$.
> - Using disjoint sets
> - Was not stated in lecture slides, but is in CLRS 21.2

> [!question]+ What is the hard part of executing this efficiently?
> - Check $u_{i}, v_{i}$ are in different connected components of $T$
> - Need to use **Disjoint Set ADT**

## Disjoint Set ADT

> [!def]+ Disjoint Set ADT
> - Objects:
>     - Collection of sets
>     - Intersections are empty
>     - Every element in collection is in exactly 1 set
> - Operations:
>     - `Make-Set(X)`
>         - Given an element $x$ that does not already belong to a set:
>             - Create a new set containing only $x$ and …
>     - `Find-Set(x)`
>         - Given an element $x$:
>             - Find the **representative** for the set that contains $x$, or
>             - NIL if $x$ is not in any set
>     - `Union(x, y)`
>         - Given two elements $x, y$, where $S_{x}$ is the set containing $x$ and $S_{y}$ is the set containing $y$:
>             - Make a new set $S_{x} \cup S_{y}$
>             - Designate a representative
>             - Delete $S_{x}$ and $S_{y}$

> [!note]+ Representative
> - An element in a set
> - Since sets are *disjoint*, representative is guaranteed to be different for every set

- ! There is no operation to get the membership of sets or get a set!

### Disjoint Sets: Kruskal’s Algorithm

- Each set contains the *vertices* in one tree of the current forest
- ? How to determine whether two vertices $u, v$ belong to the same tree?
    - `Find-Set(u)` operation returns a *representative* element from the set that contains $u$
    - & Test if `Find-Set(u)` and `Find-Set(v)` are equal
- ? How to combine trees?
    - Use `Union` procedure

#### Kruskal’s Algorithm with Disjoint Sets

![|600](https://i.imgur.com/JW6B7jA.png)
