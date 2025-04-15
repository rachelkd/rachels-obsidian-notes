---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 7
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "13"
  - "14"
Chapter: 
Slides/Notes: 
Date: 2025-02-25
Date created: Tue., Feb. 25, 2025, 3:37:40 am
Date modified: Fri., Mar. 7, 2025, 4:30:11 pm
---

# Graph Representations

## Graph Definition

> [!def]+ Graph
> A **graph** is a tuple of two sets $G = \langle V, E \rangle$, where $V$ is a set of **vertices**, and $E$ is a set of **edges** between those vertices.
> $E$ itself is a set of tuples $(v_{i}, v_{j})$, where $v_{i}, v_{j} \in V$.

![|400](https://i.imgur.com/ItxHzhR.png)

> [!info]+ Applications of Graphs
> - Network traffic flow
> - Social webs
> - Task scheduling (operating systems)
> - Maps and GPS
> - VLSI and chip design

## Basic Definitions

See [[Discover Graphs]] for definitions.

![|400](https://i.imgur.com/WLKkwLC.png) ![|400](https://i.imgur.com/M7WsSoY.png)

*Left: an **undirected** graph. Can think of the edges as bidirectional if they do not have arrows on them.*
*Right: a **directed** graph.*

- **Vertices**
- **Edges**
- **Weighted** / **unweighted** graphs
- **Connected** / **disconnected** graphs
- **Adjacent** / **non-adjacent** vertices
    - In undirected graphs:
        - $u$ and $v$ are *neighbours* $\iff$ $u$ is *adjacent to* $v$ $\iff$ $v$ is adjacent to $u$
        - The **neighbourhood** of $v$ is the *set of vertices* that share an edge with $v$
    - In directed graphs:
        - $u$ is a *in-neighbour* of $v$ $\iff$ $(u, v)$ is an edge in $E$ $\iff$$v$ is *adjacent to* $u \iff v$ is an *out-neighbour* of $u$
        - The **in-neighbourhood** of $v$ is the *set of vertices* $u$ such that $(u, v)$ is an edge
- **Paths** and **cycles**
- **Path lengths**
- **Distance** between two nodes
    - In an undirected graph:
        - Number of edges in the shortest path connecting them
        - Essentially representing the minimum number of “hops” required to travel from one vertex to the other on the graph
    - In a directed graph:
        - Sum of the weights of the edges on the shortest path connecting them
- **In-degree** / **out-degree** of vertices
    - Only defined if graph is *directed*
    - Same goes for “in-neighbourhood” and “out-neighbourhood” and “in-adjacent” and “out-adjacent”
    - ![|300](https://i.imgur.com/lTaQiQS.png)
- **Degree** of a vertex
    - Undirected: Number of edges that vertex is also in
    - ![|300](https://i.imgur.com/XtJkIoY.png)

### Examples

> [!note] If neither type of graph is specified, our default will be an **undirected** graph.

- Facebook friendship can be represented by *undirected* graphs
    - Each person (account) is represented by a vertex
    - $u$ and $v$ are friends $\iff$ there exists an edge between $u$ and $v$

- Instagram followers can be represented by *directed* graphs
    - Each person (account) is represented by a vertex
    - $u$ follows $v$ $\iff$ there exists an edge from $u$ to $v$

> [!note] Our default is that a graph is *not* weighted, unless otherwise specified.

- Network traffic flow can be presented by *weighted directed* graphs
    - Each hub is represented by a vertex
    - Incoming traffic from a hub $u$ to a hub $v$ is represented by an incoming edge from $u$ to $v$
        - $(u, v)$
    - Outgoing traffic from a hub $v$ to a hub $z$ is represented by an outgoing edge from $v$ to $z$
        - $(v, z)$
    - Traffic flow is represented by the weight of each edge

- Maps can be represented by *weighted* graphs
    - Implicitly undirected
    - Each address point is represented by a vertex
    - Roads between address points are represented by edges
    - The distance between two points is represented by the weight of the edge between the points

### Minimum and Maximum Number of Edges in an Undirected Connected Graph

> [!info]+ Minimum number of edges in an undirected **connected** graph with $n$ vertices
> $$n - 1 \; \text{edges}$$
> ![|350](https://i.imgur.com/qXZ70gk.png)
> - Simplest graph is a line

> [!info]+ What about the maximum number of edges in an undirected connected graph with $n$ vertices?
> ![|300](https://i.imgur.com/rczPKTS.png)
>
> $$(n - 1) + (n - 2) + \dots + 1 = \frac{n(n-1)}{2} \; \text{edges}$$

## Standard Operations on Graphs

- Add/remove a **vertex**
- Add/remove an **edge**
- **Edge query**
    - Given vertices $u, v$, does $G$ contain edge $(u, v)$ or $(v, u)$?
- **Neighbourhood** (undirected graph)
    - Given vertex $v$, return a set of vertices $u$ such that $(v, u) \in E$
- **In-neighbourhood** / **out-neighbourhood** (directed graph)
    - Given vertex $v$, return a set of vertices $u$ such that $(u, v) / (v, u) \in E$
- **Degree** / **in-degree** / **out-degree**
    - *Size* of neighbourhood / in-neighbourhood / out-neighbourhood
- **Traversal**
    - Visit each vertex of a graph to perform some task

## Data Structures for Graphs

### Adjacency Matrix

> [!def]+ Adjacency Matrix
> Let $V = \{ v_{1}, v_{2}, \dots, v_{n} \}$ and $|V| = n$.
> Let $A$ be a $n \times n$ matrix.
>
> $$A[i, j] = \begin{cases} 1 && \text{if } (v_{i}, v_{j}) \in E \\ 0 && \text{otherwise} \end{cases}$$

![|404x206](https://i.imgur.com/KXusJie.png)

![|300](https://i.imgur.com/CTehCRs.png)

- Matrix is symmetric with respect to the diagonal

> [!thm]+ For an undirected graph, adjacency matrix is *symmetric*.
> $$A[i, j] = A[j, i]$$

#### Weighted Graphs

- For a **weighted** graph:
    - If $(v_{i}, v_{j} \in E)$:
        - Store $w(v_{i}, v_{j})$ in $A[i, j]$
        - $w(v_{i}, v_{j})$ denotes the *weight* of the edge $(v_{i}, v_{j})$
    - If $(v_{i}, v_{j}) \not\in E$:
        - Store $-1 / 0 / \infty$ depending on application

![|300](https://i.imgur.com/0raIoLL.png)

![|300](https://i.imgur.com/w0L06gG.png)

#### Add/Remove Vertex Time

![|400|400x247](https://i.imgur.com/Gy3ZMBE.png)

![|300](https://i.imgur.com/j55INgz.png)

> [!obs] Insertion is in $\Theta(n)$. Deletion is similarly $\Theta(n)$.

### Adjacency Lists

> [!def]+ Adjacency Lists
> Let $V = \{  v_{1}, v_{2}, \dots, v_{n} \}$ and $|V| = n$.
> Let $L$ be a list of size $n$.
>
> Each position $L[i]$ corresponds with a vertex $v_{i}$ and stores a list $A[i]$ of all vertices that have an edge from $v_{i}$:
> - $A_{i}$ includes $v_{j} \iff (v_{i}, v_{j}) \in E$

![|400](https://i.imgur.com/MhHcxuZ.png)

![|300](https://i.imgur.com/agcaKvo.png)

- Can use any list representation
    - e.g., Linked list or array

> [!info]+ For an undirected graph, edge $(u, v)$ is stored *twice*.
> - $u$ in sub-list corresponding with $v$
> - $v$ in sub-list corresponding with $u$

#### Add/Remove Vertex Time

![|400](https://i.imgur.com/JNeHcWJ.png)

> [!obs] Adding a vertex is $\Theta(1)$.

![|400](https://i.imgur.com/3a2o5j7.png)

### Sparse Graph Definition

> [!def]+ Sparse graph
> A graph is a **sparse** graph if:
> - $m = |E|$ is much less than $n^{2} = |V|^{2}$
>     - $m \ll n^{2}$
> - i.e., $m$ is at most $\mathcal{O}(n)$

### Time Analysis

Let $|V| = n$.
Let $|E| = m$.

> [!important]+ We assume that looking up a vertex in $V$ takes constant time.
> - This is true if an array is used
> - David Liu’s notes leave it as an exercise to think about other implementations of this attribute
>
> > [!idea]- My idea
> > - Use a **[[Dictionaries and Hash Tables|hash table]]** that maps a vertex $v$ (its label attribute to identify the vertex) to the *position* in the *array*, which points to a linked lists of its (out-)neighbours i.e., vertices that are adjacent to $v$

|                     | Adjacency Matrix | Adjacency Lists                                                                                                                                                                 |
| ------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Space Complexity    | $\Theta(n^{2})$  | Undirected: $\Theta(n + 2m) = \Theta(n + m)$<br><br>Directed: $\Theta(n + m)$                                                                                                   |
| Add/Remove a Vertex | $\Theta(n)$      | Add: $\Theta(1)$<br><br>Remove (undirected): $\Theta(m)$<br>Remove (directed): $\Theta(n + m)$                                                                                  |
| Edge Query          | $\Theta(1)$      | $\Theta(\text{min}(n, m))$                                                                                                                                                      |
| Neighbourhood       | $\Theta(n)$      | Undirected: $\Theta(\text{min}(n, m))$<br><br>Directed (in): $\Theta(n + m)$<br>Out-neighbourhood would be the same algorithm as undirected graphs: $\Theta(\text{min}(n, m))$. |

> [!info]+ Removing a vertex
> Suppose we want to remove $v_{i}$
> - **Adjacency matrix**:
>     - Have to set all entries in the $i$-th row and the $i$-th column to zero
>         - Deleting column will delete all edges *incident to* $v_{i}$
>         - → Removes $v_{i}$ from the graph
>     - Iterate over $2n$ cells in total
>     - → $\Theta(n)$
> - **Adjacency list**:
>     - To remove all outgoing edges:
>         - Set pointer to $v_{i}$‘s list, $L_{i}$, to `NIL`
>     - To delete all occurrences of $v_{i}$ from other node’s lists:
>         - Iterate over all the other lists
>     - Undirected:
>         - Know which vertices share an edge with $v_{i}$ because of the list that $L[i]$ points to
>         - $\Theta(m)$ on average
>     - Directed:
>         - Must traverse entire graph to find vertices that have edges incident to $v_{i}$
>         - $\Theta(n + m)$
>             - Check each vertex and its edges to identify and remove references to $v_{i}$
>     - See the discussion about [[#^c4167d|neighbourhood]] running time for a more detailed explanation

> [!info]+ Edge query (checking if an edge exists between two vertices):
> - **Adjacency matrix**:
>     - Accessing the matrix at position $A[i][j]$ allows for constant-time edge existence checks
>     - → $\Theta(1)$ time complexity
> - **Adjacency list**:
>     - Requires traversing the adjacency list of a vertex to check for the presence of an edge
>     - Each vertex maintains a linked list of its adjacent vertices
>     - Time complexity of this search depends on the length of $u$‘s adjacency list
>         - & Equal to the *degree* of vertex $u$

> [!question]+ What are the bounds for the *degree* of vertex $u$ in a graph $G = \langle V,E \rangle$?
> - If there are $n$ vertices in the graph, then $u$ can have at most $n - 1$ (out-)neighbours
>     - i.e., There are at most $n - 1$ vertices adjacent to $u$
> - It is tempting to say that this is $\mathcal{O}(n)$
>     - ! But there are *two* measures of the size of the graph: $n = |V|, m = |E|$
> - If the graph has $m$ edges in total, then $m$ is also an upper bound on the number of neighbours that $u$ can have
>
> > [!info] The degree of a vertex $u$ is $\Theta(\text{min}(n, m))$
>
> > [!example]-
> > Consider a graph $G$ with $n = 1000$ nodes and $m = 2$ edges.
> >
> > - The maximum number of neighbours is then $m = 2$
> >     - We know we have 2 total edges, but we do not know *which* edges; however, we know that every vertex cannot have more than 2 neighbours
> >
> > Consider a complete graph with $m = \frac{n(n-1)}{2} = \Theta(n^{2})$ edges.
> >
> > - The maximum number of neighbours is then $n - 1$ (undirected) or $n$ (directed)
> >     - Cannot have duplicate edges
> >
> > Both these examples are bounded by $\Theta(\text{min}(n, m))$.

> [!info]+ Neighbourhood
> - **Adjacency list**:
>     - The number of neighbours (in undirected graphs) and out-neighbours (in directed graphs) is bounded by a vertex’s (out-)*degree*
>         - Similar to edge queries
>         - The justification for the $\Theta(\text{min}(n, m))$ bounds are the same for edge queries
>     - & However, to find the in-neighbourhood of a vertex, the running time is $\Theta(n + m)$
>         - Each vertex stores a list of its *outgoing* edges (i.e., out-neighbours it points to, or a list of vertices *adjacent* to a vertex $u$)
>         - Incoming edges (edges incident to $u$) are not stored
>     - To find all in-neighbours of $u$:
>         - Must iterate over *every* vertex $v$ in the graph → $\Theta(n)$
>         - For each $v$, scan its adjacency list to check if $u$ is present → $\Theta(\text{out-degree}(v))$
>         - Then, the total work is $$\sum\limits_{v\in V} \left( 1 + \text{out-degree}(v) \right) = n + m = \Theta(n + m) $$

^c4167d

> [!important] If we need to traverse the entire list, it is $\Theta(n + m)$. If we need to traverse a constant number, $c$, of adjacency lists for $c$ different vertices, it is $\Theta(\text{min}(n, m))$.

> [!tip]+ Choose the more appropriate implementation depending on the problem.
> - Use *adjacency matrix* if:
>     - Lots of edge queries
>     - Graph is *dense*
>         - Number of edges $m$ is close to $n^{2}$ i.e., not sparse
>     - For connected graphs since $n - 1 \leq m \leq \frac{n(n-1)}{2}$
> - Use *adjacency lists* if:
>     - Sparse graphs
>     - Iterating over neighbours
