---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 
Module:
  - "[[3 - Dictionaries, AVL Trees]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 
Date created: Mon., Jan. 20, 2025, 2:21:41 am
Date modified: Sun., Jan. 26, 2025, 10:15:15 pm
---

# Trees

> [!tldr] Source: <https://www.cs.cornell.edu/courses/cs2112/2021fa/lectures/lecture.html?id=trees>

- Trees are *recursive* data structures
    - **Acyclic** graphs of node objects
        - Each node may have some attached information

## Height and Depth

- Each node in a tree has a **height** and **depth**

> [!def]+ Depth
> - **Depth** of a node is the ==length== of the (unique) *path* from the root down to that node

> [!def]+ Length of a path
> - Number of edges in the path

> [!def]+ Height
> - **Height** of a node is the length of the ==longest path== from that node to a leaf below it
> - Height of a tree is the height of its root
>     - Same as the depth of its deepest leaf

![](https://www.cs.cornell.edu/courses/cs2112/2021fa/lectures/trees/tree_anatomy.png)

> [!def]+ Complete binary tree of height $h$
> - Tree which all leaves are at equal depth $h$
> - All non-leaves have exactly two children
> - A complete binary tree has $1 + 2 + 4 + \dots + 2^{h} = 2^{h + 1} - 1$ nodes
