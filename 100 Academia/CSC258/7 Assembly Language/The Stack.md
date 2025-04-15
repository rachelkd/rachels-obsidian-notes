---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [stack]
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "33"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-28
Date created: Sat., Apr. 12, 2025, 11:07:14 pm
Date modified: Sat., Apr. 12, 2025, 11:54:23 pm
---

# The Stack

- **Stack**
    - A spot in *memory* used to store values independent of the registers
        - Which can get overwritten easily
- & A special register stores the **stack pointer**
    - Which points to the *last element pushed onto the top of the stack*
        - MIPS:
            - Stack pointer is `$29` (`$sp`)
            - & Holds the address of the *last element* pushed to the top of the stack
        - % In other systems: `$sp` could point to the *first empty location on top of the stack*
- We can *==push==* data (like `$ra`) onto the stack
    - Which makes it *grow*, and
    - *==Pop==* data from the stack
        - Which makes it *shrink*
- & Stack is allocated a **maximum space** in memory
    - If it grows too large:
        - ! Risk of it exceeding this predefined size and/or ==overlapping with the *heap*==

## Programmer’s View of Memory

![](https://i.imgur.com/EULFs69.png)

- Stack is part of memory used for function calls, etc.
- & Stack grows toward smaller (lower) addresses
    - See arrow
- Stack uses **LIFO** (last-in-first-out) order
    - Like a physical pile that you add and removes items from
- **Stack overflow**
    - If you reach the end of stack memory
    - e.g., infinite recursion

## Stack to the Rescue

- ? When do you store values onto the stack?
    - Whenever you call a function and want to *preserver values* from getting overwritten
        - Like `$ra` (see the problem in [[Functions in Assembly#Nested Function Call Issue|functions in assembly]])
- ? What happens when you have nested function calls, each of which stores `$ra` on the stack?
    - Different `$ra` values will exist *in layers* on the stack over time
- Can also use stack to store:
    - & Function arguments
    - & Function return values
    - (more on this later)

## The Stack, Illustrated

![](https://i.imgur.com/pKwB91N.png)

- Stack pointer points to top element of the stack

### Popping Values off the Stack – Before

![](https://i.imgur.com/7ErqRA0.png)

- If you want to remove element from stack i.e., *pop* from stack:
    - Stack pointer is pointing to top element; single word in memory
        - Want to remove top element
    - & Load element that `$sp` is pointing to → store in `$ra` (if word is a return address)
    - & Update the stack pointer
        - Taken off a full word from stack
        - Update by ‘moving it back down’ four bytes i.e., add 4 to `$sp`
            - Since ‘down’ corresponds to higher address values

### Popping Values off the Stack – After

![](https://i.imgur.com/wLuMraJ.png)

### Pushing Values to the Stack – Before

![](https://i.imgur.com/HrhlAx3.png)

### Pushing Values to the Stack – After

![](https://i.imgur.com/eWiZowc.png)

## Stack Usage

- Whenever we refer to *pushing* and *popping* values:
    - Pushing something onto the stack means
        - *Allocate space* by ***decrementing*** the stack pointer by the appropriate number of bytes
        - Do a store (or multiple stores as needed)
    - Popping something from the stack means:
        - Do a load (or multiple loads as needed)
        - *De-allocate space* by ***incrementing*** the stack pointer by the appropriate number of bytes

## Advice on Using the Stack

- & Any space you allocate on the stack, you should later de-allocate
- If you *push* items in a certain order:
    - & Should *pop* items in *reverse order*
- When pushing *more than one item* onto the stack, you can:
    - Either allocate *all* the space in the beginning, or
    - Allocate space as you go
    - Same principle for popping

> [!info]+ The other thing people use the stack for is [[Passing Function Parameters|passing function parameters]].
