---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 8
Module:
  - "[[4 - Dictionaries, Hash Tables]]"
  - "[[6 - Graphs]]"
Lecture: 
Chapter: 
Slides/Notes: 
Date: 2025-03-07
Date created: Wed., Mar. 5, 2025, 2:54:32 pm
Date modified: Fri., Mar. 7, 2025, 4:31:00 pm
---

# Test 3 Review

## Demonstrate: Hashing

> [!question]- What is the load factor?
> $$\alpha = \frac{n}{m}$$
> where $n$ is the number of keys in our hash table, and $m$ is the number of slots/buckets in our hash table.
>
> - Also the *expected* number of elements in an individual bucket in the hash table under SUHA
> - Load factor does not have to be $\leq 1$

> [!question]- Sometimes we use hash tables instead of AVL trees to implement the dictionary ADT because:
> - Even though the worst-case running time of hash table operations are worse than that of [[Dictionaries and AVL Trees|AVL trees]], the average-case running time of hash table operations are much better than the running time of AVL trees

> [!question]+ Which of the following is *not* a situation where you would decide to use a balanced search tree instead of hashing to implement a dictionary?
> 1. When you care most about average-case performance on insert
> 2. When you care most about worst-case performance on search
> 3. When you need to augment the data structure to return a full list of all keys in the dictionary
> 4. When you need to be able to access elements in the order of the keys
>
> > [!check]- Solution
> > When you care most about average-case performance on insert

## Exam Repository

### December 2017

#### Consider an Empty Hash Table of Size $m > 3$. Suppose We Insert 3 Distinct Keys, $x_{1}, x_{2},$ and $x_{3}$, in Order, into This Hash Table Using Open Addressing with Linear Probing

(a) (2 points)
Under the uniform hashing assumption, what is the probability that $x_{1}$ and $x_{2}$ are inserted in adjacent locations, where we also consider slot 0 and slot $m - 1$ to be adjacent locations. Briefly justify your answer.

(b) (8 points)
Let $X$ be the number of locations considered when inserting $x_{3}$. Under the uniform hashing assumption, what is the expected value of $X$? Briefly justify your answer.

#### Graphs

(a) (5 points)
Consider a BFS tree of a connected, undirected graph $G = \langle V, E \rangle$.
Prove that if $\{ u, v \}$ is a cross edge, then $|u.d - v.d| \leq 1$, where $x.d$ is the depth/distance of node $x$ in the BFS tree.

(b) (5 points)
For all $n \geq 5$, give an example of a strongly connected directed graph $G = \langle V, E \rangle$ with $|V| = n$, a BFS tree of this graph, and a cross edge $(u, v) \in E$ such that $| u.d - v.d | = n - 3$.
Briefly justify your answer.

### December 2018

### Hashing

The load factor $\alpha$ of an open addressing hash table satisfied $\alpha \leq 1$. Explain why in one short sentence.

### Graphs

Consider a directed graph $G$ illustrated below on the left; and a DFS tree $T$ of $G$ illustrated below on the right.

Considering $T$, which edge(s) of $G$ is/are *back edge(s)*?

![](https://i.imgur.com/CyGPOZL.png)

A fire has started in the Hogwarts School of Witchcraft and Wizardry. The school has a fire suppression system but it needs to be activated manually using a key. Fortunately, aurors are here and they have a map of the school and the location of the key. The auror team needs to reach the key as fast as possible.

You are given the map of the building, the location of the key, and the location of an auror. Your job is to help the auror reaches the key as fast as possible by finding a shortest route from the location of the auror to the location of the key. For simplicity, assume that the auror can only move in horizontal and vertical directions: left, right, up, down. The length of a route is the number of moves between squares. Also assume that $n$ represents the number of squares in the map.

An example is given in the figure below:
- the thick lines are the walls of the school;
- K denotes the location of the key;
- A denotes the location of the auror;
- the dashed line denotes a route of length 12, which is not optimal;
- the number of squares is $n = 42$.

![|400](https://i.imgur.com/hL9qE5k.png)

Explain how to represent the task of finding a shortest route from the location of the auror to the location of the key as a graph problem by answering the following questions:

1. ? What do the vertices of your graph represent?
2. ? How many vertices does your graph have?
3. ? Which vertices have an edge between them?
4. ? How many edges *at most* does your graph have?
5. ? Restate this task in terms of your graph

Describe in clear and precise English how to efficiently solve the problem described in this question using one of the algorithms we studied in this course. That is:
- Given the locations of the key and the auror, and assuming that the auror can only move in horizontal and vertical directions, output the shortest rout between the key and the auror.
Name the algorithm that you are using and briefly explain how you use it. No implementation is required. Your algorithm can use any algorithms we studied in the course as a black-box, and you can refer to their worst-case time complexity as stated in lecture or in the textbook.

What is the worst-case running time of your algorithm? Briefly justify your answer.
