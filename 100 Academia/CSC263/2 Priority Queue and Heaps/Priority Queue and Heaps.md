---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: [Heaps, Heapsort, Priority queues]
Week: 2
Module:
  - 2 - Priority Queue and Heaps
Lecture:
  - "3"
  - "4"
Chapter: "2"
Slides/Notes:
  - https://share.goodnotes.com/s/ssWwm4WeJJAFYvYlfaqEat
  - zotero://open-pdf/library/items/V3PQA9XR?page=22&annotation=RM9M8TZ4
Date: 2025-01-14
Date created: Tue., Jan. 14, 2025, 2:12:42 pm
Date modified: Mon., Mar. 17, 2025, 2:54:22 pm
---

# Priority Queue and Heaps

- Similar to **stacks** and **queues**
- Supports adding, removing item from collection
    - Order in which items are removed does not depend on the order in which they are added
    - Depends on a *priority* specified when item is added
- Analogy: A hospital waiting room
    - More severe injuries and illnesses are generally treated before minor ones
    - Regardless of when patients arrived at hospital

> [!def]+ Priority Queue
>
> - Data:
>     - A collection of items which each have a ==priority==
>         - A value from some totally ordered domain — often an integer
> - Operations:
>     - `Insert(PQ, x, priority)`
>         - Add `x` with the given priority to `PQ`
>     - `FindMax(PQ)`
>         - Return an item in `PQ` with highest priority
>     - `ExtractMax(PQ)`
>         - Remove and return an item from `PQ` with highest priority

- Think of Insert as *enqueue* and ExtractMax as *dequeue*

> [!note]+ Insert operation does not check if an item or a priority is already in the collection!
>
> - FindMax and ExtractMax both talk about “an item”
>     - vs. “the item”
> - If PQ has multiple items with the highest priority:
>     - Operations only return ==one== of them
>     - ADT does not specify which one

- Theme in this course: Distinction between definition of an **ADT** and an implementation of that data type using a particular **data structure**
    - To emphasize this: we explore different implementations of the **Priority Queue ADT**
        - One that is correct, but inefficient
        - One that uses the **heap data structure**

<!-- break -->
- Example sequence used across various implementations:
    - IN(8), IN(5), IN(10), IN(3), FM(), IN(16), EM(), EM(), IN(7)
    - We are going to ==ignore== the data for operations
        - Only care about the priority in this case
    - i.e., Insert item with priority 8, insert item with priority 5

## Approaches

### Approach 0: An Unsorted Linked List

- & Insert in $\mathcal{O}(1)$
    - Add item and corresponding priority to front of the linked list
- & FindMax in $\mathcal{O}(n)$
    - Search through entire list
- & ExtractMax in $\mathcal{O}(n)$
    - Search through entire list again

