---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [I-type instructions, J-type instructions, R-type instructions, Register names]
Week: 9
Module:
  - 7 - Assembly Language
Lecture:
  - "27"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-14
Date created: Sun., Mar. 16, 2025, 12:14:36 am
Date modified: Wed., Mar. 19, 2025, 2:13:26 pm
---

# Machine Code Instructions

## Intro to Machine Code

> [!info]+ Now that we have a processor, operations are performed by:
> 1. The **instruction register**
>     - Sending instruction components to the processor
> 2. The **control unit**
>     - Based on the **opcode** value (sent from the instruction register), sending a sequence of signals to the rest of the processor

> [!question]+ Where do these instructions come from? How are they provided to the instruction memory?

- Instruction register
    - Has all the things the processor needs to perform the operation
        - Opcode at the start
        - Various things (e.g., registers) in it
- When we say that the instruction has everything
    - & Need to say: Do we know how to encode an instruction if we needed to?
    - Given an operation needed to perform, could you figure out what the 32-bit machine code instruction is?

## Assembly Language

> [!info]+ Each processor type has its own language for representing — say 32-bit — instructions as user-level code words.

Consider a basic operation: add two numbers together and store it in a third place.

> [!example]+ `C = A + B`
> - Assume:
>     - `A` stored in `$t1`
>     - `B` stored in `$t2`
>     - `C` stored in `$t3`
>     - Assuming $A, B, C$ are all stored in registers, because that is where all data comes from
>
> > [!info]+ Assembly language instruction
> >
> > ```assembly
> > add $t3, $t1, $t2
> > ```
> > - $ Destination always comes first
> >     - `$t3`
> >     - The things we add can sometimes have different configurations to them
> >         - e.g., Adding `$t1` to a constant
> >     - But will always have a destination, so destination comes first
> > - % `$` indicates that this is a register
>
> - Once we use the assembler program on this assembly code:
>     - Will convert directly to a machine code instruction
>
> > [!info]+ Machine code instruction
> > $$000000 \; 01001 \; 01010 \; 01011 \; XXXXX \; 100000$$
>
> - $ There is a one-to-one mapping for all assembly code and machine code instructions

- You provide the instructions in assembly language format
    - Can kind of recognize, read, and parse in brain
    - Machine code is not so easily parseable by our brain
- But it is parseable by the instruction register
    - When it takes the machine code in, it knows to take the first six bits to send to control unit
    - Knows to take next three sets of five bits and send to register file
        - To be source and destination
    - Last six bits tells ALU to do an add operation
- & Need to understand machine code to understand what kind of operations we can do in assembly language

## Encoding the Instruction

> [!question]+ What needs to be put in these machine code instructions?
> - Processor is made up of many different parts
>     - ? What parts need information from the instruction to know what needs to be done?

> [!info]+ Machine code instructions contain all the details about a processor operation, such as
> - & What *operation* is being performed (opcode)
> - & What *registers* are being used in this operation
> - & What other information might be needed to make this operation happen
>     - Immediate or shift values

- When we write or interpret a machine code instruction:
    - Need to know how to encode (or decode) these details into these 32 bits

## R-type Instructions

> [!question]+ How do we encode the earlier instruction that adds registers `$t1` and `$t2`, and stores the result into register `$t3`?

