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
Chapter: "12"
Slides/Notes:
  - https://ebookcentral-proquest-com.myaccess.library.utoronto.ca/lib/utoronto/reader.action?docID=3339142&ppg=312
Date: 
Date created: Mon., Jan. 20, 2025, 12:43:54 am
Date modified: Tue., Jan. 28, 2025, 2:14:07 pm
---

# Binary Search Trees

- Basic operations on a binary search tree take time proportional to the height of the tree
- For a complete binary tree with $n$ nodes:
    - Such operations run in ==$\Theta(\lg n)$ worst-case== time
- If tree is a linear chain of $n$ nodes:
    - Same operations take $\Theta(n)$ worst-case time

## What is a Binary Search Tree?

> [!tldr] Section 12.1

- A **binary search tree** (BST) is a more organized binary tree

> [!def]+ Binary search tree
> - A tree where in each node contains:
>     - A ==key value== by which it can be searched,
>     - Additional satellite data, and
>     - Pointers *left*, *right*, and *p*, which
>         - Point to the left child, right child, and parent node, respectively
> - Pointers may be populated
>     - Otherwise, have value `NIL`
> - The **root** of a BST
>     - Top node from which all nodes are descended
> - Keys are stored in such a way that satisfy the **binary search tree property**

> [!def]+ Binary search tree property
> Let $x$ be a node in the binary search tree.
> - If $y$ is a node in the left subtree of $x$, then
>     - $y.key \leq x.key$
> - If $y$ is a node in the right subtree of $x$, then
>     - $y.key \geq x.key$

### Tree Traversal

> [!summary]+ Three ways to walk or traverse binary search trees
>
> > [!def]+ Inorder tree walk
>  > - Traverse left subtree, visit current node, then traverse right subtree
>  > - Prints out all keys in sorted order
>  > - Traversing every node takes $\Theta(n)$ time when starting from root
> >
>  >```
>  > Inorder-Tree-Walk(x)
>  >      if x != NIL
>  >          Inorder-Tree-Walk(x.left)
>  >          print x.key
>  >          Inorder-Tree-Walk(x.right)
>  > ```
>
> > [!def]+ Preorder tree walk
>  > - Visit current node, traverse left subtree, and traverse right subtree
>  >```
>  > Preorder-Tree-Walk(x)
>  >      if x != NIL
>  >          print x.key
>  >          Preorder-Tree-Walk(x.left)
>  >          Preorder-Tree-Walk(x.right)
>  > ```
>
> > [!def]+ Postorder tree walk
>  > - Traverse left subtree, traverse right subtree, then visit current node
>  >```
>  > Postorder-Tree-Walk(x)
>  >      if x != NIL
>  >          Postorder-Tree-Walk(x.left)
>  >          Postorder-Tree-Walk(x.right)
>  >          print x.key
>  > ```

> [!thm]+ Theorem 12.1
> If $x$ is the root of an $n$-node subtree, then `Inorder-Tree-Walk(x)` takes $\Theta(n)$ time.

*Proof.* (p. 315)

Let $T(n)$ denote the time taken by `Inorder-Tree-Walk` when it is called on the root of an $n$-node subtree.

- Since `Inorder-Tree-Walk` visits all $n$ nodes of the subtree, we have $T(n) = \Omega(n)$.

It remains to show that $T(n) = \mathcal{O}(n)$.

- Since `Inorder-Tree-Walk` takes a small, constant amount of time on an empty subtree (for test $x \neq \texttt{NIL}$), we have $T(0) = c$ for some constant $c > 0$.
- For $n>0$, suppose that `Inorder-Tree-Walk` is called on a node $x$ whose left subtree has $k$ nodes and whose right subtree has $n-k-1$ nodes.
    - The time to perform `Inorder-Tree-Walk` is bounded by $T(n) \leq T(k) + T(n - k - 1) + d$ for some constant $d > 0$
        - That reflects an upper bound on the time to execute the body of `Inorder-Tree-Walk(x)`
        - Exclusive of the time spent in recursive calls
    - We use the substitution method to show that $T(n) = \mathcal{O}(n)$ by proving that $T(n) \leq (c+d)n + c$
        - For $n = 0$, we have $(c + d) \cdot 0 + c = c = T(0)$
        - For $n>0$, we have
          $$
            \begin{align}
            T(n) & \leq T(k) + T(n-k-1) + d \\
             & \leq \left( (c+d)k + c \right) + \left( (c+d)(n-k-1)+c \right) +d \\
             & = (c+d)n + c-(c+d) + c + d \\
             & = (c+d)n + c
            \end{align}
            $$

## Querying a Binary Search Tree

> [!tldr] Section 12.2

