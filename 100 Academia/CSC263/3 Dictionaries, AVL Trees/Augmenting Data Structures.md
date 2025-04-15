---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 5
Module:
  - "[[3 - Dictionaries, AVL Trees]]"
Lecture:
  - "10"
  - "9"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/vk7Ek3weUGTUoAbt9uIbs9
Date: 2025-02-04
Date created: Thu., Feb. 6, 2025, 2:00:31 pm
Date modified: Thu., Mar. 6, 2025, 4:30:09 am
---

# Augmenting Data Structures

> [!idea]- Deliberate Compromise
> - Perfect balance is too expensive to maintain, and does not buy us anything asymptotically
>     - i.e., $\mathcal{O}(n)$
> - We *settle* for something *good enough* — e.g., BF -1 and 1 — so the AVL tree is almost balanced, but still $\mathcal{O}(\log n)$

> [!idea]- Selective Augmentation
> - Storing extra information on existing data structures extends possible operations with no additional run time
>     - e.g., BFs in AVL trees are just an augmentation of a BST

> [!def]+ Augmented data structure
> - Existing data structure modified to store additional information and/or perform additional operations

1. Choose data structure to implement
2. Determine the additional information
3. Check that additional information can be maintained during each original operation
4. Implement new operations

## Example: Ordered Sets

- **Rank**
    - Position within an ordered list
    - Typically from lowest to highest

New Operations:

- `RANK(k)`
    - Return rank of key `k`
- `SELECT(r)`
    - Return the key with rank `r`
- Also support `INSERT`, `DELETE`, and `SEARCH`

### Approach 1: AVL Tree without Modification

- Queries:
    - Carry out an in-order traversal counting as we go, until we reach the rank or key we want

