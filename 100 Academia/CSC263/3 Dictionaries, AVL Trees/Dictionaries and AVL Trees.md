---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [AVL trees]
Week: 3
Module:
  - "[[3 - Dictionaries, AVL Trees]]"
Lecture:
  - "5"
  - "6"
  - "7"
  - "8"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/IkjmCCOCfCz4E68WbRU3Ne
Date: 2025-01-21
Date created: Tue., Jan. 21, 2025, 2:18:43 pm
Date modified: Wed., Apr. 16, 2025, 8:20:17 pm
---

# Dictionaries and AVL Trees

## Dictionary ADT

- **Dictionary**
    - A collection of key-value pairs that supports the following operations

> [!def]+ Dictionary ADT
>
> - `Insert(S, x)`
>     - Insert both the ==key== and ==value== pair into the dictionary
> - `Search(S, k)`
>     - Return the value corresponding to a given key in the dictionary
>     - Just the key
> - `Delete(S, x)`
>     - Assume that we can directly access the element in the data structure i.e., we have searched for it already
>     - Then remove that element

^cf3316

- Something about ==incremental costs==?

> [!summary]+ Different implementations of dictionaries
>
> 1. Unsorted array
> 2. Sorted array
> 3. Unsorted linked list
> 4. Sorted linked list
> 5. Direct-access table
> 6. Hash table
> 7. Binary search tree
> 8. Balance search tree

### Approach 1: An Unsorted Array

