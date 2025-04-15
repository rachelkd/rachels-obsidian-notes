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
  - "36"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-31
Date created: Sun., Apr. 13, 2025, 3:00:02 am
Date modified: Sun., Apr. 13, 2025, 4:23:23 am
---

# A Note on Interrupts and Exceptions

- **Interrupts**
    - Take place when *external* event requires a change in execution
    - **Exception** examples:
        - Arithmetic, overflow, undefined instructions, division by zero
        - All *internal* to processor
    - **Interrupt** examples:
        - Key pressed, printout finished, network packet arrived
    - & Interrupts signalled by an *external input wire*
        - Usually checked at the end of each instruction

## Handling Interrupts and Exceptions

- **Interrupt service routine** (ISR)
    - Piece of code in the OS that handles the interrupt and serves the interrupting device or the exception event
- Similar to a function-call
    - But with a **==major difference==**
        - You never know *when* it happens
    - Interrupts: caused by external event
    - Exceptions: thrown by instructions, but you (usually) do not know at the time of writing the program
- & → Extra things should be done by HW (processor)
    - Disabling the interrupts
    - Saving the return address (and some other registers)
    - Jumping to the ISR
- Can view this like a *function-call by hardware*

> [!info]+ Interrupts can be handled in two general ways
> - **Single handler**
>     - Interrupt source identification done in software
>     - Processor branches to the address of interrupt handling code
>         - Which begins sequence of instructions that check the cause of the exception
>     - This branches to handler code sections
>         - Depending on the type of exception encountered
>     - % This is what MIPS uses
> - **Vectored handling**
>     - Interrupt source identification done in *hardware*
>     - Processor can branch to a different address for each type of exception
>         - Each exception address is separated by only one word
>         - Jump instruction is placed at each of these addresses for the handler code for that exception

## Interrupt Handling

- In the case of single-handler interrupt handling:
    - The handler (software) checks the value in the **cause register**
        - Jumps to corresponding exception handler code

![](https://i.imgur.com/IjuQTSA.png)

- If the original program can resume afterwards:
    - Interrupt handler returns to the program by calling the `rfe` instruction
- Otherwise:
    - Stack contents are *dumped*
    - Execution will continue elsewhere
    - i.e., back to OS
