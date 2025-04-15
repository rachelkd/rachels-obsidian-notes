---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [for loop, if statement, if/else statement, while loop]
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "31"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-24
Date created: Sat., Apr. 12, 2025, 1:51:51 am
Date modified: Sat., Apr. 12, 2025, 2:43:35 am
---

# Using Branches and Jumps

> [!abstract]+ `if`, `else`, `while`, `for`

## If Statements

- `if` statements
    - Test a condition
    - Execute lines of code if condition is true

For example:

```c
if (i == j) {
    i++;
}
j = j + 1;
```

- Would have some `if` statement that would check a condition
- If condition satisfied → do one thing
- If you have `else` statement → do another thing
- After that:
    - Would be the rest of your code

> [!important]+ Testing conditions is done using either a `beq` instruction or a `bne` instruction

### Translated `if` Statement

> [!question]+ How would this look in assembly?

```c
if (i == j) {
    i++;
}
j = j + i;
```

- Testing two variables to see if they are equal
- If they are equal: do one thing
- Skip otherwise (if they are not equal)
- & Use `bne` instruction to skip the `i++` step → proceed straight to the `j = j + i` step
- `bne` says:
    - If this condition (`i == j`) is not satisfied → jump over ‘this’ and skip to the end
    - Branching will happen if the condition is not satisfied
        - Otherwise: would go to the next line
- Even though you might intuitively think `i == j` tests to see if they are equal:
    - & Really what you are saying is: do I kick this process out of here if they are not equal?
    - Default is program will go one line at a time unless you tell it otherwise
- Think of branching as a *bouncer* that says what parts are *not allowed* at the club
    - If `i != j`, then you have to move on

```assembly
#  $t1 = i, $t2 = j
main:    bne $t1, $t2, END     # branch if (i != j)
         addi $t1, $t1, 1      # i++
END:     add $t2, $t2, $t1     # j = j + i
```

- Normally, in code, we use branching like:
    - Give me the condition I need to satisfy
    - Then execute the code right after if it satisfies
- & Branch is telling you when to *skip lines*
    - If `addi $t1, $t1, 1` is the line you want to execute if `i == j`/`$t1 == $t2`
        - You want to say the condition under which they *do not execute* this line

> [!tip]+ What the branch does
> - What is the condition that I do not want to go to the next line?
>     - Want to skip and go to the code after it
> - e.g., Want to execute `i++` if the two things are equal
>     - Therefore would jump over this code *if they are not equal*

## `if/else` Statements

```c
if (i == j) {
    i++;
} else {
    i--;
}
j += i;
```

- Possible approaches to `if/else` statements
    - Test condition; jump to `if` logic block whenever condition is true
    - Otherwise: perform `else` logic block; jump to fire line after `if` logic block

### `if/else` Statements

![](https://i.imgur.com/eUXU1zC.png)

- Can do it either way (with `beq` or `bne`)
- Some people think:
    - In higher level code: testing if `i` and `j` are equal to each other
    - Shouldn’t I do a `beq` to see if they’re equal at the beginning
        - You can do that
        - & What `beq` is saying is: if they are equal, what do you want to do?
            - Jump down to the section (`IF`) that handles the if statement
            - If not satisfied, then goes to the next line
                - → `i--` if not equal, so this is the instruction right after `beq`
                    - Then you have to jump to `END`
                        - ! If you did not have `j END`, it would also execute the `IF` condition (or `ELSE` in the bottom example)

## A Trick with `if` Statements: `if`/`else` Statement Flowcharts

> [!tip]+ Use flow charts to help you sort out the control flow of the code
> ![](https://i.imgur.com/7jsVfwV.png)
> ![](https://i.imgur.com/ivWkg7F.png)

## Multiple `if` Conditions

### OR Conditions

![](https://i.imgur.com/gGqs9me.png)

- ! Testing two conditions!
    - Branch can only do one test at a time

> [!question]+ How do you handle this?
> - & Branch statement for each condition

![](https://i.imgur.com/DXZ7IU6.png)

### AND Conditions

![](https://i.imgur.com/1rpWEnd.png)

![](https://i.imgur.com/DuDn5aB.png)

- Looking for any one of these conditions to fail
    - Since any one that fails will make us go to `ELSE`
- → Check to see if first, second, third, etc. fails
- **Short circuit**
    - If you are testing a bunch of statements and have them all in AND statements:
        - First one that fails → do not test any of the others (bypass the others)
        - i.e., will not test the rest if one fails
    - % This is why short circuiting happens in higher-level code

## `while` Loops

![](https://i.imgur.com/78i0Jx8.png)

- Loops look similar to `if` statements:
    - & Test if the loop condition fails
        - If it does → branch to the end
        - Otherwise, execute the `while` loop contents
        - (Make sure to update loop condition values)
        - Jump back to beginning

### Example of a Simple Loop in Assembly

![](https://i.imgur.com/mrq7gTQ.png)

… which is the same as saying (in C):

![](https://i.imgur.com/GWX4RkE.png)

- Jump `j` is what helps us become like a loop
- Branch helps it not be an *infinite loop*

## `for` Loops

- For loops usually are implemented with the same structure

![](https://i.imgur.com/w7PV8fZ.png)

… translates to

![](https://i.imgur.com/oazrEXr.png)

### `for` Loop Example

![](https://i.imgur.com/cA2VnEG.png)

… translates to

![](https://i.imgur.com/ZEdnHnG.png)

- $ Without the initialization and update sections, this is the ==same as a `while` loop==
    - `for` and `while` loops at this level look exactly the same:
        - Initialization part
        - Branch telling you when to jump out
        - Some kind of update
        - Jump to make the loop happen