![|500](https://i.imgur.com/x1zkNto.png)

```python title:"Unsorted Linked List Priority Queue"
def Insert(PQ, x, priority):
    n = Node(x, priority)
    oldHead = PQ.head
    n.next = oldHead
    PQ.head = n

def FindMax(PQ):
    n = PQ.head
    maxNode = None
    while n is not None:
        if maxNode is None or n.priority > maxNode.priority:
            maxNode = n
        n = n.next
    return maxNode.item

def ExtractMax(PQ):
    n = PQ.head
    prevNode = None
    maxNode = None
    prevMaxNode = None
    while n is not None:
        if maxNode is None or n.priority > maxNode.priority:
            maxNode, prevMaxNode = n, prevNode
        prevNode, n = n, n.next

    if prevMaxNode is None:
        # Set head to item after maxNode
        self.head = maxNode.next
    else:
        # Link the node before maxNode to the node after maxNode
        prevMaxNode.next = maxNode.next

    return maxNode.item
```

- This is a naive or *brute force* implementation
    - Using familiar primitive data structures e.g., arrays and linked lists
    - Quick to come up with
    - Analyzing running time of operations often straightforward
    - → Gives targets to beat:
        - Insert in $\Theta(1)$
        - FindMax and ExtractMax in $\Theta(n)$
- ? Can we do better using a more complex data structure?

### Approach 1: An Unsorted Array

> [!info] IN(8), IN(5), IN(10), IN(3), FM(), IN(16), EM(), EM(), IN(7)

- & Insertion is $\mathcal{O}(1)$
    - Keep track of size
        - Start with `size = 0`
    - Insert 8 → `size = 1`
    - Insert 5 → `size = 2`
    - Insert 10 → `size = 3`
    - Insert 3 → `size = 4`
- & FindMax is $\mathcal{O}(n)$
- & ExtractMax
    - Can either take out the max, and shift everything up in the array, or
        - $T(n) = \mathcal{O}(n^{2})$
    - Take the last element and put it in place for extracted max
        - $\mathcal{O}(n)$

![](https://i.imgur.com/2J9kGwm.png)

> [!note]+ Both Python and Java often hide the complexity of operations.
>
> - e.g., Appending an element into a Python list: `L.append(x)`
> - In [[CSC263]], we want to write and analyze algorithms from the simple operations that do not depend on hidden complexity

### Approach 2: A Sorted Array

> [!info] IN(8), IN(5), IN(10), IN(3), FM(), IN(16), EM(), EM(), IN(7)

![](https://i.imgur.com/41OxH93.png)

- `IN(16)`
    - Insert at the beginning
    - Shift all elements back one
    - & $\mathcal{O}(n)$
- `FM()`
    - First element in descending order
    - Last element in ascending order (and we know size)
- `EM()`
    - & $\mathcal{O}(n)$ if sorted in descending order
        - Need to shift everything up
    - & $\mathcal{O}(1)$ if sorted in ascending order

### Approach 3: An Ordered Linked List

> [!info] IN(8), IN(5), IN(10), IN(3), FM(), IN(16), EM(), EM(), IN(7)

- & Insert in $\mathcal{O}(n)$
- & FindMax in $\mathcal{O}(1)$
- & ExtractMax in $\mathcal{O}(1)$

![|372](https://i.imgur.com/9gRw6dd.png)

### Approach 4: A Binary Search Tree

> [!info] IN(8), IN(5), IN(10), IN(3), FM(), IN(16), EM(), EM(), IN(7)

![|358](https://i.imgur.com/akT7Fx3.png)

- Let $h$ be the *height* of the tree.
- & Insert in $\mathcal{O}(h)$
- & FindMax in $\mathcal{O}(h)$
- & ExtractMax in $\mathcal{O}(h)$
    - Need to find *predecessor* or *successor* for BST property invariant
        - [[Binary Search Trees (CLRS)#Successor and Predecessor]]

## Heaps

- Based on a **nearly complete binary tree**
- **Heap Property**
    - Determines relationship between values of parents and children
- Kind of sorted
    - Enough to make query operations fast while not requiring full sorting after each update
- Stored in an ==array==

> [!def]+ Heap
>
> - A **heap** is a binary tree that satisfied the *heap property* and is *nearly complete*

### Nearly Complete Binary Tree

> [!def]+ Nearly complete binary tree
>
> - Binary tree
> - Every row is completely filled except for possibly the lowest row
> - Lowest row is filled from the left

- Only 1 possible shape for heap of $n$ nodes
    - i.e., Exactly one way for creating a heap of $n$ nodes
    - → Do not need to use space to store references between nodes as we do in standard binary tree implementation
        - Fix a conventional ordering of nodes in the heap
        - Write down the items in heap according to **level order**
            - Orders elements based on their depth in the tree
            - First node is the root of the tree, then
            - Its children (at depth 1) in left-to-right order, then
            - All nodes at depth 2 in left-to-right order, etc.
- Height of tree with $n$ nodes is $\lceil \log n \rceil$

### Heap Property

> [!def]+ Heap Property
>
> - Two types of heap property
>
> > [!thm]- Max Heap
> >
> > - Value at every node is ==equal to or greater than== the value of its immediate children
>
> > [!thm]- Min Heap
> >
> > - Value at every node is ==equal to or less than== the value of its immediate children

### Array Indices

Assume that items are stored starting at index 1.

For a node corresponding to index $i$:

- Left child is stored at index $2i$
- Right child is stored at index $2i + 1$
- Parent when $i > 1$ is stored at index $\left\lfloor  \frac{i}{2}  \right\rfloor$

> [!obs] A node is a leaf when its left child index is larger than the heapsize.

### Heap Implementation of a Priority Queue

#### Find Maximum

- Root of heap is always the item with maximum priority
    - → Stored at front of array

```python
def FindMax(PQ):
    return PQ[1]
```

> [!note] Indexing starts at 1 rather than 0.

#### Extract Maximum (Remove)

![](https://discusschool.com/uploads/default/original/1X/52b62162e211a0e883c1b8f7b82d2e72f1a75600.gif)

- We obviously need to remove root of the tree

> [!question]+ What do we replace the root with?
>
> - Resulting heap must still be complete
> - Structure must change
>     - Very last (rightmost) leaf must no longer appear
> - & Save the root of the tree, and then replace it with the last leaf

> [!note]+ We can still access leaf in ==constant== time because we know the size of the heap
>
> - Last leaf is always at the end of the list of items

> [!obs]+ Last leaf priority is smaller than many other priorities
>
> - Cannot leave heap like this; violates heap property
> - → Repeatedly swap the moved value with one of its children until heap property is satisfied once more
>     - & Choose the *larger* of the two children to ensure that heap property is preserved

![|313](https://i.imgur.com/WsqCRTE.png)

```python
def ExtractMax(PQ):
    temp = PQ[1]  # Save max priority item to return at end
    PQ[1] = PQ[PQ.size]  # Replace root with last leaf
    PQ.size = PQ.size - 1

    # Bubble down
    i = 1
    while i * 2 <= PQ.size:  # We stop when there is no left child
        curr_p = PQ[i].priority
        left_p = PQ[2 * i].priority
        right_p = PQ[2 * i + 1].priority  # -inf if not exist

        # Heap property is satisfied
        if curr_p >= left_p and curr_p >= right_p:
            break
        # Left child has higher priority
        else if left_p >= right_p:
            PQ[i], PQ[2 * i] = PQ[2 * i], PQ[i]
            i = 2 * i
        # Right child has higher priority
        else:
            PQ[i], PQ[2 * i + 1] = PQ[2 * i + 1], PQ[i]
            i = 2 * i + 1
    return temp
```

> [!note] Bubble down is also called **max-heapify**.

> [!summary]+ Running time of `ExtractMax`
>
> - All individual lines of code take ==constant== time
>     - → Runtime is determined by number of loop iterations
> - At each iteration:
>     - Either loop stops immediately, or
>     - $i$ increases by at least a factor of 2
>     - → Total number of iterations is at most $\log n$ where $n$ is the number of items in the heap
> - & Worst-case running time of `ExtractMax` is $\mathcal{O}(\log n)$

#### Insert

![](https://discusschool.com/uploads/default/original/1X/cd71ed285f67de1c5a4d4b61bfe3e87dbc36cb7a.gif)

- The number of items in the heap completely determines its structure
- When adding a new item:
    - Will be a new leaf immediately to the right of the current final leaf
    - Corresponds to the next open position in the array after last item
- & Algorithm puts new item after last item, then *bubbles up*
    - Compares new item with its parent
    - Swaps if new item has a larger priority than parent’s priority

The following diagram shows the result of adding 35 to the given heap:

![|344](https://i.imgur.com/ApMegWD.png)

```python
def Insert(PQ, x, priority):
    PQ.size = PQ.size + 1
    PQ[PQ.size].item = x
    PQ[PQ.size].priority = priority

    i = PQ.size
    while i > 1:
        curr_p = PQ[i].priority
        parent_p = PQ[i // 2].priority

        if curr_p <= parent_p:  # Heap property satisfied
            break
        else:
            PQ[i], PQ[i // 2] = PQ[i // 2], PQ[i]
            i = i // 2
```

- Loop runs at most $\log n$ iterations where $n$ is the number of items in the heap
- Worst-case running time of this algorithm is $\mathcal{O}(\log n)$

#### Runtime Summary

| Operation    | Linked list (unsorted) | Heap             |
| ------------ | ---------------------- | ---------------- |
| `Insert`     | $\Theta(1)$            | $\Theta(\log n)$ |
| `FindMax`    | $\Theta(n)$            | $\Theta(1)$      |
| `ExtractMax` | $\Theta(n)$            | $\Theta(\log n)$ |

- Tradeoffs:
    - Heaps beat unsorted linked lists in two of the three priority queue operations
    - Heaps are asymptotically slower than unsorted linked lists when adding a new element
- The slowest operation for heaps run in $\Theta(\log n)$ time in the worst case
    - Substantially better than $\Theta(n)$ time for unsorted linked lists
- Reason for speed of heap operations is the two properties:
    - Heap property
    - Nearly completeness

### Max Heapify or Bubble Down

> [!def]+ Max heapify
>
> - Procedure that maintains the max heap property
> - Assumes ==binary trees rooted at left and right are max heaps==, but
> - Current node $i$ might be smaller than its children
>     - → Potentially violating the max heap property

- If violated, look at left and right children of node $i$
- Swap $i$ with whichever child has the bigger value
- If parent is already the largest, then subtree rooted at $i$ is already a max heap
    - Procedure terminates
- Moving down a branch of the binary tree takes $\mathcal{O}(\lg n)$ or $\mathcal{O}(h)$ where height of tree is $h = \lg n$

```pseudocode title:"Max Heapify Procedure (Bubble Down)"
Max-Heapify (A, i)
    l = left(i)
    r = right(i)
    if l <= A.heapsize and A[l] > A[i]
        largest = l  // Index of largest element is left child
    else
        largest = i  // Out of parent and left child, parent is larger
    if r <= A.heapsize and A[r] > A[largest]
        largest = r  // Right child has largest value
    if largest != i
        // Largest node was a child
        // Keep going down to ensure max-heap property is not violated
        exchange A[i] with A[largest]
        Max-Heapify(A, largest)
```

- We go down a single branch in each call to `Max-Heapify`
    - → Costs $\mathcal{O}(\lg n)$ time

![|640](https://i.imgur.com/EhSVwUt.png)

### Heapsort and Building Heaps

Given a heap, we can extract a sorted list of the elements in the heap by repeatedly calling `ExtractMax` and adding the items to a list.

> [!goal]+ To turn this into a true sorting algorithm…
>
> - Need a way of converting an input unsorted list into a heap

Suppose we have an unsorted array $A$ of length $n$.

- Approach 1:
    - Start with an empty i.e., garbage-filled array $B$ and size 0
        - Empty heap
    - Insert each item from $A$ into heap $B$
- Approach 2:
    - Start with the same array $A$ and call `max_heapify(A, i)` on each item in $A$ working backwards
        - i.e., $i$ goes from $n$ to 1

#### Build Heap

##### Approach 1: Insert Each Item into a Heap

- Start with empty heap
- Repeatedly call insert for a total of $n$ times
- Last insert takes all inserts build time ($\log n$)
- Repeated insertion is $\mathcal{O}(n \log n)$
- When starting with sorted array, set $\Omega(n \log n)$
- So $\Theta(n\log n)$

![](https://i.imgur.com/UQlB9Ko.gif)

##### Approach 2: Modify In-place

- Interpret list as the level order of a nearly complete binary tree
    - Same as with heaps
- Perform bubble down operation on each node in the tree
    - Starting at the bottom node
        - & All leaf nodes are valid individual heaps!
            - Can actually start with the last parent
    - Working way up to root
    - → Makes every subtree a max heap
    - → Result satisfies heap property

![](https://media2.dev.to/dynamic/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdpy879r83nua30a2ntgf.gif)

From CLRS:

```pseudocode
Build-Max-Heap(A)
    A.heapsize = A.length
    for i = floor(A.length / 2) downto 1
        Max-Heapify(A, i)
```

From Course Notes:

```python
def BuildHeap(items):
    i = items.size // 2
    while i > 0:
        BubbleDown(items, i)
        i = i - 1

def BubbleDown(heap, i):
    while i * 2 <= heap.size:
        curr_p = heap[i].priority
        left_p = heap[2 * i].priority
        right_p = heap[2 * i + 1].priority

        # Heap property is satisfied
        if curr_p >= left_p and curr_p >= right_p:
            break
        # Left child has higher priority
        else if left_p >= right_p:
            PQ[i], PQ[2 * i] = PQ[2 * i], PQ[i]
            i = 2 * i
        # Right child has higher priority
        else:
            PQ[i], PQ[2 * i + 1] = PQ[2 * i + 1], PQ[i]
            i = 2 * i + 1
```

##### Running Time of `BuildHeap` and `BubbleDown`

Let $n$ be the length of `items`.

The loop in `BubbleDown` iterates at most $\log n$ times.
Since `BubbleDown` is called $n$ times, the worst-case running time is $\mathcal{O}(n\log n)$.

> [!important]+ This is a loose analysis! We can do better.
>
> - The larger $i$ is, the fewer iterations the loop runs
> - Example of a situation where obvious upper bound on worse-case is actually not tight
> - Require a better understanding of how long `BubbleDown` takes to run as a ==function of $i$== and not just length of `items`

- Running time of `max_heapify(A, i)` is proportional to ==height of *subtree* of $A$ rooted at $i$==
- ? How many subtrees of each height do we have?
    - $\frac{n}{2}$ leaves at height 0 → Requires 0 swaps on each node
    - $\frac{n}{4}$ leaves at height 1 → Requires (at most) 1 swap on each node
    - $\frac{n}{8}$ leaves at height 2 → Requires (at most) 2 swaps
    - $\cdots$
    - 2 nodes at height $\lg n - 1$ → (At most) $\lg n - 1$ swaps
    - 1 node at height $\lg n$ → (At most) $\lg n$ swaps

Let $T(n, i)$ be the maximum number of loop iterations of `BubbleDown` for input $i$ and a list of length $n$.
Then, the total number of iterations in all $n$ calls to `BubbleDown` from `BuildHeap` is $\sum\limits_{i=1}^{n} T(n, i)$.
The maximum number of iterations is the height $h$ of node $i$ in the complete binary tree with $n$ nodes.

$$
\sum\limits_{i=1}^{n} T(i, n) = \sum\limits_{h=1}^{\lfloor \lg n \rfloor } h \cdot \text{\# nodes at height } h
$$

> [!question]+ How many nodes are at height $h$ of a binary tree?
>
> - $\left\lceil  \frac{n}{2^{h + 1}}  \right\rceil$ nodes

$$
\begin{align}
\sum\limits_{i=1}^{n} T(i, n) & = \sum\limits_{h=1}^{\lfloor \lg n \rfloor } h \cdot \text{\# nodes at height } h \\
 & = \sum\limits_{h=1}^{\lfloor \lg n \rfloor } h \cdot \left\lceil  \frac{n}{2^{h + 1}}  \right\rceil  \\
 & = \frac{n}{2} \sum\limits_{h=1}^{\lfloor \lg n \rfloor } \frac{h}{2^{h}} \\
 & < \frac{n}{2} \sum\limits_{h=1}^{\infty}  \frac{h}{2^{h}} \\
 & = \frac{n}{2} \sum\limits_{h=1}^{\infty} \left( \frac{1}{2} \right)^{h} \\
 & = \frac{n}{2} \frac{\left( \frac{1}{2} \right)}{\left( 1-\frac{1}{2} \right)^{2}} &  & \left( \sum\limits_{k=0}^{\infty} kx^{k} = \frac{x}{(1-x)^{2}} \text{ for } \left| x \right| < 1, \text{ A.11 CLRS} \right) \\
 & = n \\
 & = \mathcal{O}(n)
\end{align}
$$

> [!note]- Course Notes provides a different analysis from lecture.
>
> - Suppose the tree has height $k$ and $n = 2^{k + 1} - 1$ nodes
>     - Binary tree has a full last level
>     - % Course Notes indicates that a complete tree with one node is height 1, but this is defined different in textbook
>         - A tree with one node has height 0 (CLRS)
>         - Course Notes says $n = 2^{k} - 1$ instead
>
> ![](https://i.imgur.com/G9T08Pm.png)
>
> - Then, the number of nodes at height $h$ when the tree has height $k$ is $2^{k-h}$
>
> $$
> \begin{align}
> \sum\limits_{i=1}^{n} T(i, n) &= \sum\limits_{h=1}^{k} h \cdot \text{\# nodes at height } h \\
>  & = \sum\limits_{h=1}^{k} h \cdot 2^{k-h} \\
>  & = 2^{k} \sum\limits_{h=1}^{k} h \cdot \frac{h}{2^{h}} \\
>  & = \frac{n + 1}{2} \sum\limits_{h=1}^{k} \frac{h}{2^{h}}  &  & (n = 2^{k + 1} - 1) \\
>  & < \frac{n+1}{2} \sum\limits_{h=1}^{\infty} h \left( \frac{1}{2} \right)^{h} \\
>  & = \frac{n+1}{2} \frac{\frac{1}{2}}{\left( 1 - \frac{1}{2} \right)^{2}}  &  & \left( \sum\limits_{k=0}^{\infty} kx^{k} = \frac{x}{(1-x)^{2}} \text{ for } \left| x \right| < 1, \text{ A.11 CLRS} \right)  \\
>  & = \frac{n+1}{2} \cdot 2 \\
>  & = n+1
> \end{align}
> $$
>
> Then, the total number of iterations is less than $n + 1$.
> Thus, the total cost of all the calls to `BubbleDown` is $\mathcal{O}(n)$, which leads to a total running time of `BuildHeap` of $\mathcal{O}(n)$.

> [!note]+ The final running time depends only on the size of the list
>
> - What we did was a more careful analysis of the helper function `BubbleDown`, which did involve $i$
> - Used $i$ in summation over all possible values for $i$

#### Heapsort

- Algorithm starts by called `Build-Max-Heap` to build a max heap on the input array
    - (Approach 2)
- Remove value from root by swapping root with last value in valid max heap
    - Place old root at end of array
- Decrement heap size (to $n - 1$)
- Maintain max heap and repeat process for the max heap of size $n - 1$
    - Down to a heap of size 2
        - Since the last iteration on heap size 2 will swap with the root, which is the larger value

```pseudocode
Heapsort(A)
    Build-Max-Heap(A)
    for i = A.length downto 2
        exchange A[1] with A[i]
        A.heapsize = A.heapsize - 1
        Max-Heapify(A, 1)
```

> [!obs]+ Heapsort takes $\mathcal{O}(n \lg n)$ time
>
> - `Build-Max-Heap` takes $\mathcal{O}(n)$ time
> - Each of the $n - 1$ calls to `Max-Heapify` take $\mathcal{O}(\lg n)$ time

- We can call `Max-Heapify` on line 6 since the subtrees all satisfy the max heap property
    - We only swapped the root with the last element that has not been sorted, then decremented the size
