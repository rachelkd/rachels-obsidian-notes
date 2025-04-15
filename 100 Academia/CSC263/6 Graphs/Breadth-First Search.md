---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [BFS, Breadth-first search]
Week: 7
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "14"
Chapter: 
Slides/Notes: 
Date: 2025-02-27
Date created: Thu., Mar. 6, 2025, 3:00:19 am
Date modified: Thu., Mar. 27, 2025, 10:19:16 pm
---

# Breadth-First Search

## Introduction

> [!def]+ Breadth First Search
> - Starting from source $s$ in $V$:
>     - BFS visits every vertex $v$ reachable from $s$
> - In the process, we find paths from $s$ to each reachable vertex $v$
> - Paths make BFS tree rooted at $s$
> - Works on directed and undirected graphs
> - Get **level-by-level** traversal

> [!idea]+ Intuition
> Starting from $s$,
> - First visit *all* (unvisited) neighbours of $s$
> - Then visit *all* (unvisited) neighbours of all neighbours of $s$
> - Then visit *all* (unvisited) neighbours of all neighbours of all neighbours of $s$
> - And so on
> - Until all reachable vertices are visited

> [!question]+ Consider performing the BFS traversal on the root of a tree. What ADT should we use to store vertices in level-by-level tree traversal?
> - A tree ADT, called a **BFS tree**
>
> ![|300](https://i.imgur.com/KKyVXQt.png)

## An Initial Implementation

```
NotYetBFS(T, root):
    Q = ∅
    Enqueue(Q, root)

    while Q not empty:
        u = Dequeue(Q)
        print u
        
        for each child v of u:
            Enqueue(Q, v)
```

^cfb9e1

### Example with a Graph that is a Tree

![|300](https://i.imgur.com/SomMsWj.png)

![|500](https://i.imgur.com/lllRtUo.png)

### Example with Graph that is not a Tree

> [!question]+ What happens if we run `NotYetBFS` on a non-tree graph?
> - BFS will try to visit some of the vertices *twice*

![|400](https://i.imgur.com/ECiWrIE.png) ![|400](https://i.imgur.com/IKlv4PK.png)

- $ $v_{4}$ is enqueued twice
- This implementation may repeat vertices already printed!
    - Need a way of tracking which vertices have been unvisited, encountered, and explored

> [!def]+ **Source** vertex
> - The *starting vertex* of a BFS call

## How to Avoid Visiting a Vertex *twice*

> [!impl]+ How do we avoid visiting a vertex twice?
> Remember the visited vertices by ==labelling== them with colours.
>
> - **White**
>     - *Unvisited* (undiscovered) vertices
>     - Have not been enqueued
> - **Grey**
>     - *Encountered* (discovered) vertices
>     - Have been enqueued
> - **Black**
>     - *Explored* vertices
>     - Have been dequeued and all of their neighbours are enqueued

- Initially:
    - All vertices are *white*
- & Change a vertex’s colour to *grey* the first time *visiting* (enqueuing) it
- & Change a vertex’s colour to *black* when all its *neighbours* have been *encountered* (enqueued)
- & Avoid visiting (enqueuing) *grey* or *black* vertices
- In the end:
    - All vertices that are reachable from the source are *black*

- ? What are some other useful values to remember during traversal?
    - The vertex from which $v$ is encountered
        - Stored in $v.p$
        - i.e., the vertex’s parent in a BFS tree
    - The **distance** value
        - Distance *from $v$* to the *source* vertex of the BFS
        - Stored in $v.d$

## Breadth-First Search

### Pseudocode

```python
BFS(G, s):
    for each vertex v in V - {s}:  # Initialize vertices
        v.colour = White
        v.d = +infty
        v.p = NIL
    
    Q = ∅
    s.colour = Grey  # Start BFS by encountering the source vertex
    s.d = 0  # Distance from s to s is 0
    
    Enqueue(Q, s)
    
    while Q not empty:
        u = Dequeue(Q)
        for each v in G.adj[u]:
            if v.colour == White  # Only visit unvisited vertices
                v.colour = Grey
                v.d = u.d + 1  # v is "1-level" farther from s than u
                v.p = u
                Enqueue(Q, v)
        u.colour = Black  # u is explored as all its neighbours have been encountered
```

^6434ac

- Lines that are the same as `NotYetBFS`:
    - 7, 11, 13, 14, 15, 20

### Example

![|600](https://i.imgur.com/Q8JwcSN.png)

![|200](https://i.imgur.com/JqXdOEl.png)

*BFS Tree of `BFS(G, v4)`*

- The *introducer* of $v_{3}$ is $v_{5}$
    - Dequeuing $v_{5}$ made the algorithm enqueue i.e. introduce $v_{3}$

### BFS Tree

> [!def]+ BFS Tree
> - Set of all edges between vertices and their *introducer* in a BFS search
> - Tree in the BFS traversal

### What if $G$ is Disconnected?

![](https://i.imgur.com/Cuak8DS.png)

- **Infinite distance** values of $v_{6}, v_{7}$
    - i.e., $v_{6}.d, v_{7}.d$
    - Indicate that they are *unreachable* from the source

## BFS Worst-Case Running Time

> [!info]+ The total amount of work assuming *adjacency list* implementation
> 1. When visiting each vertex
>     - Enqueue, Dequeue, assign values to $v.colour, v.d, v.p$, etc.
> 2. At each vertex, check all its neighbours (i.e., all its incident edges)
>     - Each edge is checked at most twice (by the two end vertices)

- Total for visiting every vertex:
    - $\Theta(n)$
- Total for checking each vertex’s neighbours:
    - $\Theta(m)$ since there are at most $2m$ edges

Then, the total running time is
$$
\Theta(n + m)
$$

> [!question]+ What is the BFS worst-case running time when using an adjacency *matrix*?
> - Need to go through entire matrix
> 1. Go through all $n$ columns for a particular row
> 2. Do this for all $n$ vertices
> $$
> \Theta(n^{2})
> $$

## BFS Properties

- BFS can be performed on both *directed* and *undirected* graphs
- ? What do we get after performing a BFS?
    - Get to visit every vertex which is *reachable* from the **source** $s$
    - Generate a **BFS tree**
        - Each edge in a BFS tree is called a **BFS tree edge**
        - & BFS tree connects *all* vertices, if graph is **connected**
            - & i.e., If graph is *not* connected, then for *every* BFS tree of the graph, *some* vertices of the graph are *not included* in the tree
    - & For every vertex $v$, $v.d$ stores the *minimum* distance (or *shortest-path* distance) between the BFS **source** and $v$
        - To get the shortest path:
            - Follow $.p$ attributes from $v$ to $s$ backwards

> [!tip]+ If you see “shortest path” on a test or example, you should consider a **graph** and **breadth-first search**.

### Proof that BFS Finds the Shortest Paths

Define $\sigma(s, v)$ to be the minimum distance from $s$ to $v$.

> [!thm]+ Claim
> At the end of breadth-first search, $v.d = \sigma(s, v)$.

> [!thm]+ Lemma: $\sigma(s, v) \leq \sigma(s, u) + 1$ for all $(u, v) \in E$
> If $u$ is reachable from $S$, then so is $v$.
> $\sigma(s, v)$ cannot be longer than $\sigma(s, u) + \underbrace{ \text{going down edge }(u, v) }_{ 1 }$.
>
> If $u$ is not reachable from $S$, then $\sigma(s, u) = \infty$.

> [!thm]+ Lemma: $v.d \geq \sigma(s, v)$ for all $v$ in $V$, at any point during BFS
> *Proof* (by induction on the number of enqueue operations).
> Base Case:
> When $s$ is enqueued, then $s.d = 0$. This is $\sigma(s, s)$.
> Also,
> $$v.d = \infty \quad \text{for all} \quad v \in V - \{ s \}$$
>
> Induction Hypothesis:
> When $v$ is discovered from parent $u$, then $u.d \geq \sigma(s, u)$
>
> Inductive Step:
> $$v.d \stackrel{(\text{IH})}{=} u.d + 1 \geq \sigma(s, u) + 1 \stackrel{\text{(Lemma 1)}}{\geq} \sigma(s, v)$$

- Every time that $v$ starts, $v.d = \infty$
- Every time that $v$ sets, $v.d \geq \text{shortest path}$

> [!thm]+ Lemma: If $Q = [v_{1}, \dots, v_{r}]$, then $v_{i}.d \leq v_{i + 1}.d$ for each $i$ and $v_{r}.d \leq v_{1}.d + 1$
> $r$ is the number of elements in the queue.
>
> *Proof* (by induction on the number of queue operations).
>
> Base Case:
> Queue $Q$ only contains $s$, so $s.d \leq s.d + 1$. Trivial ordering.
>
> Inductive Hypothesis:
> Assume Lemma holds for $Q = [v_{1}, \dots, v_{r}]$.
>
> Inductive Step:
> We dequeue $v_{1}$ at the front.
> Then, $Q = [v_{2}, \dots, v_{r}]$.
> By IH, $v_{r} \leq v_{1}.d + 1$ and $v_{1}.d \leq v_{2}.d$, so
> $$v_{r} \leq v_{2}.d + 1$$
> This works because BFS only discovers vertices in the next layer after the current layer, so this is correct (assuming BFS order is preserved).
>
> Enqueue: $Q = [v_{1}, v_{2}, \dots, v_{r}, v_{r+1}]$
> Let $u$ be the last vertex dequeued i.e., the vertex that triggered new enqueues.
> So, when we discovered $v_{r + 1}$, we took edge $(u, v_{r + 1})$.
> $$v_{r+1}.d = u.d + 1$$
> Before dequeuing: we had $Q = [u, v_{1}, \dots, v_{r}]$.
> By IH,
> $$v_{r}.d \leq u.d + 1 = v_{r + 1}.d$$
> Then, $v_{r + 1}.d \leq u.d + 1$.

> [!thm]+ Theorem: At the end of BFS, $v.d = \sigma(s, v)$ for all $v$.

*Proof (by contradiction).*
Assume there exists a vertex $v$ such that
$$v.d > \sigma(s, v)$$
with minimum $\sigma(s, v)$.

Choose $v$ to be the first vertex on some path when $\sigma(s, v)$ is shorter than $v.d$.
Let $u$ be the vertex just before $v$ on that path.

So, when we discovered $v$ and set $v.d$, we were examining edge $(u, v)$.
Then,
$$
\sigma(s, v) = \sigma(s, u) + 1
$$
By minimal choice of $v$, BFS computed:
$$
u.d = \sigma(s, u)
$$
Since we assume $v.d > \sigma(s, v)$, we get
$$
\begin{align}
v.d & > \sigma(s, v) \\
 & = \sigma(s, u) + 1 \\
 & = u.d + 1
\end{align}
$$
So, $v.d > u.d + 1$.

Now, consider the point when we dequeue vertex $u$ on line 10.
We then explore edge $(u, v)$ and colour $v$.

**Case 1 ($v.colour$ is white):**
Then, we set $v.d = u.d + 1$.
We then enqueue $v$, colour it, and never change $v.d$.
This contradicts $v.d > u.d + 1$

**Case 2 (black):**
Vertex was in the queue earlier i.e., before $u$.
Then, by Lemma 3, $v.d \leq u.d$.
This is a contradiction.

**Case 3 (grey):**
It was painted grey by some vertex $w$, looking at edge $(w, v)$. That is, $v$ was discovered earlier via some other vertex $w$ and edge $(w, v)$.
Then,
$$
\begin{align}
v.d & = w.d + 1 \\
 & \leq u.d + 1 && (\text{since }w.d \leq u.d \text{ by Lemma 3})
\end{align}
$$
Which is a contraction.

All cases contradict $v.d > \sigma(s, v)$, so $v.d = \sigma(s, v)$.
<div class="right-align">■</div>

## Course Note Exercises

> [!question]+ Build on our BFS algorithm to visit every vertex in a graph, even if the graph is not connected. Hint: you’ll need to use a BFS more than once.
>
> > [!info]- `BFS` implementation
> > ![[#^6434ac]]
>
> ```python
> BFS-Visit-All(G):
>     for each v ∈ G.V:  # Initialize vertices
>             v.colour = White
>             v.d = +infty
>             v.p = NIL
>         
>     for each s ∈ G.V:
>         if s.colour == White  # Make sure NO vertex is left unvisited
>             BFS-Modified(G, s)
> ```
>
> ```python
> BFS-Modified(G, s):
>     # Do not have to initialize vertices here anymore
>     
>     Q = ∅
>     s.colour = Grey  # Start BFS by encountering the source vertex
>     s.d = 0  # Distance from s to s is 0
>     
>     Enqueue(Q, s)
>     
>     while Q not empty:
>      u = Dequeue(Q)
>      for each v in G.adj[u]:
>          if v.colour == White  # Only visit unvisited vertices
>              v.colour = Grey
>              v.d = u.d + 1  # v is "1-level" farther from s than u
>              v.p = u
>              Enqueue(Q, v)
>          u.colour = Black  # u is explored as all its neighbours have been encountered
> ```
