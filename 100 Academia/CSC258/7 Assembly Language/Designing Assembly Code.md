---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "33"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-28
Date created: Sat., Apr. 12, 2025, 7:36:08 pm
Date modified: Sat., Apr. 12, 2025, 10:28:43 pm
---

# Designing Assembly Code

## Making Sense of Assembly Code

- [[Arrays and Structs|Last time]]:
    - Had two arrays called `A` and `B`
    - How do we declare them? How do we iterate through the elements of `B` and put them into `A`
    - $ Used a lot of registers for this code
- Assembly language look sintimidating
    - Programs involve a lot of code

> [!tip]+ Key to reading and designing in assembly code
> - *Recognizing* portions of code that represent *higher-level operations* that you are familiar with

## Array Code Example

![](https://i.imgur.com/jWAPQOS.png)

- ? How did we create our solutions?

> [!tip]+ First stage
> - **Initialization**
>     - Store locations of `A[0]` and `B[0]`
>         - In `$t8`, `$t9`, for example
>     - Create a value of `i($t0)` and set it to zero
>     - Create a value to store the max value for `i`, as a stopping condition
>         - In `$t1`, in this case

- Think about all the variables that you need to have
    - All the registers you need in order to make this simple program happen
    - Need to think in advance
        - To anticipate how many you will need
- & Best to initialize all the registers that you will need at once
    - Even ones that do not have variable names in the original code

> [!tip]+ Second stage
> - **Main processing operation**

- Fetch source `B[i]`
    - Get address of `B[i]` by adding `i * 4` to the address of `B[0]`
        - Which is stored in `$t8`
    - Load value of `B[i]` from that memory address
        - In `$s4`
- Ready destination `A[i]`
    - Same steps as for `B[i]`, but address is stored in `$t4`
- Add 1 to `B[i]`
    - Storing result in `$t6`
- Store this new value into `A[i]`
    - Same as fetching a value from memory, but in reverse
- Increment `i` to the next offset value
- Loop to the beginning if `i` has not reached its max value

## Loop Exercise

![](https://i.imgur.com/rz7fTdR.png)

![](https://i.imgur.com/4aPJvKk.png)
