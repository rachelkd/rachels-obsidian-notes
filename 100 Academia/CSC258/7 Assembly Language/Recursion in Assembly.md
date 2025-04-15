---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 12
Module:
  - 7 - Assembly Language
Lecture:
  - "34"
  - "35"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-31
Date created: Sun., Apr. 13, 2025, 3:00:02 am
Date modified: Sun., Apr. 13, 2025, 4:24:04 am
---

# Recursion in Assembly

- Anytime you make a function call:
    - Say:
        - ? What registers am I going to need?
        - ? What registers is this function going to overwrite?
    - In general: do not really know
- With recursion:
    - Every function is going to overwrite the registers of the calling program
        - All the nested functions are all the same function
    - If you create a recursive function, it is going to call itself
    - Any registers that the calling program needs:
        - & Recursive call is going to use too
    - Know in advance what you need to preserve

## Example: `factorial(int n)`

> [!info]+ Basic pseudocode for recursive factorial
> - Base case (`n == 0`)
>     - Return 1
> - Get `factorial(n - 1)`
>     - Store result in “product”
> - Multiply product by `n`
>     - Store in “result”
> - Return result

### Recursive Programs

![](https://i.imgur.com/YwEt2Qq.png)

- ? How do we handle recursive programs?
    - Still needs *base case* and *recursive step*
        - As with other languages

> [!important]+ Main difference (w.r.t. other languages)
> - & Maintaining register values

- When recursive function calls itself in assembly:
    - Calls `jal` back to the beginning of the program
- ? What happens to the ==previous value of `$ra`==?
- ? What happens to the ==previous register values==, when the program runs a second time?

> [!check]+ Solution
> - & The **stack**
>     - Before recursive call:
>         - *Store* register values that you use onto the stack
>         - *Restore* them when you come back to that point
>     - & Do not forget to store `$ra`
>         - Or else program will loop forever

### Factorial Solution

> [!info]+ Steps to perform
> - Pop `x` off the stack
> - Check if `x` is zero
>     - If `x == 0`:
>         - Push `1` onto the stack
>         - Return to calling the program
>     - If `x != 0`:
>         - Push `x - 1` onto the stack
>         - Call `factorial` again
>             - i.e., jump to beginning of the code
> - After recursive call:
>     - Pop result off of stack and multiply that value by `x`
> - Push result onto stack and return to calling program

### Pseudocode

![](https://i.imgur.com/1ffovWU.png)

### Translated Recursive Code

![](https://i.imgur.com/6wgLEpt.png)

![](https://i.imgur.com/0PXrDdB.png)

- % `jal` always stores the next address location into `$ra`
- % `jr $ra` returns to that address

### Factorial Stack view

![](https://i.imgur.com/q0JNQmF.png)
