---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [$ra, function calls, program counter]
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
Date modified: Sat., Apr. 12, 2025, 11:07:28 pm
---

# Functions in Assembly

…or, why we need the stack.

- Recall three *jump* instructions
    - `j`
        - For for/while loops
    - `jal`
        - Jump and link
        - Jump to a section of code that is going to store that function
        - Need to return to where we were when we called the function
    - `jr`
        - Jump to a register
        - Use this to go to return address

## Functions vs. Code

- Up until this point:
    - Looked at how to create pieces of code in siolation
- **Function**
    - Creates an *interface* to this piece of code by defining an entry and exit point to it
    - Input and output parameters
- Requires two major considerations:
    - & How to jump into and out of a function
    - & How to pass values to and from a function

## Function Calls and Returns

- To support functions, need to be able to:
    - & Define the start of a function
        - *Label* the first line to provide a target address to jump to
    - & Take in function arguments and return values
        - Could use registers
        - ! But might need another solution
            - There are certain registers set aside for function arguments
            - `$a…` registers
            - Likewise: `$v0`, `$v1` registers are used for return values
    - & ==Store variables local to the function== and also ensure functions ==do not clobber useful data on registers==
        - If function A is using `$t0`, `$t1`, `$t2` and calls function B that also uses `$t0`, `$t1`, `$t2`:
            - When B returns, those registers are gone
        - ? How do we figure out ways for each function to use certain registers without worrying about the calling program needing the values there originally?
            - ? Can use registers for storing data, but are any off-limits?
    - & Return to the calling site
        - After last line in function, ==return to instruction *after* the one that did the function call==

### How Do We Call a Function?

![](https://i.imgur.com/FXmok1j.png)

- & `jal FUNCTION_LABEL`
    - Jump and link
    - Jumps to the first line of the function, which has the specified label
        - i.e., `function_X` here
- `jal` is a **J-type** instruction
    - Updates register `$31`
        - Which is also **`$ra`, return address register**
    - After it is executed:
        - `$ra` contains the *address of the instruction **after** the line that called `jal`*
            - e.g., `sum = 5`
        - If we returned to the same line that called it, then we would be stuck in a loop
- % Can also write values to `$ra` like any other register
    - But we use as convention to store return address

### How Do We return from a Function?

![](https://i.imgur.com/Zgz63JP.png)

- & `jr $ra`
    - Program counter is set to the address in `$ra`
- % You can jump to any register you want
    - But only time people ever use this is to jump back to whatever called you

![](https://i.imgur.com/LS3KoOh.png)

- ? How do we know what is in `$ra`?
    - `$ra` was set by the most recent `jal` instruction
        - (Function call)
    - How do we know the return address is in `$ra`?
    - Because that is what everyone does

![](https://i.imgur.com/JEUF5zd.png)

### Nested Function Call Issue

> [!question]+ What if `function_X` calls `function_Y`?

![](https://i.imgur.com/D0D9ifP.png)

- At (5) `jr $ra`:
    - Jumps back to `function_X` at the line with `return`
- (6) Execution continues at `return` in `function_X` with `jr $ra`
    - ! But `$ra` does not contain the return address of the line after `function_X(sum)`!
        - `jal FUNCTION_Y` overrode `$ra` → no way of getting back up to `sum += 5`

#### Solving the `$ra` Dilemma

> [!question]+ How do we make sure that we do not clobber the value of `$ra` every time we call a nested function?
> - & Need to ==put `$ra` away somewhere== for safe keeping if we know we are about to overwrite it
>     - Would be *ideal* if the structure used for this made it ==easy to find the last item== that was stored away

See [[The Stack]].
