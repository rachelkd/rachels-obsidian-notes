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
Date created: Sat., Apr. 12, 2025, 11:07:14 pm
Date modified: Sun., Apr. 13, 2025, 2:37:56 am
---

# Passing Function Parameters

## Functions vs. Code

- Since functions have *entry* and *exit* points:
    - Also need *input* and *output* parameters
- In other languages:
    - Parameters are assumed to be available at the start of function
- In assembly:
    - % Have to fetch those values from memory first before you can operate on them
- ? Where do you look for these parameters?

## Common Calling Conventions

- While most programs pass parameters through the stack:
    - Also possible to use *registers* to pass values to and from programs
        - & Registers 2-3: `$v0`, `$v1`
            - Return values
        - & Registers `4-7`: `$a0` - `$a3`
            - Function arguments
- If you function has up to four arguments:
    - Would only use the `$a0` to `$a3` registers in that order
        - First argument in `$a0`, second in `$a1`, and so on
- & Additional arguments would be pushed on the stack
    - More common convention is just to ==push all arguments to the stack==

## String Function Program

![](https://i.imgur.com/iPNgWIf.png)

- @ Convert this to assembly code
    - Also take in parameters from the stack
- In this case:
    - Parameters `x` and `y` are passed into the function, in that order
    - Pointer to the stack is stored in register `$29` aka `$sp`
        - Which is the address of the ==top element in the stack==

### Converting `strcpy`

#### Initialization

- ? What values do we need to store?
    - Address of `x[0]` and `y[0]`
    - Current offset value (`i` in this case)
    - Temporary values for the address of `x[i]` and `y[i]`
    - Current value being copied from
- Consider that the locations of `x[0]` and `y[0]` are passed in on the stack
    - & Need to fetch those first

![](https://i.imgur.com/UCcBXJJ.png)

#### Main Algorithm

![](https://i.imgur.com/iPNgWIf.png)

- ? What steps do we need to perform?
    - Get the location of `x[i]` and `y[i]`
    - Fetch a character from `y[i]` and store it in `x[i]`
    - Jump to the end if the character is the null character
    - Otherwise, increment `i` and jump to the beginning
    - At the end:
        - Push the value `1` onto the stack and return to the calling program

![](https://i.imgur.com/gN4ToKA.png)

- Characters are 1 byte long
    - Not 4 bytes!
    - So we add `i` to the address and not `i * 4`