![](https://i.imgur.com/sZNI8PV.png)

### Search

- Given a key $k$, we must search the BST for a node with a key that matches $k$
- Always start at root
- If $k$ is less than $x\texttt{.key}$:
    - Go down the left subtree
- If $k$ is larger than $x\texttt{.key}$:
    - Go down right subtree
- If $k$ is equal to $x\texttt{.key}$:
    - Return a pointer to node $x$
- Repeat until we find matching key, otherwise return `NIL`

```
Tree-Search(x, k)
    if x == NIL or k == x.key
        return x
    if k < x.key
        return Tree-Search(x.left, k)
    else
        return Tree-Search(x.right, k)
```

^7977df

- The *iterative* version is more efficient on computers

```
Iterative-Tree-Search(x, k)
    while x != NIL and k != x.key
        if k < x.key
            x = x.left
        else
            x = x.right
    return x
```

### Minimum and Maximum

> [!obs]+ Minimum
> - Can always find an element in BST whose key is a minimum by following ==left== child pointers from root
>     - Until we encounter a `NIL`

```
Tree-Minimum(x)
    while x.left != NIL
        x = x.left
    return x
```

- Procedure returns a pointer to the ==minimum== element in the subtree rooted at a given node $x$
    - Assume $x$ is non-`NIL`
- BST guarantees that `Tree-Minimum` is correct
    - If node $x$ has no left subtree:
        - BST Property: Every key in the right subtree of $x$ is at least as large as $x\texttt{.key}$
        - → Minimum key in subtree rooted at $x$ is $x\texttt{.key}$
    - If node $x$ has a left subtree:
        - No key in right subtree is smaller than $x\texttt{.key}$
        - Every key in left subtree is not larger than $x\texttt{.key}$
        - → Minimum key in subtree rooted at $x$ resides in subtree rooted at $x\texttt{.left}$

```
Tree-Maximum(x)
    while x.right != NIL
        x = x.right
    return x
```

> [!info]+ Running times of `Tree-Minimum` and `Tree-Maximum`
> - $\mathcal{O}(h)$ time on a tree of height $h$

- Sequence of nodes encountered forms a simple path downward from the root

### Successor and Predecessor

- Sometimes, we want to find the node that comes before or after node $x$
    - Based on the **inorder tree walk**

> [!def]+ Successor
> - If all nodes are ==distinct==
>     - The **successor** of a node $x$ is the node with the smallest key *greater* than $x\texttt{.key}$
> - Regardless:
>     - The **successor** of a node is the next node visited in an **inorder tree walk**
>
> (Cormen et al., p. 291-2)

```
Tree-Successor(x)
    if x.right != NIL
        return Tree-Minimum(x.right)  // Leftmost node in right subtree
    else  // Find the lowest ancestor of x whose left child is an ancestor of x
        y = x.p
        while y != NIL and x == y.right
            x = y
            y = y.p
        return y
```

- Two cases to consider when searching for a *successor*
    - If $x$ has a right subtree $\implies$ Find minimum of that subtree
    - If no right subtree exists $\implies$ Find parent of the first node that is a left child
        - i.e., The lowest ancestor whose left child is also an ancestor

![|338](https://i.imgur.com/gzReFSH.png)

*The successor of node with key 15 is the node with key 17.*

![|338](https://i.imgur.com/fU5SiVk.png)

*Node with key 13 has no right subtree. Its successor is its lowest ancestor whose left child is also an ancestor.*

> [!def]+ Predecessor
> - The **predecessor** of a node $x$ is the node with the *largest* key smaller than $x\texttt{.key}$

```
Tree-Predecessor(x)
    if x.left != NIL
        return Tree-Maximum(x.left)
    else
        y = x.p
        while y != NIL and x == y.left
            x = y
            y = y.p
        return y
```

- Two cases to consider when searching for a *predecessor*
    - If $x$ has a left subtree $\implies$ Find the maximum of the left subtree
    - If no left subtree exists $\implies$ Find the parent of the first node that is a right child

## Insertion

- Insertion new node into BST $T$ starts with a ==binary search==
    - → Find where to insert node $z$
    - Can be left or right of some parent node $y$, or
    - Existing tree may be empty
- When we insert $z$:
    - $z.key$ is some value $v$
    - $z.left, z.right$ is NIL
- Procedure will take $\mathcal{O}(h)$ time for tree $T$ of height $h$

```
Tree-Insert(T, z)
    y = NIL
    x = T.root
    while x != NIL
        y = x
        if z.key < x.key
            x = x.left
        else
            x = x.right
    z.p = y
    if y == NIL
        T.root = z  // Tree T was empty
    elseif z.key < y.key
        y.left = z
    else
        y.right = z
```
