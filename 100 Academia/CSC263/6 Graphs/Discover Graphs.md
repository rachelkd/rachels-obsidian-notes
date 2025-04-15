---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Graphs]
Week: 7
Module:
  - "[[6 - Graphs]]"
Lecture:
  - "13"
Chapter: 
Slides/Notes: 
Date: 2025-02-25
Date created: Tue., Feb. 25, 2025, 3:37:40 am
Date modified: Thu., Mar. 6, 2025, 10:40:59 pm
---

# Discover Graphs

- **Graphs**
    - Generalization of trees that can represent arbitrary (binary) ==relationships== between various objects
    - e.g., Modelling geographic and spatial relationships
- Examples of graphs:
    - Activity between members of a social network
    - Requirements and dependencies in a complex system like the development of a space shuttle

- & A graph is an abstraction of the concept of a **set of objects** and the **relationships** between them

## Definitions

- Define a **vertex** $v$ to be a single object
    - i.e., node
- Define an **edge** to be a tuple $e = (v_{1}, v_{2})$ or a set $e = \{ v_{1}, v_{2} \}$

> [!def]+ Graph
> A **graph** is a *tuple* of two sets $G = (V, E)$, where $V$ is the *set of vertices*, and $E$ is the *set of edges* on those vertices

- **Undirected graph**
    - Graph where order does not matter in edge tuples
    - i.e., $(v_{1}, v_{2})$ is equal to $(v_{2}, v_{1})$
    - % Sometimes see edge in an undirected graph expressed as a *set* $\{ v_{1}, v_{2} \}$
        - Since order does not matter
- **Directed graph**
    - Graph where the tuple order does matter
    - i.e., $(v_{1}, v_{2})$ and $(v_{2}, v_{1})$ represent different edges in a directed graph
- **Self-loop** in a directed graph
    - An edge going from a vertex to itself
    - ! Self-loops are not allowed in undirected graphs

![|300](https://i.imgur.com/srVbevN.png)

*Graph 1: An undirected graph with 5 vertices*

![|300](https://i.imgur.com/elUAKPP.png)

*Graph 2: A directed graph with 3 vertices*

- **Adjacent**
    - If $(u, v)$ is an edge in an *undirected* graph, then vertex $v$ is said to be **adjacent** to vertex $u$
        - This relationship is *symmetric* i.e., vertex $u$ is also adjacent to vertex $v$
    - In a *directed* graph, $v$ is **adjacent** to $u$ if there is an edge $(u, v)$
    - Graph 1:
        - $a$ is adjacent to $e$ and $c$
    - Graph 2:
        - $A$ is adjacent to $C$
        - $C$ is *==not==* adjacent to $A$

- **Incident on**
    - *Undirected* graph
    - An edge $(u, v)$ is **incident on** vertices $u$ and $v$
- **Incident from** and **incident to** (leaves and enters)
    - *Directed* graph
        - Terminology differentiates between the beginning and ending vertex of an edge
    - An edge $(u, v)$ which **leaves** vertex $u$ is said to be **incident from** vertex $u$ and is **incident to** (or **enters**) vertex $v$
    - Graph 2:
        - Edge $(C, A)$ is incident from $C$ and incident to $A$

- **Degree**
    - Undirected graph:
        - **Degree** of a vertex $v$ is the number of edges incident on $v$
    - Directed graph:
        - **In-degree** of vertex $v$ is the number of edges incident to $v$
        - **Out-degree** of vertex $v$ is the number of edges incident from $v$

- **Neighbourhood**
    - A **neighbourhood** of a particular vertex $v$ is the *set of vertices* that share an edge with $v$
    - Undirected graph:
        - Neighbourhood of vertex $v$ is the set of vertices $u$ such that $(u, v)$ is an edge
    - Directed graph:
        - **In-neighbourhood** of vertex $v$ is the set of vertices $u$ such that $(u, v)$ is an edge
        - **Out-neighbourhood** of vertex $v$ is the set of vertices $u$ such that $(v, u)$ is an edge
    - Graph 2:
        - Out-degree of $C$ is 2
        - In-neighbourhood of $C$ is $\{ C \}$

- **Weighted graph**
    - We associate an additional real number with each edge
    - Call this value the edge **weight** or the edge **cost**
    - Can be either directed or undirected

![|400](https://i.imgur.com/98ij3Tu.png)

*Graph 3: Weighted undirected graph*

### Path and Distance

- **Path**
    - *Sequence* of edges connected to each other
    - $(v, u_{1}), (u_{1}, u_{2}), \dots, (u_{k - 1}, x)$
- **Length** of a path
    - Number of **edges** on the path
- **Simple path**
    - Has no repeated edge or vertex
- Examples:
    - $(p, s), (s, t), (t, q)$ is a path in Graph 3 with length 3
    - $(p,s), (s,t), (t,r), (r,p), (p,s)$ is ==not== a simple path, but has length 5
    - Path $(p, s), (s, p)$ has length 2 and is also not simple

- **Distance** between two vertices/nodes
    - In an undirected graph:
        - Number of edges in the shortest path connecting them
        - Essentially representing the minimum number of “hops” required to travel from one vertex to the other on the graph
    - In a directed graph:
        - Sum of the weights of the edges on the shortest path connecting them

### Cycles

- **Cycle**
    - A path with the same starting and ending vertex
    - Undirected graph:
        - Path forming a cycle must have no repeated edges
        - Therefore must contain at least 3 vertices
    - Directed graph:
        - Cycle must have at least 1 edge
        - Self-loop is a cycle of length 1
- **Simple cycle**
    - Cycle that has no repeated edge or vertex (other than the same first and last vertex)
    - Examples:
        - Graph 3 $(r,t), (t,q), (q,r)$ is a simple cycle
        - Graph 2 $(A,B), (B,A)$ is a simple cycle
        - Graph 2 $(C, C)$ is a simple cycle of length 1
- **Acyclic**
    - A graph is **acyclic** if it contains no cycles

### Connectedness

- **Connected**
    - An *undirected* graph is **connected** if it contains a path between any two vertices
    - Graph 4 has 8 vertices and 5 edges
        - It is not connected
        - Has three components, each of which is connected
        - Would say that the graph has three *connected components*

![|400](https://i.imgur.com/vYT4Sk8.png)

*Graph 4: Graph with three connected components*

### Trees and Forests

- **Tree**
    - A **tree** is an *undirected connected* graph that is *acyclic*
    - % Definition of tree is different from what we have seen before, which have all been **rooted** trees
- **Rooted tree**
    - One vertex is designated as the root
- **Free tree**
    - An undirected connected acyclic graph without a specific vertex designated as the root

- **Forest**
    - A collection of (zero or more) disjoint trees
    - e.g., Graph 4 is a forest that comprises 3 trees

### Complete Graphs

- **Complete graph** $K_{n}$
    - Undirected graph on $n$ vertices in which every pair of vertices is joined by an edge
    - $m = |E| = \frac{n(n-1)}{2} = \sum\limits_{i=1}^{n-1}i = (n-1)+\dots+1$
    - See [[Graph Representations#Minimum and Maximum Number of Edges in an Undirected Connected Graph]]
