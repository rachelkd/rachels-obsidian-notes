---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Graphs]
Week: 9
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "17"
Chapter: 
Slides/Notes: 
Date: 2025-03-11
Date created: Tue., Mar. 11, 2025, 3:04:05 am
Date modified: Tue., Mar. 18, 2025, 2:01:09 am
---

# Discover: Spanning Trees

## Introduction

- When we run either [[Depth-First Search|DFS]] or [[Breadth-First Search|BFS]] on an *undirected connected graph*:
    - $ Algorithm produces a tree that includes every vertex from the original graph
    - This is a **spanning tree**
        - Since the BFS and DFS tree *spans* the vertices of $G$ (the original graph)

- **Spanning tree** (of a connected graph)
    - A subgraph that includes all the vertices of the original graph
    - Forms a tree
        - & → *Connected* and contains *no* cycles

![|300](https://i.imgur.com/QooGXQu.png)

Consider the graph above.

- ? What are the sets of edges for a possible spanning trees?
    - $\{ (a,c),\;(c, b),\; (c,e),\; (e,d) \}$
    - $\{ (d,c),\;(d,e),\;(c,b),\;(a,e) \}$

![](https://i.imgur.com/03mc6jn.png)

- Things to notice:
    - Do not typically designate a root for a spanning tree
    - Spanning tree cannot contain *any* cycle
        - Or else, it would not be a tree

Consider the edges in the graph that are *not* edges in the spanning tree for this spanning tree: $\{ (a,c),\;(c, b),\; (c,e),\; (e,d) \}$.
i.e., Consider the pink edges.

- $ If we add any of these pink edges to our spanning tree, then we would have a **cycle**

Consider the edge $(c,d)$.

- ? What happens if we add to our tree?
    - Would have a cycle
    - $\{ (c,d),\;(d,e),\;(e,c) \}$

Suppose we went ahead and added the edge $(c, d)$.

- Could remove either:
    - $(d,e)$, or
    - $(e,c)$
- → Would have a different spanning tree

> [!goal] Usually, in some real application, our goal is not to simply find a spanning tree, but to find the one that is *best* in some sense.

## Minimum Spanning Trees

![](https://i.imgur.com/mxYFwGX.png)

*Graph 2: a weighted undirected graph.*

- Recall that in a weighted graph:
    - Each edge is assigned a *weight* or a *cost*

> [!def]+ **Weight of a spanning tree**
> - The total of the weights on all its edges

- Example:
    - Spanning tree: $\{ (t,s),\; (t,q),\; (q,r),\; (r,p) \}$
    - Weight of the spanning tree:
        - $11.5 = 5 + 1 + 3 + 2.5$

> [!def]+ **Minimum spanning tree** of a graph $G$
> - A spanning tree of $G$ with the minimum possible weight of any spanning tree

- Might be multiple minimum spanning trees for the same graph
    - if they have the same minimum weight

- For a graph like this:
    - Can simply eyeball the graph and spot the minimum spanning tree
- However, in order to do this in real applications, we need an *algorithm* that we can be sure is correct and efficient
- @ **Prim’s algorithm** and **Kruskal’s algorithm**
    - Both take an undirected connected graph as input and produce a minimum spanning tree
