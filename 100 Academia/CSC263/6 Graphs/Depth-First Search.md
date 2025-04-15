---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Depth-first search, DFS, Graph traversal, Graphs]
Week: 8
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "15"
Chapter: 
Slides/Notes: 
Date: 2025-03-04
Date created: Tue., Mar. 4, 2025, 1:22:20 pm
Date modified: Sat., Mar. 8, 2025, 3:21:54 pm
---

# Depth-First Search

## BFS vs. DFS

Consider performing the BFS and DFS algorithms on the *root* of a tree.

![](https://i.imgur.com/NFqMGlP.png)

- Breadth-first search in a tree (starting from root):
    - **Level-by-level** traversal
    - Use a queue
- Depth-first search
    - Visits the **child vertices** before visiting the sibling vertices
    - Use a *stack*
        - Store things yet to be explored, but keep adding children that you want to be explored first

## An Initial Implementation

Recall [[Breadth-First Search#^cfb9e1|BFS]] initial implementation:

![](https://i.imgur.com/y8cGSLo.png)

- DFS:
    - Instead of using a queue (BFS)
    - & Use a *stack*

```
NotYetDFSIte(T, root):
    S = ∅
    Push(S, root)
    
    while S not empty:
        u = Pop(S)
        print u
        for each child c of u:
            Push(S, c)
```

### Example

![|200](https://i.imgur.com/Fsw1KNi.png)

![](https://i.imgur.com/othR9ic.png)

- Want to print from left to right
    - % Add children in right-to-left order (reverse alphabetical)
- At each iteration:
    - Pop, print, add all children to stack

> [!obs]+ `NotYetDFSIte` is a stack simulation of a *recursive* algorithm.
>
> ```
> NotYetDFSRec(T, root):
>     print root
>     for each child c of root:
>         NotYetDFSRec(T, c)
> ```
>
> - € Rather than pushing and popping, can recursively call

## Depth-First Search Implementation

> [!impl]+ How do we avoid visiting a vertex twice?
> - & Remember the visited vertices by labelling them using colours
>     - Similar to [[Breadth-First Search|BFS]]
> - **White**
>     - *Unvisited* (undiscovered) vertices
> - **Grey**
>     - *Encountered* (discovered) vertices
> - **Black**
>     - *Explored* vertices
>     - Have been visited, and all of their neighbours are explored

- Initially:
    - All vertices are *white*
- & Change a vertex to *grey* the first time *visiting* it
    - i.e., Calling `DFSVisit` for the vertex
- & Change a vertex’s colour to *black* when all its *neighbours* have been *explored*
- & Avoid visiting — i.e., calling `DFSVisit` — for *grey* or *black* vertices
- In the end:
    - All vertices are *black*

### Timestamps

> [!info]+ Useful values to remember during the traversal
> - Vertex form which $v$ is encountered
>     - Stored in $v.p$
> - Keep track of two *timestamps* for each vertex $v$:
>     - There is a *timer* incremented whenever a vertex’s colour is changed
>     - **Discovery time**
>     - **Finishing time**

> [!def]- Discovery time
> - *Time* when $v$ is first *encountered*
>     - Stored in $v.d$
> - % This is different from BFS, where $v.d$ was the distance from $v$ to the source vertex

> [!def]- Finishing time
> - *Time* when all the neighbours of $v$ have been *completely visited*
>     - Stored in $v.f$

### Pseudocode

```python
DFS(G):
    for each t ∈ G.V:  # Initializing; set all vertices to white
        t.colour = White
        t.p = NIL
    
    time = 0
    
    for each s ∈ G.V:
        if s.colour == White  # Make sure NO vertex is left unvisited
            DFSVisit(G, s)
```

```python
DFSVisit(G, s):
    time = time + 1  # Time is a global variable
    s.d = time
    s.colour = Grey
    
    for each t ∈ G.adj[s]:  # Line 6
        if t.colour == White  # Only visit unvisited vertices
            t.p = s
            DFSVisit(G, t)  # Line 9
    
    s.colour = Black  # s is explored as all its neighbours have been encountered
    time = time + 1
    s.f = time  # Save finishing time after exploring all neighbours
```

- ? What lines are the same as `NotYetDFSRec`?
    - Lines 6, 9

### Example

Suppose we call `DFS(G)` on the graph below:

![|300](https://i.imgur.com/NYl7X9X.png)

- $\text{time} = 0$
---
Suppose DFS calls `DFSVisit(G, u)` first.

- Encounter the **source** vertex of `DFSVisit`
- $\text{time} = 1$

![|300](https://i.imgur.com/KgqelaN.png)

---
Suppose we then call `DFSVisit` on vertex $v$.
At level two of the recursive call:

- $\text{time} = 2$

![|300](https://i.imgur.com/pGBxlZc.png) ![|400](https://i.imgur.com/QbHA9ch.png)

| Call stack at $\text{time} = 2$ |
| ------------------------------- |
| `DFSVisit(G, v)`                |
| `DFSVisit(G, u)`                |

Node $v$ has one neighbour, $y$, so it calls `DFSVisit(G, y)`.

---

At level three of the recursive call:

- $\text{time} = 3$

![|300](https://i.imgur.com/EsWw7OL.png)

| Call stack at $\text{time} = 3$ |
| ------------------------------- |
| `DFSVisit(G, y)`                |
| `DFSVisit(G, v)`                |
| `DFSVisit(G, u)`                |

Call `DFSVisit` on node $x$.

---

At level four of the recursive call:

- $\text{time} = 4$

![|300](https://i.imgur.com/08XZ1wZ.png)

| Call stack at $\text{time} = 4$ |
| ------------------------------- |
| `DFSVisit(G, x)`                |
| `DFSVisit(G, y)`                |
| `DFSVisit(G, v)`                |
| `DFSVisit(G, u)`                |

---

Node $x$ has no white out-neighbours, so we exit the for loop.
We are still at level four of the recursive call:

- $\text{time} = 5$
- Set $x$ to black
    - Vertex $x$ is finished

![|300](https://i.imgur.com/ZLx8Tnj.png)

| Call stack at $\text{time} = 5$ |
| ------------------------------- |
| ~~`DFSVisit(G, x)`~~            |
| `DFSVisit(G, y)`                |
| `DFSVisit(G, v)`                |
| `DFSVisit(G, u)`                |

- % Can pop `DFSVisit(G, x)` off the stack (when finished operations on $x$)

---

Recursion goes back to level three:

- $\text{time} = 6$
- Vertex $y$ is finished

![|300](https://i.imgur.com/mtiFYcP.png)

| Call stack at $\text{time} = 6$ |
| ------------------------------- |
| ~~`DFSVisit(G, y)`~~            |
| `DFSVisit(G, v)`                |
| `DFSVisit(G, u)`                |

---

Recursion back to level two:

- $\text{time} = 7$
- Vertex $v$ is finished

![|300](https://i.imgur.com/0wHWVQ4.png)

| Call stack at $\text{time} = 7$ |
| ------------------------------- |
| ~~`DFSVisit(G, v)`~~            |
| `DFSVisit(G, u)`                |

---

Recursion back to level one:

- $\text{time} = 8$
- Vertex $u$ finished

![|300](https://i.imgur.com/0g2e5ZJ.png)

---

- & DFS visits all vertices
    - Whereas BFS only visits all reachable vertices from the source vertex

Suppose we call `DFSVisit(G, w)` from `DFS(G)`.

- Encounter the *source* vertex of `DFSVisit`
- $\text{time} = 9$

![|300](https://i.imgur.com/kIUSf8Y.png)

| Call stack at $\text{time} = 9$ |
| ------------------------------- |
| `DFSVisit(G, w)`                |

---

At level two of the recursive call:

- $\text{time} = 10$

![|300](https://i.imgur.com/suzoxpQ.png)

| Call stack at $\text{time} = 10$ |
| -------------------------------- |
| `DFSVisit(G, z)`                 |
| `DFSVisit(G, w)`                 |

---

Node $z$ has no white out-neighbours.
We are still at level two of the recursive call:

- $\text{time} = 11$
- Vertex $z$ finished

![|300](https://i.imgur.com/ArerhZk.png)

| Call stack at $\text{time} = 11$ |
| -------------------------------- |
| ~~`DFSVisit(G, z)`~~             |
| `DFSVisit(G, w)`                 |

---

Recursion back to level one:

- $\text{time} = 12$
- Vertex $w$ finished

![|300](https://i.imgur.com/x0L4vTh.png)

| Call stack at $\text{time} = 12$ |
| -------------------------------- |
| ~~`DFSVisit(G, w)`~~             |

## DFS Forest

![|400](https://i.imgur.com/F7BPgci.png)

> [!note]+ Depth-first search creates DFS *forests*
> - Unlike BFS, which creates BFS trees
> - DFS searches *all* vertices
>     - While BFS only explores vertices that are reachable

### Edge Classification in the DFS Forest

![](https://thealgoristsblob.blob.core.windows.net/thealgoristsimages/dfs_edges.png)

- **Tree edges**
    - An edge $(u, v)$ in the DFS forest such that $v$ is *white* when edge $(u, v)$ is examined
- **Back edge**
    - An edge $(u, v)$ such that $v$ is an ancestor of $u$ in the DFS forest, and
    - $v$ is *grey* when edge $(u, v)$ is examined
- **Forward edge**
    - An edge $(u, v)$ such that $v$ is a descendant of $u$ in the DFS forest, and
    - $v$ is *black*  when edge $(u, v)$ is examined
- **Cross edge**
    - An edge $(u, v)$ that connects two nodes such that $v$ is neither an ancestor nor descendant of $u$
    - $v$ is *black* when edge $(u, v)$ is examined
    - & No overlap in discovery and finish times
    - e.g., An edge across trees in a DFS forest (right blue arrow)
    - Can think of as doing a BFS search on the source node, and $u,v$ are on the same level

![](https://i.imgur.com/PKqsxDl.png)

## DFS Running Time

Assuming that we are using *adjacency lists*, the total amount of work is:

1. & Visit each vertex once → $\Theta(n)$
    - Assign values to $v.colour, v.d, v.p$, etc.
2. & At each vertex, check all its neighbours (i.e., all its incident edges) → $\Theta(m)$
    - Each edge is checked at most twice (by the two end vertices, in an undirected graph)

> [!thm]+ Total running time for DFS
> $$\Theta(n + m)$$

> [!question]- What is the DFS worst-case running time when using an adjacency matrix?
> $$\Theta(n^{2})$$

## DFS Properties

> [!note] DFS can be performed on both *directed* and *undirected* graphs.

- $ *Timestamps* generated by the DFS algorithm have **parenthesis structure**

> [!info]+ Parenthesis Structure
> - Either one pair *contains* another pair, or
> - One pair is *disjoint* from another

![|200](https://i.imgur.com/vj7sI2v.png)

- This is a valid type of parenthesis

![|150](https://i.imgur.com/ddIR2jK.png)

- Overlapping *never* happens

Consider the DFS forest from earlier:

![|400](https://i.imgur.com/F7BPgci.png) ![|300](https://i.imgur.com/IQiNg0r.png)

- $v$ interval completely included in $u$
- $y$ is a predecessor of $x$

### Parenthesis Theorem

> [!thm]+ Parenthesis Theorem
> In any depth-first search of a graph $G$, for any two vertices $u$ and $v$,
> 1. Interval $[v.d, v.f]$ contains interval $[u.d, u.f]$
> 2. Interval $[u.d, u.f]$ contains interval $[v.d, v.f]$
> 3. $[v.d, v.f]$ and $[u.d, u.f]$ are disjoint i.e., no overlap
>
> (Theorem 22.7 of CLRS)

> [!thm]+ Nesting of Descendants’ Intervals
> In the depth-first forest for a graph $G$,
> - Vertex $v$ is a proper *descendant* of vertex $u$ if and only if the interval $[u.d, u.f]$ *contains* $[v.d, v.f]$
> That is,
> $$u.d < v.d < v.f < u.f$$
>
> (Corollary 22.8 of CLRS)

## Applications of DFS

> [!info]+ Applications of DFS
> 1. Detecting cycles in graphs
> 2. Topological sort
> 3. Finding strongly connected components
>     - Section 22.5 of CLRS, optional

> [!question]+ For each of the following graphs, can you order the vertices such that for every edge $(u, v)$ in the graph, $u$ comes before $v$ in your ordering?
> 1. ![|200](https://i.imgur.com/TAH1Sou.png)
> 2. ![|200](https://i.imgur.com/wCFLmoC.png)
> 3. ![|200](https://i.imgur.com/ugzuSNT.png)
> 4. ![|400](https://i.imgur.com/sDZELWE.png)
>
> > [!check]- Graph 1
> > ![|200](https://i.imgur.com/wfZ1RYB.png)
>
> > [!check]- Graph 2
> > ![|200](https://i.imgur.com/GauDgzf.png)
>
> > [!check]+ Graph 3
> > - $a$ needs to come before $b$
> > - $b$ needs to come before $c$
> > - $c$ needs to come before $a$
> >     - ! But $a$ needs to come before $b$, and $b$ needs to come before $c$
> >
> > If there is a *cycle*, linearizing the graph will *not* work.

### Detecting Cycles

Recall the definition of [[Discover Graphs|cycles]] in graphs.

> [!def]+ Cycle
> In a graph, a **cycle** is a path from a vertex $u$ to *itself*.

- If we know that there exists an edge between $v$ and $u$, $(v, u)$, and there exists *another* path between $u$ and $v$ (other than the edge $(v, u)$), we can say:
    - There exists a *path* from $u$ to itself
    - → Graph has a cycle

> [!info]+ General case (directed and undirected graphs)
> Consider a graph $G$.
>
> Suppose $u$ is an *ancestor* of $v$ in a DFS forest of $G$.
> This means that there exists a **path** from $u$ to $v$.
>
> Assume that there is an edge from $v$ to $u$. Note that this is a **back edge**.
> Then, we can say that a **cycle** is detected.

- **Tree edge**
    - An edge in the DFS forest
- **Back edge**
    - A non-tree edge pointing from a vertex to its *ancestor* in the DFS forest

> [!thm]+ Lemma 22.11 of CLRS
> A graph contains a **cycle** if and only if DFS yields a **back edge**.

![|400](https://i.imgur.com/x0L4vTh.png)

- Edge $(x, v)$ is a *back edge*
    - There is a cycle starting at vertex $v$
    - Path is $(v, y), (y, x), (x, v)$ (length 3)

> [!question]+ How do we identify a *back edge*?
> - When performing DFS, look for edges to *grey* vertices
>     - If such an edge exists, it is a **back edge**
> - & In DFS, ancestors of the vertex being visited are grey
>
> ![|300](https://i.imgur.com/AuBdmCv.png)

> [!question]+ Why do we care about detecting cycles?
> - **Topological sort**
> - If edges represent dependency relations, then having a cycle implies **cyclic dependency**

#### Example: Course Prerequisite Graph

If the graph has a cycle, then all courses in the cycle become impossible to take.

![|300](https://i.imgur.com/2YdCG83.png)

### Topological Sort

[[Topological Sort]]