![](https://i.imgur.com/5QOkSbF.png)

- `add` is an **R-type instruction**
    - Operating on two registers that are going to ALU
    - Result is stored in a third register
    - Opcode $000000$ signals a R-type instructions
        - Same datapath signals
            - No matter if operation is add, subtract, AND, OR
        - All involve taking two registers and storing a value from ALU into a third register
        - What happens inside ALU is not known to control unit
- Instruction needs to tell ALU what operation you are doing
    - Last 6 bits
    - e.g., Add operation is $100000$

> [!important]+ With any R-type instruction — two source registers, one destination register — the first six bits are always going to be $000000$
> - The actual ALU operation is from the instruction via the *last six bits*

- Rest of the bits
    - Specify which three registers are involved
        - Two source registers
        - One destination register
- Recall:
    - `$t3` is the destination
        - $01011$
    - `$t1`, `$t2` come first in machine code instruction
        - $01001, 01010$
- & Source registers come before destination register in machine code
- ? Why are the registers 5 bits?
    - Have 32 registers
    - Every register is addressed by a 5-digit address
- We have names for each register
    - Based on roles
    - Refer to as `$t1`, `$t2`, `$t3`
        - Instead of the number

<!-- break -->
- Next 5 bits after registers:
    - & Tell you how much you are shifting by
        - Only if doing a shift operation
    - Does not matter what this number is if not doing shift
        - Probably set to 0s to be safe

<!-- break -->
- When we *assemble*:
    - → Gets converted into 32-bit instruction

### Operating on Registers

- `add` instruction
    - One of many operations that processes two register values and stores result in a third

> [!def]+ R-type instruction
> - Operation that processes two register values
> - Stores the result in a third
> - Any operations whose ==inputs and outputs are all registers==
>     - Even if they involve less than three registers
>         - e.g., `jr` instruction

- ? How to encode R-type instructions?
    - Need to know the *5-bit codes* used to refer to our input and output *registers*

### Machine Code and Registers

> [!info]+ MIPS is **register-to-register**.
> - Almost all operations rely on register data

Recall our example above:

```assembly
`add $t3, $t1, $t2`
```

- `$t1`, `$t2`, `$t3`:
    - Encoded as registers $01001, 01010, 01011$
        - $9, 10, 11$
    - Could have done `$0`, `$1`, …, `$31` to represent the 32 different registers that could be involved in this register
- When we do things in assembly:
    - Trying to make things more *intuitive*
    - Why we make something like `add` instead of 6 zeroes opcode and $100000$ operation code
- % The people who designed assembly created *labels* for each of the registers in register file
    - Labels indicate register’s purpose/function
    - There is a convention for how these registers are meant to be used
        - For consistency
    - ? If you wanted to use some registers to pass values between one function and another, how do you make sure the next person who uses your program understands that and does not break what you are trying to do?
        - → Adopted a *convention*
        - Some registers are used for one thing; some for another
        - e.g., Some used for doing calculations
            - Some are used for very specific functions
        - Made this convention all throughout MIPS
    - In order to communicate this convention, they came up with label names

> [!attention]+ Just because we have 32 registers, it does not mean that we can use all 32 registers
> - Some need to be set aside for *special purposes*

MIPS provides 32 registers (with *numerical* and *label* names).

> [!tip]+ When making our programs, we refer to the labels.
> - Instead of saying `$0`, we say `$zero`
>     - More meaningful
>     - Know that this register has the value 0 in it

#### Special Values and Reserved Registers

| Register    | Name    | Purpose                                     |
| ----------- | ------- | ------------------------------------------- |
| `$0`        | `$zero` | Always holds the value `0`                  |
| `$1`        | `$at`   | Reserved for the assembler                  |
| `$26`-`$27` | —       | Reserved for the OS kernel                  |
| `$28`       | `$gp`   | Global pointer                              |
| `$29`       | `$sp`   | Stack pointer (used for stack manipulation) |
| `$30`       | `$fp`   | Frame pointer                               |
| `$31`       | `$ra`   | Return address                              |

- `$ra`
    - Stores the return address of whatever called the function
    - When done:
        - Know which address to jump back to pick up where you left off
- `$sp`
    - Stack pointer
    - Stores where the stack is
- `$gp`, `$fp` are beyond the scope of this course

#### Registers Used by Programs as Functions Parameters

| Register | Name  | Purpose             |
| -------- | ----- | ------------------- |
| `$2`     | `$v0` | Return value        |
| `$3`     | `$v1` | Return value        |
| `$4`     | `$a0` | Function argument 1 |
| `$5`     | `$a1` | Function argument 2 |
| `$6`     | `$a2` | Function argument 3 |
| `$7`     | `$a3` | Function argument 4 |

- `v` registers:
    - If performing a function and want to send return value back to calling program
        - Put values into `v` registers
        - Return control
        - When calling program wakes up, it will look in the `v` registers for whatever values you are passing back
- `a` registers:
    - Similarly, if program needs to call another function and pass in some values
        - Four registers (`a` registers)
        - Put some stuff in there
        - Call program
        - When program invoked, it looks at those four registers for arguments

#### Registers Used by Programs to Store Values

| Register    | Name        | Purpose             |
| ----------- | ----------- | ------------------- |
| `$8`-`$15`  | `$t0`-`$t7` | Temporary registers |
| `$24`-`$25` | `$t8`-`$t9` | Temporary registers |
| `$16`-`$23` | `$s0`-`$s7` | Saved temporaries   |

- `t` registers
    - Do whatever you want with them
    - Trash them and not have to feel bad about it
- `s` registers
    - Have some values we might need
        - → Put in `s` registers
        - Then restore when we are done
    - Safer place to put values

> [!note]+ Also three special registers — `PC`, `HI`, `LO` — that are not directly accessible
> - `HI`, `LO` are used in multiplication in multiplication and division
>     - Have special instructions for accessing them

### Filling in the Blanks

![](https://i.imgur.com/pWHOvTn.png)

- Registers `$t1`, `$t2` are registers 9, 10 respectively
- Register `$t3` is the 11th register
- Registers for this instruction
    - Encoded as $01001$, $01010$, $01011$

## I-type Instructions

- **I-type instructions**
    - Also operate on *registers*
    - But involve a *constant* value as well
        - Constant is ended in the last 16 bits of the instruction

![](https://i.imgur.com/nbgIkGe.png)

- Takes some register `$t1`
- Adding it to constant `42`
- Storing result in `$t2`

> [!info]+ `i` stands for *immediate*.

- Anything that is not an R-type
    - First six bits is not all zeroes
    - e.g., `addi` opcode is $001000$
- Source and destination registers come after opcode
    - 5 bits for each register
- Remaining 16 bits used for encoding constant number

## J-type Instructions

- **J-type instructions**
    - *Jump* to a location in memory encoded by the last 26 bits of the instruction
        - Everything but the opcode
    - Location is stored as a *label*
        - Resolved when assembly program is *compiled*
        - At *compile-time*

![](https://i.imgur.com/tW3L95f.png)

- Opcode
    - One of the two jump instructions
    - Control unit sees this opcode and recognizes that this is a jump type
        - → Take the rest of the instruction and send it to the *program counter*
- Program counter will go to that memory address to fetch the next instruction
    - Basically jumps ahead in your program
- % Do not have to worry about encoding this 26-bit address value
    - When you create your program and have a line that says “Jump to `main`”
        - Main is one of the labels for one of the lines in your program
        - That is the label you need to jump to
    - Just need to tell it what the label is for the line you want to jump to
        - Assembler converts everything to code
            - Sees `main` and knows it has to correspond to some address
            - Assembler puts things into instruction memory
            - Knows where `main` is

## Summary: MIPS Instruction Types

![](https://i.imgur.com/FWxWBfg.png)

- R-type
    - Broken down into *opcode*, *source*, *source*, *destination*, *shift amount*, *function*

![](https://i.imgur.com/NmqcW7S.png)

- I-type
    - *Opcode* to tell it what I-type operation we are doing
    - Two registers
        - Might be *source* and *destination*, or
        - Two *sources*
    - 16-bit *constant* value

![](https://i.imgur.com/3imsFq0.png)

- J-type
    - *Opcode* to tell control unit it is a jump operation
        - One of two types
    - *Address*

## Machine Code Details

> [!important]+ R-type instructions have an *opcode* of $000000$
> - 6-bit function listed at the end
> - Although we specify *don’t care* bits as $X$ values:
>     - Assembly language interpreter always assigns them to *some value* (like 0)

- Possible to program processor with machine code
    - But makes more sense to use an equivalent language
    - More natural for humans

See [[Assembly Language Overview]].