![](https://i.imgur.com/2gR2lPi.png)

- Search:
    - $\mathcal{O}$: Go through every element at most
    - $\Omega$: Case where key is not in the dictionary or is the last element
    - $\implies \Theta(n)$
- Insert:
    - Know the size of the dictionary, but
    - Need to search if key already exists in dictionary first
    - & Times for `Insert` and `Delete` are *in addition* to time to search for the element

| Data Structure | Search      | Insert                                                        | Delete                                                                  |
| -------------- | ----------- | ------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Unsorted array | $\Theta(n)$ | $\Theta(n) + \Theta(1)$<br><br>(Search if key already exists) | $\Theta(n) + \Theta(1)$<br><br>(Search for key to get pointer to value) |

### Approaches 2-4

> [!important] Times for `Insert` and `Delete` are ==in addition== to time to *search* for the element.

- % Sorted array:
    - `Search`
        - *Binary search*
    - `Insert`
        - $\Omega$: When we are inserting the smallest element
    - `Delete`
        - At most move every element to the left
        - $\Omega$: Removing the first element → Everything shifts to the left
- % Unsorted linked list:
    - `Insert`
        - Insert at the beginning of the linked list
- % Sorted linked list
    - `Search`
        - Binary search does not work with linked lists
    - `Insert`
        - If you know where predecessor is, then inserting is easy
        - Same with `Delete`

| Data Structure       | Search           | Insert        | Delete                                  |
| -------------------- | ---------------- | ------------- | --------------------------------------- |
| Sorted array         | $\Theta(\log n)$ | $+ \Theta(n)$ | $+\Theta(n)$                            |
| Unsorted linked list | $\Theta(n)$      | $+\Theta(1)$  | $+ \Theta(1)$<br><br>(If no duplicates) |
| Sorted linked list   | $\Theta(n)$      | $+ \Theta(1)$ | $+ \Theta(1)$                           |

### Approach 5: A Direct-Access Table

![](https://i.imgur.com/FiLPE0o.png)

| Data Structure      | Search      | Insert      | Delete      |
| ------------------- | ----------- | ----------- | ----------- |
| Direct-access table | $\Theta(1)$ | $\Theta(1)$ | $\Theta(1)$ |

- Approaches 1-5 take $\mathcal{O}(n)$ space
- & Direct-access tables need one location per possible key
    - Suppose keys are 32-bit integers
        - → Max integer value is $2^{32}$
    - Then, we have $\Omega(2^{32})$ space

### Approach 6: Hash Table

- & *Map* all possible keys onto a ==smaller set== of actual keys from which you do direct access

![](https://i.imgur.com/fF1bsmF.png)

| Data Structure | Search       | Insert       | Delete       |
| -------------- | ------------ | ------------ | ------------ |
| Hash table     | $\Theta(1)$? | $\Theta(1)$? | $\Theta(1)$? |

> [!attention]+ There is a chance that something may map to the same thing.
>
> - **Collision**
> - ? How do we handle collision?
>     - Take all values in collusion and put it in a linked list
>     - Then, search the linked list
> - Worse case could be:
>     - Search in $\Theta(n)$
>     - Insert in $+\Theta(1)$
>     - Delete in $+\Theta(1)$

### Approach 7: Binary Search Tree

- Can implement Dictionary ADT assuming keys can be ordered

```python title:"BST Implementation of Dictionary ADT"
def Search(D, key):
    # Return the value in <D> corresponding to <key>, or None if key does not appear
    if D is empty:
        return None
    else if D.root.key == key:
        return D.root.value
    else if D.root.key > key:
        return Search(D.left, key)
    else:
        return Search(D.right, key)


def Insert(D, key, value):
    if D is empty:
        D.root.key = key
        D.root.value = value
    else if D.root.key >= key:
        Insert(D.left, key, value)
    else:
        Insert(D.right, key, value)


def Delete(D, key):
    if D is empty:
        pass  # Do nothing
    else if D.root.key == key:
        D.root = ExtractMax(D.left) or ExtractMin(D.right)  # Get predecessor or successor
    else if D.root.key > key:
        Delete(D.left, key)
    else:
        Delete(D.right, key)
```

- All three algorithms are *recursive*
    - In each one, cost of the *non-recursive* part is $\Theta(1)$
        - Simply some comparisons, attribute access/modifications
- → Each algorithm has running time proportional to number of recursive calls made
    - Each recursive call is made on a tree of height one less than its parent call
    - → Worst-case the number of recursive calls is $h$
        - $h$ is the height of the BST
    - → Upper bound on the worst-case running time of each of these algorithms is $\mathcal{O}(h)$
        - (Can show that bound is tight)
- A binary tree of height $h$ can have:
    - $h$ to $2^{h+1} - 1$ nodes
    - → A tree of $n$ nodes can have height $n$
    - → Worst-case running time of $\mathcal{O}(n)$ for all three algorithms
        - (Can show that bound is tight)
- The best case for the height of a tree of $n$ nodes is $\log n$
    - Would be pretty unlucky to get trees of height $n$

> [!important]+ Motivation
>
> - Implement insertion and deletion to keep height small when adding or removing values
> - ? Can we implement BST insertion and deletion to not only insert/remoove a key, but also keep the tree’s height (relatively) small?

## AVL Trees

### Introduction

> [!tldr] Notes on AVL Trees (Hadzilacos)

> [!warning]+ Binary search trees work well in the average case, but still have the drawback of ==linear== worst case time complexity for all three operations.
>
> - i.e., `Search`, `Insert`, and `Delete`

> [!def]+ Ideally Height-Balanced Property
>
> - A binary tree of height $h$ is **ideally height-balanced** if
>     - Every node of depth $< h-1$ has two children

- If we could keep BST ideally height-balanced at all times, then:
    - Tree of $n$ nodes would be guaranteed to have height $h = \lceil \lg n \rceil$
    - Searches would always take time in $\mathcal{O}(\log n)$
- But insertions and deletions might destroy the *ideally height-balanced property*
    - Reorganization to restore property in addition to BST property might take $\mathcal{O}(n)$ worst-case time
- & AVL (height-balanced) trees are a *compromise* between arbitrary BSTs and ideally height-balanced BSTs.

> [!def]+ Height-Balance Property
>
> - A binary tree is **height-balanced** if:
>     - Heights of the left and right subtrees of every node differ by ==at most one==

> [!def]+ AVL Tree
>
> - An **AVL tree** is a height-balanced ==binary search tree==

- & Height of an *empty* binary tree (with no nodes) is ==$-1$==
    - Height of a tree consisting of a single node is $0$

![|300](https://i.imgur.com/VozRfr4.png)

![|400](https://i.imgur.com/FJQbT96.png)

> [!check]+ Good news
>
> - Worst case height of AVL tree with $n$ nodes is:
>     - & $1.44 \log_{2}(n+2)$
>         - i.e., $h \leq 1.44 \log_{2} (n + 2)$
>     - → `Search` operation can be carried out in $\mathcal{O}(\log n)$ time in the worst case
> - Insertions and deletions can be done in $\mathcal{O}(\log n)$ time
>     - While preserving the AVL-ness of tree
> - AVL trees work very well on the average case too

- [c] Algorithms for insertion and deletion are complex

#### Balance Factor

> [!def]+ Balance factor
>
> - Let $h_{R}, h_{L}$ be the heights of the right and left subtrees of a node $m$ in a binary tree respectively.
> - The **balance factor** of $m$, $BF[m]$, is defined as $$BF[m] = h_{R} - h_{L}$$
>
> > [!example]-
> > ![|400](https://i.imgur.com/0QRQF12.png)
>
> - For an AVL tree, the balance factor of any node is:
>     - $-1, 0,$ or $+1$
> - If $BF[m] = +1$, $m$ is *right-heavy*
> - If $BF[m] = -1$, $m$ is *left-heavy*
> - If $BF[m] = 0$, $m$ is *balanced*

> [!impl]+ In AVL trees, we will store $BF[m]$ in each node $m$
>
> - When we draw:
>     - Put a $+, -,$ or $0$ next to each node
>     - Indicates node balance factor is $+1, -1,$ or $0$ respectively

> [!example]+ Which tree is height-balanced?
> ![](https://i.imgur.com/jYO8rXl.png)
>
> - Tree on left only

### Search

- Treat $T$ as an ordinary binary search tree

![[Binary Search Trees (CLRS)#^7977df]]

### Insertion

> [!impl]+ Insertion
>
> - Insert $x$ in $T$ as in ordinary BSTs

#### Three (Easier) Cases

![|300](https://i.imgur.com/PEtpDyU.png)

##### Insert 8: Insertion without Imbalance Increases Height

![|300](https://i.imgur.com/gMh5mmr.png)

- New tree is still AVL-balanced

##### Insert 5: Insertion without Imbalance Keeps Height Same

![|600](https://i.imgur.com/cO4DqKK.png)

- New tree is still AVL-balanced

##### Insert 10: Insertion with Imbalance Keeps Height Same

![|600](https://i.imgur.com/gDFxK85.png)

- Consider the *minimum height* ancestor of new leaf which is no longer height balanced which is no longer height balanced as a result of insertion
    - Node 7
- Need to do a *single left rotation* on node 7

![|500](https://i.imgur.com/SaI5l2z.png)

- Red BFs are unchanged
- $h = 3$
    - Managed to insert new element without adding much to the height

<!-- break -->
- Three cases:
    1. & Insertion without imbalance ==increases== tree height
    2. & Insertion without imbalance keeps tree height ==same==
    3. & Insertion with imbalance keeps the height ==same==
        - Always true!

#### Single Rotations

- Case 3 used a single left rotation on node 7

![](https://i.imgur.com/VKM2ll1.png)

> [!thm]+ Single Rotation Properties
>
> 1. Insertion only affects the balance factors of its *ancestors*
> 2. Root balance factor depends on $h(A), h(C), h(E)$
> 3. Overall height of rotated tree remains the same (as before insertion)
>     - Nothing beyond it is affected in terms of height or balance factors

^3c89d0

##### Insert 11

![|400](https://i.imgur.com/gLaTfXC.png)

- BF of node with 3 is unbalanced after insertion
    - → Need to rebalance subtree rooted at 3

![|400](https://i.imgur.com/s43uV2q.png)

- → Do a single left rotation on node 3

![|400](https://i.imgur.com/wOk6VDG.png)

- $ Height of subtree (rooted in the same position that node 3 was at before rotation) is still the same after rotation

#### Double Rotations

##### Insert 6

###### Attempt to Use a Single Rotation

![|400](https://i.imgur.com/gK22JLi.png)

- Node 3 is right-heavy → Do a single left rotation on node 3

![|400](https://i.imgur.com/T97R1KY.png)

- Node 7 is left-heavy → Do a single right rotation on node 7
    - Notice that the height of subtree also did not stay the same

![|400](https://i.imgur.com/fHTqThQ.png)

- Same tree that we started with after inserting 6

###### Use a Double Rotation

> [!warning] The root is ==left-heavy==, and the left ==child is right heavy==.

Consider this tree after the first single left rotation (on node 3).

![|400](https://i.imgur.com/wrGX8FX.png)

- Notice the *different signs* for the minimum height unbalanced ancestor of new node 6 and left child of such node
- Do a single left rotation on subtree rooted at left child (node 3)

![|400](https://i.imgur.com/0wtHXvy.png)

- Now the signs are the same
- → Do a single right rotation on node 7
    - i.e., Minimum height ancestor of new node 6 which is unbalanced

![|400](https://i.imgur.com/CFSYOuL.png)

#### Left Right Rotation

![](https://i.imgur.com/qCJmemN.png)

#### Rebalancing an AVL after Insertion

> [!info]+ The height-balance property of a node may have been destroyed as a result of the insertion of the new leaf in two ways:
>
> 1. New leaf increased height of right subtree of a node that was already right heavy
> 2. New leaf increased height of left subtree of a node that was already left heavy

Such cases are illustrated in Figure 1.

![|686](https://i.imgur.com/CaTXUMF.png)

> [!note]+
>
> - Insertion of new leaf can only affect balance factors of its ancestors.
> - Node $m$ in Figure 1 is assumed to be the *minimum height* ancestor of the new leaf which is no longer height balanced
>     - As a result of insertion

- & Since both cases are symmetric, we shall only consider case (1) in detail
    - Obtain other by changing every reference of “right” to “left”
    - Change “+” to “-”

- Both cases split into ==two== more cases:
    - One where a single rotation is sufficient
    - One where a double rotation is needed

![|626](https://i.imgur.com/uT6z44u.png)

##### Single Rotations

> [!question]+ What transformation can we do to rebalance subtree in Figure 2(a)?
>
> - **Single left rotation** on node $m$
>
> ![|800](https://i.imgur.com/DNWA0Gt.png)

> [!question]+ Transformation to rebalance Figure 1(b)
>
> - **Single right rotation**
>
> ![|700](https://i.imgur.com/RGxpV8H.png)

- $ Notice that the signs are the same for the unbalanced node and its left child for $--$ and right child for $++$

Recall:

![[#^3c89d0]]

> [!thm]+ More Single Rotation Properties
>
> 1. Transformation rebalances the subtree rooted at node $m$ (S.1)
>     - Subtree becomes height-balanced again
> 2. Maintains the binary search tree property (S.2)
> 3. Can be done in constant time (S.3)
>     - Only few pointers have to be switched around
> 4. Keeps the height $m$ equal to its height *before* the insertion of the new node (S.4)
>     - Height $h + 2$

##### Double Rotations

![|626](https://i.imgur.com/uT6z44u.png)

> [!danger]+ Subtree in Figure 2(b) cannot be rebalanced by a single left rotation on node $m$
>
> - Node $m$ is right-heavy
> - Right child of node $m$ is left-heavy

- Assume that subtrees of node $B$ in Figure 2(b) are nonempty
    - i.e., $h \neq -1$

> [!question]+ What transformation do we use to rebalance Figure 2(b)?
>
> - **Double right left rotation** on node $m$
>
> Figure 4(a) shows these subtrees in more detail.
>
> ![|800](https://i.imgur.com/dnrko7L.png)

- ? How do we obtain the double right left rotation transformation?
    - Rotate $B$ to the right
    - Then rotate $C$ left in resulting subtree
- Balance factors labeled as “$*/*$” depend on whether new node $x$ was inserted under $T_{21}$ or $T_{22}$
    - $T_{22}$ is the first entry of the label
    - $T_{21}$ is the second entry of the label

###### If Subtrees of Node $B$ Are Empty

If subtrees of node $B$ in Figure 2(b) are empty i.e., $h = -1$,

- → $A$ only has a right child $B$
    - $B$ only has a *left* child new node $x$
- After double right-left rotation:
    - $x$ is the root of the subtree
    - $A, B$ are its left and right children, respectively

> [!idea]+ Can be thought of a degenerate instance of Figure 4
>
> - $C = x$
> - Subtrees $T_{1}, T_{21}, T_{22}$ all empty

###### Double Rotation for Figure 1(b)

- Do a **double left-right rotation**

![|700](https://i.imgur.com/bTn1HUF.png)

###### Double Rotation Properties

> [!thm]+ Double Rotation Properties
>
> 1. It rebalances the subtree rooted at $m$ (D.1)
>     - Subtree becomes height-balanced again
> 2. Maintains the binary search tree property (D.2)
> 3. Can be done in constant time (D.3)
>     - Only have to change a few pointers
> 4. Keeps the height of $m$ equal to that node’s height *before* insertion of new node (D.4)
>     - Height $h + 2$

#### Updating the Balance Factors after Insertion

> [!obs] Observation
>
> - Only the balance factors of the new node’s ancestors may need updating
> - For any other node $i$, $i$‘s left and right subtrees — and their heights — have no changed
>     - → Balance factor of $i$ has not changed
> - & Not all of the new node’s ancestors’ balance factors may need updating
>     - Figure 7(a-b)

![](https://i.imgur.com/ci3JINx.png)

- Insertion of key 8 to the AVL tree in 7(a) results in AVL tree in 7(b)
    - $ Only BF of 9 (8‘s parent) has changed
- Insertion of key 8 to AVL tree in 7(c) results in 7(d)
    - BF of all of 9’s ancestors have changed

#### Insertion Algorithm

![](https://i.imgur.com/0LFpZBE.png)

```pseudocode
insert as normal BST

walk back from new node towards root:  # Assume i is the parent of the new node:
    if new node is inserted right, add 1 to BF[i]
    if new node is inserted left, subtract one from BF[i]

    if BF[i] becomes zero, then done
    if BF[i] becomes 1 or -1:
        don't rotate, but continue up the path and repeat
    if BF[i] becomes 2 or -2:
        do the rotation (R, L, LR, RL) and then done
```

![|484](https://i.imgur.com/aBkgzhm.png)

> [!question]+ How many rotations in the worst case?
>
> - 1 single or 1 double rotation

> [!important] Insertion takes $\log n$ worst-case time.

### Deletion

To delete a key $x$ from AVL tree $T$:

- Locate the node $n$ where $x$ is stored
    - Can be done using algorithm for `Search`
    - If no such node exists, we are done
        - Nothing to delete
- Otherwise, we have *three* cases

> [!summary]+ Three cases for tree deletion
>
> 1. $n$ is a leaf
> 2. $n$ is a node with only *one* child
> 3. $n$ has *two* children

#### Case 1. $n$ is a Leaf

- Simply remove it
- May cause tree to not be height-balanced
    - → May need to rebalance
- Also have to ==update== BFs of some nodes

#### Case 2. $n$ is a Node with only One Child

- Let $n'$ be $n$‘s only child
    - % $n'$ must be a leaf
        - Otherwise, subtree rooted at $n$ would not have been height-balanced before deletion
- Copy the key stored at $n'$ into $n$
- Remove $n'$ as in case (1)
    - $\because$ It must be a leaf (as we just argued)

#### Case 3. $n$ Has Two Children

- Find the smallest key in $n$’s right subtree
    - Key is the smallest key in $T$ larger than the key stored in $n$
        - By BST property
    - To find this key:
        - Go to $n$‘s right child
        - Follow the longest chain of left child pointers until we get to node $n'$ that has no left child
- Copy key stored in $n'$
- Remove $n'$ from the tree
    - As in Case (1), if $n'$ does not have a right child either, or
    - As in Case (2), if $n'$ has only a right child

#### Rebalancing an AVL Tree after Deleting a Leaf

- Deletion of a leaf $n$ will cause the tree to become unbalanced in one of two cases:
    - & It reduces the height of the right subtree of a left heavy node; or,
    - & It reduces the height of the left subtree of a right heavy node
- Both cases are *symmetric*

![|859](https://i.imgur.com/ukET2lg.png)

Consider case (a).

> [!info] There are ==two== ways case (a) could arise.
>
> 1. If the balance factor of $B$ is 0 or -1
>     - i.e., Height of $T_{2}$ is $h + 1$ or $h$
>     - After rotation: Balance factors of $B$ and $A$ will be $+1$ or $0$, and $-1$ or $0$
>
> ![|500](https://i.imgur.com/sHjVqM5.png)
>
> 2. If the balance factor of $B$ is +1
>     - i.e., $A$ is *left*-heavy, and $B$ is *right*-heavy
>
> ![|500](https://i.imgur.com/ZvtKZTk.png)
>
> - % Balance factors of nodes that have a label of the form “$*/*/*$” depend on the heights of $T_{21}, T_{22}$
>     - Note that *at least one* of these subtrees must have height $h$
>         - Other one can have height $h - 1$ or $h$
>     1. First entry of label indicates: Balance factor in the event $T_{21}$ has height $h - 1$ and $T_{22}$ has height $h$
>     2. Second: BF in the event both trees have height $h$
>     3. Third: BF when $T_{21}$ has height $h$ and $T_{22}$ has height $h - 1$

> [!summary]+ Deletion Rebalance Properties
>
> 1. They rebalance the subtree rooted at $m$.
>     - So the subtree becomes height-balanced again
> 2. They maintain the BST property.
> 3. They can be done in ==constant== time by simply manipulating a few pointers.
> 4. They *may decrease* the height of the subtree rooted at $m$, compared to the height of the subtree before the deletion.

- In insertion:
    - One rotation always rebalances the subtree
    - By maintaining height of that subtree, it rebalances the entire tree
- In deletion:
    - Rotation balances the subtree, but
    - ! Height may decrease
        - Balance factor of nodes higher up (closer to root) may change as a result
        - May have to go on rotating subtrees all the way up to the root in order to rebalance entire tree
    - & May have to do as many as $\mathcal{O}(\log n)$ rotations
        - This is acceptable, because one rotation only takes constant time

#### Deletion Algorithm

```pseudocode
find node p to delete

on route from p back to route at each node i:
    if BF(i) was previously 0:
        update BF and exit
    if BF(i) was previously +1 or -1:
        if it becomes 0 after deletion, then we shortened tree rooted at i, so continue up path
    if BF(i) was previously +1 or -1 and change makes it +2 or -2:
        do appropriate rotation
        if rotation shortened tree rooted at i, then continue up tree
        if not, stop
```

> [!question]+ How many rotations in the worst case?
>
> - $\mathcal{O}(h) = \mathcal{O}(\log n)$
