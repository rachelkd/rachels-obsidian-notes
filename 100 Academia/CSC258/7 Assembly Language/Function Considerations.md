---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [callee, caller, caller vs. callee, function call reference, function calling conventions, register saving conventions]
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "33"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-28
Date created: Sun., Apr. 13, 2025, 2:30:05 am
Date modified: Sun., Apr. 13, 2025, 2:59:48 am
---

# Function Considerations

## Function Call Reference

- ? How do you call a function?
    - `jal FUNCTION_LABEL`
    - Sets `$ra` to be return address to come back to
        - The instruction right after `jal FUNCTION_LABEL`
- ? How do you return from a function?
    - `jr $ra`
    - ? What if you have nested calls? Won’t `$ra` get overwritten?
        - Yes
        - Need to push `$ra` to stack
        - Then restore it

## Maintaining Register Values

- Have already demonstrated why we would need to *push `$ra` onto stack* when having nested function calls
- ? What about other registers?
    - How do we know that a function we called did not overwrite registers that we were using?
        - % Remember there is only one register file

> [!check]+ Solution
> - **Caller vs. callee calling conventions**

## Calling Conventions

- **Caller vs. Callee**
    - **Caller** is the function calling another function
    - **Callee** is the function being called
    - % A function can be both a caller and a callee i.e., recursion

> [!tip]+ We separate registers into…
> - **Caller-saved registers**
>     - `$t0`-`$t9`
> - **Callee-saved registers**
>     - `$s0`-`$s7`

- & Some registers are not your responsibility to save before you make a function call
- e.g., Suppose function A calls function B
    - Function A is responsible for saving return address, temporary `t` registers
        - Not responsible to save the saved temporaries `s` registers
    - Function B is responsible for saving `s` registers

### Register Saving Conventions

This is the idea behind the *caller* saving things and the *callee*.

> [!summary]+ Caller-saved registers
> - Registers `8`-`15` (`$t0`-`$t9`)
>     - Temporaries
> - & ==Registers that the caller should save== to the stack before calling a function
>     - If they do not save them → no guarantee contents of these registers will not be clobbered
>     - Push temporaries to stack just before you call another function
>     - Restore them immediately after

> [!summary]+ Callee-saved registers
> - Registers `16`-`23` (`$s0`-`$s7`)
>     - Saved temporaries
> - & It is the responsibility of the *callee* to save these registers and later restore them
>     - If callee is going to modify them
> - Push them to stack first thing in function body
> - Restore them just before you return

## Stack and Function Summary

> [!summary]+ Before calling a subroutine
> - Push registers onto the stack to preserve their values
>     - e.g., `$ra`
> - Push the input parameters onto the stack

> [!summary]+ At the start of a subroutine
> - Pop the input parameters from the stack

> [!summary]+ At the end of a subroutine
> - Push the return values onto the stack

> [!summary]+ Coming back from a subroutine call
> - Pop the return values from the stack
> - Pop the saved register values and restore them