![|235](https://i.imgur.com/qaIJEQ3.png)

- ? What will be the time for the new queries?
    - $\Theta(n)$
        - $\mathcal{O}(n)$: There are only $n$ elements
        - $\Omega(n)$: Querying for the last item
- ? Will the other operations take any longer?
    - i.e., `SEARCH`, `INSERT`, `DELETE`
    - No; still $\Theta(\log n)$
    - No new information

> [!goal] Can we have a better query time?

### Approach 2: Augment AVL Tree

> [!impl] Augment AVL tree, so each node has an additional field `rank[x]` that stores its rank in the tree

![|400](https://i.imgur.com/Fy8hEDX.png)

- `RANK(14)`
    - Travels down the height of the tree → $\Theta(\log n)$
- `SELECT(4)`
    - $\Theta(\log n)$

<!-- break -->
- ? What will be the time for the new queries?
    - $\Theta(\log n)$
- ? Will the other operations take any longer?
    - `SEARCH`, `INSERT`, `DELETE` take $\Theta(n)$
    - $\because$ Have to update rank of all nodes

### Approach 3: Store Subtree Size

> [!impl] Augment an AVL tree, so each node $n$ has an additional field `n.size()` that stores the number of keys in the subtree rooted at $n$, including $n$ itself

![|300](https://i.imgur.com/2C81ROk.png) ![|300](https://i.imgur.com/DQPSSJn.png)

![|500](https://i.imgur.com/Qq3h9aZ.png)

- ? How do we quickly find the rank of the root of the tree?
    - `root.left.size + 1`
- ? How do we quickly find the local i.e., relative rank of a node $m$ in the tree rooted at $m$
    - `m.left.size + 1`

#### Identify the Local (Relative) Rank

##### `SELECT(7)`

![](https://i.imgur.com/Q6jwoHT.png)

- Since the subtree rooted at $h$ has rank 8, we know rank 7 has to be in the subtree

##### `SELECT(15)`

![](https://i.imgur.com/3DexduP.png)

- The rank at node $o$ is $13 + \texttt{o.left.size} + 1 = 15$

## Implement `SELECT`

![|300](https://i.imgur.com/Mv6bnyC.png)

### Trace `SELECT(T, 5)` on This Tree $T$

- At node $F$:
    - Rank is $5 + 1 = 6 > 5$ → Go left
- At node $D$:
    - Rank is $3 + 1 = 4 < 5$ → Go into right subtree
- At node $E$:
    - Rank is $4 + \texttt{n.left.size} + 1 = 4 + 0 + 1 = 5$, where $n$ is the node rooted at $E$
    - Return key for node $E$

### Trace `SELECT(T, 7)`

### Pseudocode

```pseudocode
SELECT(T, k):
    if T.left != NULL:
        local_rank = T.left.size + 1
    else:
        local_rank = 1
    
    if local_rank == r:
        return T.root
    if r < local_rank:
        return SELECT(T.left, r)
    else:
        return SELECT(T.right, r - local_rank)
```

## Implement `RANK`

> [!impl] Implementation
> - Perform `SEARCH` on $k$ and keep track of the current rank.
> - Each time you go down a level by going right, add the size of the subtrees to the left that you skipped.
>     - This is the local rank of the parent $p$ you just left in the subtree rooted at $p$.
> - Keep track of accumulating ranks.

### Trace `RANK(T, B)`

![|423x342](https://i.imgur.com/y12Ddwk.png)

### Trace `RANK(T, H)`

![|572x439](https://i.imgur.com/r68aMRT.png)

- At root node $F$:
    - Go right $\because H > F$
    - `skipped_nodes = left.size + 1`
- At node $I$:
    - Go left $\because H < I$
    - `skipped_nodes` did not increase because we went left
- At node $G$:
    - Go right $\because H > G$
    - Add local rank of node $G$ to `skipped_nodes`, so `skipped_nodes = 6 + local_rank(G) = 7`
- At node $H$:
    - Node found!
    - Add local rank of node $H$ to `skipped_nodes`, so `skipped_nodes = 7 + 1 = 8`

### Questions

> [!question]- How do we calculate the local rank of a node $m$ in the tree rooted at $m$?
> - `m.left.size + 1`

> [!question]- When we recurse on the left subtree, what do we add?
> - 0

> [!question]- When we recurse on the right subtree, what do we add?
> - Local rank of parent i.e., `left.size` and $+ 1$

> [!question]- When we find $x$, how do we determine its true rank?
> - `skipped_nodes + local_rank`

### Pseudocode

```python
RANK(T, k):
    return TREE-RANK(T.root, k, 0)

TREE-RANK(root, k, skipped_nodes):
    if root is NIL:  # k not in S
        pass
    
    # Determine local_rank in the tree rooted at root
    if root.left == null:
        local_rank = 1
    else:
        local_rank = root.left.size + 1
    
    if k < root.item.key:
        # Move left; do not deduct anything
        return TREE-RANK(root.left, k, skipped_nodes)
    else if k > root.item.key:
        # Move right; add for items/nodes skipped
        return TREE-RANK(root.right, k, local_rank + skipped_nodes)
    else:  # k == root.item.key
        # Since we have found the item, return our accumulated current rank
        return local_rank + skipped_nodes
```

> [!obs]+ We pass `skipped_nodes` into recursive call on left subtree
> - Just because we are going left now does not mean that we have not previously gone right!
> - Pass `skipped_nodes`, not 0

> [!note] Key `k` that we are searching for does not change in other calls

### Runtime Analysis

> [!question]+ What is the time for the new queries, `SELECT` and `RANK`?
> - Complexity of `Tree-Rank`: $\mathcal{O}(\log n)$
>     - Do at most one recursive call → Height of tree goes down 1
>     - Height of tree is $\Theta(\log n) \because$ tree is AVL-balanced

> [!question]+ What about updates of new information in old operations?
> - Search:
>     - [p] Stays the same
> - Insertion:
>     - We want to make sure that you can keep the $\log n$ time
>         - [I] Want to be able to know something about the left/right subtree and cut that part out to achieve $\log n$ time
>     - Can update sizes on insert by looking at sizes of children
>         - $\therefore$ Constant time to do one insertion
>     - ![|495x205](https://i.imgur.com/rWzRXUu.png)
>     - Only the orange nodes change

## Takeaway

Augmenting an AVL tree with size of subtree rooted at $m$ allowed us to efficiently implement `RANK` and `SELECT`.

> [!summary]+ General strategy
> - Use known data structure with extra information
> - Extra information must be maintained on original operations

## Overlapping Intervals

We have a set of **events** each of which has a *start time* and an *end time*.
We need the following operations and would like them all in $\mathcal{O}(\log n)$ time where $n$ is the number of items in the collection:

- `INSERT(event)`
    - Insert a new event into the collection
- `OVERLAPS(event)`
    - Return an existing event from the collection that overlaps with this event, or None if no such event exists
- `SEARCH(event)`
    - Find this event in the collection
- `DELETE(event)`
    - Remove this event from the collection

> [!tip] When we see $\log n$ time, we should think AVL trees

### How Do We Know Intervals Overlap?

![](https://i.imgur.com/1MQB5vi.png)

- This is messy

Rather, it is easier to ask:

> [!question]+ How do we know intervals do not overlap?
> 1. All blue goes completely *after* all orange, or
> 2. All blue goes completely *before* all orange
>
> ![|640x242](https://i.imgur.com/eNyFAWJ.png)

### How Could We Use an AVL Tree to Do This?

> [!impl]+ Implementation
> 1. Order the tree by interval start time
> 2. Augment each node with the ==maximum end time== for an interval in the **subtree**

![|631x241](https://i.imgur.com/pJnaKo1.png)

![|566x261](https://i.imgur.com/z52MqPY.png)

- Augment the tree with the maximum end time in the subtree

> [!question]+ What do the augmentations represent in this picture?
> ![|818x345](https://i.imgur.com/GiEKxj2.png)

> [!question]+ Given an interval $[x, y]$, what happens if $x > 22$?
> - No possible overlap

> [!question]+ What if $x > 5$?
> - Could be overlap, but not in the left subtree

> [!important]+ Key idea
> - Know that if `root.left.max < x`, then there is no possibility of overlap in the left subtree
>     - → Must go right
> - ? What if `root.left.max >= x`?

Consider looking for $[0, 0.5]$ or $[3.5, 3.7]$ in the left subtree.

![](https://i.imgur.com/R82gZjK.png)

> [!danger]+ Problem
> Could we go left, then discover that there is no overlap in the left subtree when there actually is one in the right?

> [!thm]+ Claim
> If we go left, and there is no overlap in the left subtree, there could not have been an overlap in the right subtree.

**Proof.**

Consider tree $T$ with left subtree $T_{1}$ and right subtree $T_{2}$.
We are checking for overlap with interval $[x, y]$, and $T.left.max \geq x$, so we went left.

Assume:
1. There exists an element $[x', y']$ in $T_{2}$ that overlaps $[x, y]$, and
2. There is no element in $T_{1}$ that overlaps $[x, y]$

- ~ Look for a contradiction

- Since $T.left.max \geq x$, there exists some element in $T_{1}$, $[a, b]$, where $b \geq x$ by $(2)$ and $[a, b]$ does not overlap $[x, y]$.
- $ Since $[a, b]$ does not overlap $[x, y]$, then $[a, b]$ must come completely after $[x, y]$
    - In particular, $a > y$
- By BST order, all keys in $T_{2} \geq$ all keys in $T_{1}$
    - $\implies x' \geq a > y$
- But $[x', y']$ overlaps $[x, y]$, so $x' \leq y$
- Thus, we have a contradiction

Therefore, $[x', y']$ cannot overlap $[x, y]$, and has to come completely after.

![|500](https://i.imgur.com/qIuTbdA.png)
