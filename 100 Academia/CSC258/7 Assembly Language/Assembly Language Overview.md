---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Assembly]
Week: 9
Module:
  - 7 - Assembly Language
Lecture:
  - "27"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-14
Date created: Sun., Mar. 16, 2025, 6:42:28 pm
Date modified: Wed., Mar. 19, 2025, 11:46:10 am
---

# Assembly Language Overview

## Assembly Language

![|300](https://i.imgur.com/JazwidL.png)

- **Assembly language**
    - *Lowest-level* language that you will ever program in

> [!info]+ Many *compilers* translate their high-level program commands into *assembly commands*
> - Which are then *converted* into machine code
> - Then used by the processor

- % There are *multiple* types of assembly language
    - For different architectures/processors
    - Assembly is specific to a kind of processor
    - e.g., AMD will have a different assembly language set than Intel
        - Mobile devices, video game consoles
    - If you know one, it is fairly trivial to learn another
        - While processors might be slightly different, they will not differ that much

## A Little about MIPS

The assembly language we learn is MIPS.

- **MIPS**
    - Microprocessor without Interlocked Pipeline Stages
    - A type of *RISC* architecture
        - *Reduced Instruction* Set Computer
    - & Provides a *set* of *simple and fast instructions*
        - Compiler translates instructions into 32-bit instructions for instruction memory
        - Complex instructions (e.g., multiplication) are built out of simple ones by the compiler and assembler
- Pipelining
    - Can start executing one instruction before the previous one is finished
    - → Doing a memory effect
        - Going to take a few clock cycles to come back

## MIPS Instructions

> [!info]+ Things to note about MIPS instructions
> - Instructions are written:
>     - `<instr> <parameters>`
>     - e.g., `add destination, source, source, function`
> - Each instruction is written on its own line
> - All instructions are 32 bits (4 bytes) long
>     - Recall: Every address stores one byte
> - Instruction addresses are measured in bytes
>     - Starting from the instruction at address $0$
>     - & All instruction addresses are divisible by 4

## Frequency of Instructions

The following table show the *most common* MIPS instructions, the syntax for their parameters, and what operation they perform.

![](https://i.imgur.com/0Pmu7RA.png)
