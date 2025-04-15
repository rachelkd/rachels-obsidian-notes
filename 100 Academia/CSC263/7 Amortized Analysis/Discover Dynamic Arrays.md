---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC263]]"
aliases: []
Week: 10
Module:
  - "[[7 - Amortized Analysis]]"
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - https://q.utoronto.ca/courses/379913/pages/discover-dynamic-arrays-2?module_item_id=6330550
Date: 2025-03-18
Date created: Tue., Mar. 11, 2025, 3:04:05 am
Date modified: Tue., Mar. 18, 2025, 3:00:13 am
---

# Discover: Dynamic Arrays

- Up until this point in the course, each time we used an array:
    - Assumed that before we created the array, we already knew the maximum array *size* that we would require
    - Assumed that we would declare array of this maximum size
        - Then we would always have enough space in our array for any elements that we would ever want to store
    - → $\mathcal{O}(1)$ access to read (or write) an individual array element
- ! Assuming that we know maximum required array size is *problematic*
    - Often:
        - ! Have situations where we do not know a reasonable maximum

<!-- break -->
- One solution:
    - Use a **linked data structure** instead of array
        - e.g., linked list or binary tree
    - & These structures are **dynamic**
        - Can grow and shrink as needed
    - ! We lose the constant time access to an individual element based on its index

> [!info]+ High-level programming languages often provide **dynamic arrays**.
> - Data structures that behave like arrays
>     - i.e., give access to elements based on their index
>     - But instead of having a fixed size, these “arrays” *grow* as necessary
>         - (and shrink)
> - Have encountered dynamic arrays in Python lists, Java `ArrayList`s
>     - ? How are these dynamic arrays implemented?

- Reason why we can have fast (constant time) access to array elements:
    - & Array is stored in a contiguous block of memory
    - Based on array *index*, memory *address* of the start of the array and the *size* of the array:
        - Memory address to a particular element can be determined with arithmetic
    - & The ability to directly access an element of an array using its index is dependent on the fact that the array is stored in one *contiguous* block of memory
- **Contiguous**
    - Together in sequence
    - % Array elements come right after each other in the computer memory without anything between them

> [!danger]+ This requirement that array elements are contiguous is a *problem* when it comes to adding more elements to an existing array.
> - Want to append a new item to a dynamic array:
>     - May not be allowed to use the next contiguous address
>         - e.g., already allocated to another variable in our program, or OS has allocated that address to another program that is running on the same computer

> [!summary]+ What happens is that when a dynamic array is first created?
> - A contiguous block of memory is allocated
>     - Holds some default number of elements
> - As elements are appended to dynamic array (and size grows):
>     - Elements fill up
> - At som epoint:
>     - All allocated elements in the underlying array are filled
>     - When another element is appended and dynamic array has to grow:
>         - An entirely *new* larger block of memory is allocated
>         - All existing elements are *copied* to the new array corresponding to the new block ofmemory
>         - New element is inserted

## Pseudocode for One Implementation of Insertion into a Dynamic Array

![](https://i.imgur.com/bXf9xvW.png)

- `A.size`
    - Holds the actual number of elements currently in the dynamic array
- `A.allocated`
    - Holds the total number of elements for which memory space is currently allocated
- `A.array`
    - Underlying array of size `A.allocated` that holds the elements themselves
- % To the user of this ADT, it appears as if `A` is the array, but it is actually a *wrapper* around the array `A.array`

## Worst Case Analysis

- % We will only count the number of *array accesses* made
    - Allows us to compute an exact number for the cost
        - But one that is within a constant factor of the total time
    - Still provides us with an accurate measure of the overall runtime

Suppose we have a dynamic array that contains $n$ items.

- % Final two steps involve just one array access (inserting new item `x`)
- % Code executed in the if block has exactly $2n$ accesses in line 5
    - For each of the $n$ items in the old array, there is *one* access to read it from the old array and a *second* access to copy value to new array

- `Insert`
    - Worst-case running time of $2n + 1 = \mathcal{O}(n)$ array accesses
    - Upper bound does not seem very impressive
        - AVL trees were able to support insertion in worst-case $\Theta(\log n)$ time
- ? Why should we bother with dynamic arrays at all?
    - Intuition:
        - Linear worst-case insertion is not a trult representative measure of the efficiency of this algorithm
            - Since it occurs when underlying array is full
        - If we assume underlying array’s length starts at 1 and *doubles* each time the array expands:
            - & Underlying array’s lengths are always *powers of 2*
            - & $2n + 1$ array accesses can only ever occur when $n$ is a power of two
        - Rest of the time:
            - Insertion accesses only a *single* element

$$
T(n) =
\begin{cases} \\
2n + 1, \quad & \text{if }n \text{ is a power of 2} \\
1, \quad & \text{otherwise}
\end{cases}
$$
- While it is true that $T(n) = \mathcal{O}(n)$:
    - ! Not true that $T(n) = \Theta(n)$

> [!question]+ Are average-case or worst-case expected analyses any better here?
> - These forms of analysis are only relevant if we treat either the input data as random, or if the algorithm itself has some randomness
> - This algorithm certainly doesn’t have any randomness in it
>     - Isn’t a realistic input distribution (over a finite set of inputs) we can use here
>         - The whole point of this application is to be able to handle insertion into arrays of any length
> - ? What do we do instead?

During this week’s lectures, we will learn about [[Amortized Analysis|amortized analysis]] and explore a few basic techniques in this area.
