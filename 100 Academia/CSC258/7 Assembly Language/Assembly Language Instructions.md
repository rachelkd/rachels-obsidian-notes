---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [ALU instructions, Assembly, Jump]
Week: 10
Module:
  - 7 - Assembly Language
Lecture:
  - "28"
  - "29"
  - "30"
  - "31"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-17
Date created: Mon., Mar. 17, 2025, 3:09:56 pm
Date modified: Sat., Apr. 12, 2025, 1:52:32 am
---

# Assembly Language Instructions

> [!summary]- Last time
>
> - Introduced the idea of assembly language
> - Talking first about machine code
> - Machine code
>     - The thing that has all the information the datapath needs to do its operations
> - Assembly language
>     - A way to be able to express exactly what is in machine code
>     - In a way that is easier for humans
>     - More intuitive in terms of:
>         - Labels of the operations
>         - Expressing what registers or data you are working with
> - In the end:
>     - A line in assembly code translates to a 32-bit instruction machine code

- @ Introduce all assembly language instructions
    - Machine code instructions:
        - Encapsulate all information that control unit needs
            - In the form of an opcode
        - Information that ALU needs
            - R-type instructions
        - Register addresses
    - How does assembly language express these machine code instructions?

## Arithmetic Instructions

![](https://i.imgur.com/TTrOLja.png)

- $ Four versions of `add`
    - Regular `add`
        - Assumes two signed numbers
    - `addu`
        - Assumes we are doing *unsigned* operation
        - Needed because there are conditions that will cause overflow in signed arithmetic
            - Different from unsigned arithmetic
        - In unsigned arithmetic:
            - Overflows when you get a number that is all ones (biggest number) and you need to go beyond that
        - Signed arithmetic:
            - All ones is considered -1, so going one higher gets you to 0
    - `addi`
        - `i` means *immediate*
        - Constant involved
    - `addiu`
        - Unsigned immediate operation
- If operation is I-type or J-type:
    - Second column is an *opcode*
- & All R-type operations have opcode $000000$
    - Second column is the function (last 6 bits in machine code instruction)

> [!note]- Syntax
>
> - Expecting those arguments (separated with space) after the instruction and space
>     - e.g., `add $d $s $t`

> [!note]+
>
> - **hi:lo**
>     - Refer to the *high* and *low* bits referred to in the register slide
>     - Multiplication:
>         - Length of the product is the same number of bits of both operands added together
>             - e.g., If you had 2 4-bit inputs in Lab 4, result had to be 8 bits long
>     - If you have two 32-bit inputs, there is no register that can store a 64-bit value
>     - Processor has two registers called *high* and *low*
>         - & Act as the upper and lower half of multiplication result
>     - Multiplication has no destination register
>         - Puts result in these high and low registers
>     - Division:
>         - Puts quotient in low
>         - Puts remainder in high
> - **SE**
>     - Sign extend
>     - `i` is 16-bits long
>     - Need to sign extend to add `i` to a 32-bit number

### Assembly → Machine Code

> [!question] What happens when you create an assembly language instruction?

![](https://i.imgur.com/1edDBW0.png)

- Recall:
    - All 32 registers have a name
    - `t` registers indicate *temporary* registers
        - Things you use to do rough work to build up this calculation
        - When done, can put results in memory
        - Meant to trash them as you see fit
        - Most of what you do use these `t` registers
- Left operation is what you *want* to do
- Right assembly code is how you express the operation in assembly
    - `add destination, source, source`

> [!question] How do we translate this assembly code into machine code?

![](https://i.imgur.com/LDRZSDW.png)

- Know that `add` is R-type
    - Know that it is broken down like the image above
- First six digits is opcode $000000$
    - Since R-type
- Last six bits is $100000$ (function code)
- After opcode, it is broken down into 5, 5, and 5
    - For two source registers and one destination

### R-type vs. I-type Arithmetic

| R-type          | I-type  |
| --------------- | ------- |
| `add`, `addu`   | `addi`  |
| `div`, `divu`   | `addiu` |
| `mult`, `multu` |         |
| `sub`, `subu`   |         |

- In general:
    - Some instructions are **R-type**
        - Meaning all operands are *registers*
    - Some are **I-type**
        - Meaning they use an *immediate*/*constant* value in their operation
- `addi`, `addiu`:
    - Have one input from register
    - One input from instruction itself
    - Instruction would send its value around the register file and into ALU directly
        - After sign-extending from 16-bit to 32-bit

<!-- break -->
- % Cannot multiply or divide constants in a single instruction
    - Need to take `addi`; add constant 0; put it in one of the registers
    - Then call `mult` on the register
    - Not including `multi` is a design choice
        - MIPS is a part of RISC architecture
        - If you could do `multi` using two instructions, there is no need for it to be in a single one

### Assembly → Machine Code II

![](https://i.imgur.com/Iu9IMsY.png)

- If you have an operation where you want to add a constant to one register:
    - Must use `addi`
        - The instruction that takes in one destination and one source
        - And a constant value
    - Cannot use `add`
- % Constant can only be a 16-bit number
    - If need to use 32 bits for constant:
        - Need to do some fancy processing
            - Take in the first 16 bits
            - Then shift
            - Then load the next 16 bits

## Logical Instructions

![](https://i.imgur.com/nCs0Jqq.png)

- **ZE**
    - Zero extend
    - Pad upper bits with 0 value
- ? How to do NOT?
    - Use `nor` to do a NOT operation
    - `X nor 0` $= \neg(A + 0) = \neg A$
    - `X nor X` $= \neg(A + A)$
- % These are all *bitwise* operations

## Shift Instructions

![](https://i.imgur.com/UB4LNvz.png)

- Each letter in instruction stands for a word
    - `s`
        - Shift
    - `l`/`r`
        - Left/right
        - Talks about direction
    - `l`/`a`
        - Logical/arithmetic
- Arithmetic shifts
    - Involve the sign
    - If shifting to the right and number is negative:
        - Need arithmetic shift to maintain negative numbers if original was negative
        - Shift it to the right, and put the sign in the empty spot (MSB)
- There is no arithmetic shift for shift left
    - Only reason you need to use arithmetic is to maintain the sign of the number
    - Only happens when shifting right
    - Shift left just adds 0s on the right

- `sra`
    - Make sure the first bit that comes in on the left is the sign bit
        - → Maintain negative numbers if original number was negative
- `sll`
    - “This is not a number; this is just a bunch of 1s and 0s”
    - Shift left and fill empty spots with 0s
- `v`
    - Denotes a *variable* number of bits
        - Specified by `$s`
    - Says: the amount to shift will not be in the instructions, but in the register
    - `a` is a *shift amount*
        - Stored in `shamt` when encoding R-type machine code instructions

- % The majority of the reasons why people shift is to do multiplication and division by 2
    - Faster, easier than using giant multiplication circuit
    - That is why default `>>` (right shift) is *arithmetic*

## Data Movement Instructions

- Recall:
    - Result of multiplication and division is stored in two registers — low and high — *external* to the 32 registers you have access to
    - Cannot refer to them as parameters to any of the other instructions
        - e.g., `add` on low, `sub` from high
- Need something like a getter method for these two registers

![](https://i.imgur.com/WyonqAA.png)

These are the **R-type** instructions for operating on the HI and LO registers described earlier.

- `mfhi`
    - Move from high
    - Put the result in `$d`
- `mflo`
    - Move from low
    - Put result in `$d`
- `mthi`
    - Move to high
- `mtlo`
    - Move to low
- % Not really a good reason for why you would need to use `mthi` and `mtlo`
    - But they exist

## ALU Instructions

> [!note]+ Most ALU instructions are *R-type* instructions.
>
> - Six-digit codes in the tables are therefore the function codes
>     - Opcodes are $000000$
> - *Exceptions* are the **I-type** instructions
>     - `addi`, `andi`, `ori`, etc.

> [!important]+ Not all R-type instructions have an I-type equivalent.
>
> - RISC architectures dictate that an operation does not need an instruction if it can be performed through multiple existing operations
> - e.g., `addi + div -> divi`

### Example: Fibonacci

> [!question]+ How would you convert this into assembly?
>
> - Ignore function arguments, return call for now

```c
int fib(void) {
    int n = 10;
    int f1 = 1, f2 = -1;

    while (n != 0) {
        f1 = f1 + f2;
        f2 = f1 - f2;
        n = n - 1;
    }
    return f1;
}
```

- % Not a particular good implementation
    - More to illustrate how things work in assembly

<!-- break -->
- Initialize some variables
    - `n`, `f1`, `f2`
    - Always happens in the beginning
- Each iteration of `while` loop:
    - Decrement `n` so loop eventually loops stops
    - Updating `n`, `f1`, `f2`

#### Assembly Code

```assembly
# fib.asm
# register usage: $t3=n, $t4=f1, $t5=f2
#
FIB:    addi $t3, $zero, 10     # initialize n=10
        addi $t4, $zero, 1      # initialize f1=1
        addi $t5, $zero, -1     # initialize f2=-1
LOOP:   beq $t3, $zero, END     # done loop if n==0
        add $t4, $t4, $t5       # f1 = f1 + f2
        sub $t5, $t4, $t5       # f2 = f1 - f2
        addi $t3, $t3, -1       # n = n - 1
        j LOOP                  # repeat until done
END:    sb $t4, 0($sp)          # store result
```

- `FIB`:
    - Initialize `$t3` which is `n`
        - Take `$t3` (destination) to be equal to `0 + 10`
        - Use `addi` since we are using a constant `10`
        - & Whenever we want to set a register to some constant, we use `addi` on the `$zero` register and the constant
            - This is why we need `$zero` to always have the 0 value!
    - Do the same for `$t4`, `$t5`
        - For each one, we use `addi` on 0 and the constant
        - % If we used `add`, we need a way to put the constant in some register, then add two registers
            - And the only way you can do that is use `addi` first
            - So might as well just use `addi` to initialize registers
- `LOOP`:
    - % `beq` means **branch if equal**
        - Checks whether two registers have the same value and, if they do, it branches to a specified label
            - Changing the program’s execution flow
        - e.g., Test if `$t3` and `$zero` are equal
            - If equal, jump to `END`
    - Need to take `f1`; make it equal to `f1 + f2`
        - `f1` is stored in `$t4`
        - `f2` is stored in `$t5`
        - `add $t4, $t4, $t5`
    - `f2 = f1 - f2`
        - `f1` is still `$t4`
        - `f2` is still `$t5`
        - `sub $t5, $t4, $t5`
    - `n = n - 1`
        - % We do not have a `subi`
            - & Use `addi` on a negative constant
        - `addi $t3, $t3, -1`
    - % `j` is a **jump** operation

> [!obs]+ There is not a lot of structure.
> - Not a lot of indenting
>     - Going to look like a long line of text
> - ! Except **labels**

- **Label**
    - Put a name on a particular line to indicate that this line has a name
        - e.g., Can see it starts at `FIB`, where the `LOOP` is, and where the `END` is
    - Useful for humans, but also needed for *code* as well
        - Labels used for `beq` and `j` operations

> [!info]+ At the end, we have `sb`
> - **==`sb`==**
>     - **Store byte**
>     - Memory operation
>         - There’s ALU operations, branch/jump operations, and *memory* operations
>         - Have to be able to store and fetch things in memory

## Making an Assembly Program

> [!info]+ Assembly language programs typically have structure similar to simple Python or C programs
> - Set aside registers to store data
> - Have sections of instructions that manipulate this data

> [!tip]+ It is always good to decide at the beginning which registers will be used for what purposes.
> - More on this later

> [!question]+ How to make an assembly program?
> Have to figure out:
> - ? What is the high-level code?
>     - ? What does that translate into?
>     - ? What kind of operations do you need to do?

## Control Flow in Assembly

> [!important]+ Not all programs follow a linear set of instructions.
> - Some operations require the code to **branch** to one section of code or another
>     - `if`/`else`
> - Some require the code to *jump* back and repeat a section of code again
>     - `for`
>     - `while`
> - Need to be able to move to different parts in memory

> [!important]+ We have **labels** on the left-hand side that indicate the points that the program flow might need to jump to
> - Labels
>     - Partly for humans to organize code
>         - Can have, even if not jumping
>     - ! But you *need* labels if you are jumping
>         - To know where it is supposed to go
> - References to these points in the assembly code are *==resolved at compile time==*
>     - To offset values for the *program counter*

## Jump Instructions

![](https://i.imgur.com/UIJOhy9.png)

- `j`
    - The main one you want to know
    - Goes straight to the label
- `jal`
    - Jump and link
    - Indicates that you are going to jump to another line of code, but that code is a function
        - → Jump to that location
            - Execute some stuff
            - Come *back*
        - % `j` does not come back
    - & Register `$31` i.e., `$ra` stores the address that is used when returning from a subroutine
        - i.e., the next instruction to run
- `jalr`
    - Not used very much
- `jr`
    - Jump register
    - Jump to an address stored in a register e.g., `$ra`

> [!note]+ `jr` and `jlr` are jumps, but *not* [[Machine Code Instructions|J-type instructions]]
> - Both do not use labels, but *registers*
> - → R-type instructions

### Calling Functions

- ? How do you **call a function**?
    - & `jal` stores the address of where you came from (program counter value) in `$31`
        - Return address register
    - & Once done that function, you call `jr`
        - Jump to register
        - Put in return address register `$s`
        - → Take you back to whatever called this function
- Think of `jal label` as invoking the **function call**
- Think of `jr $ra` as the **return statement**

### `jr` And `jalr` (Jump to Register)

For the following instructions:

```assembly
jr $ra
```

```assembly
jalr $t0
```

- `$ra` and `$t0` hold address
- Processor moves the *address* stored in `$ra` and `$t0` into the *program counter*
    - Next instruction to be fetched will be at this new address
    - Program will continue from there
- Jump to register is easy
    - Register value is 32 bits (4 bytes)
    - Program counter is 32 bits
    - Take whatever is in the register → Dump it in the program counter

> [!question]+ What happens in the other cases, when the destination address is stored in the instruction?
> - `j` and `jal` (jump to label)

### `j` And `jal` (Jump to Label)

- `j` and `jal` instructions
    - & Address is supplied by the instruction
        - ! Potential problem

![](https://i.imgur.com/JWM2h6z.png)

- J-type instruction
    - 6 digit opcode at front
    - 26 bit address at the back
- If the first 6 bits are occupied by the opcode:
    - ! Remaining bits are *not enough* for a full 32-bit address

> [!question] How do you get around this?
> - No elegant solution
> - Have to take 26-bit value and stick it in a 32-bit register

#### Solution 1: Trailing Zeros

- Jump instructions load new addresses into the program counter
    - *Values* being loaded ==must be divisible by 4==
        - & Every address (binary value) that the program counter might be set to ends with `00`
        - & $\therefore$ No point in storing the last two bits of the address in the instruction

> [!check]+ Solution
> Use the 26 bits in the J-type instructions to store the new PC address, minus the last two zeroes at the end

- Now, we have 28 bits
    - Still not 32!

#### Solution 2: Leading Bits

- Have a 28-bit address
    - 26 bits from instruction + `00` at the end

> [!question]+ What should we use for the first four bits?
> - Several solutions exist
> - MIPS:
>     - & Keep the ==first four bits of the *previous* PC value==

> [!thm]+ Formula
> $$\texttt{pc} = \left( \texttt{pc \& 0xF0000000} \right) | (\texttt{i << 2})$$

- Use the same first four bits from PC
    - To fill up the missing first 4 bits
- New PC value will have:
    - Same first four bits as the original
    - 26 bits from the jump instruction
    - $+2$ bits for shifting it
- When we do `pc & 0xF0000000`:
    - `0xF0000000` is a *mask*
        - When you `&` with zeros, everything is going to be zero
        - Top four bits are ANDed with $1111$
        - Going to zero out the entire `pc` value, except the first four bits
        - Will keep the same first four bits
        - Take that and join it with whatever was in the jump instruction, shifted by 2

![](https://i.imgur.com/jSKtdr9.png)

## Branch Instructions

> [!question]+ How does a branch instruction work?

![](https://i.imgur.com/h2JwMDK.png)

- Control flow statements
    - Instead of code in assembly running one line at a time, you have the ability to “direct traffic”
        - Redo this section
        - Skip over this section
        - Jump into a function
            - Execute that, then
            - Jump back
    - Need to be able to change what program counter is pointing to
        - To jump to another location in memory
- Previously talked about `j` and `jal`
    - To *jump* to a *function*
- & **Branches** are another aspect of control flow
    - Similar to jump instructions
        - Go to another line of code
        - But only if a condition is satisfied
        - Helps implement things like:
            - `if` statements
            - Condition that causes you to stop a `for`/`while` loop

![](https://i.imgur.com/h2JwMDK.png)

- `beq`
    - Branch if equal
    - Tests a condition
    - If condition is satisfied, then go to the label
- Example:
    - Check if `$t0 == $t1`
    - If they are equal, then jump down to the function `end`

### List of Branch Instructions

![](https://i.imgur.com/vaZGP9h.png)

- `bne`
    - Check to see if two provided registers are *not equal* to each other
    - If not equal, then jump down to `label`
    - Otherwise, go to the next instruction
- `bgtz`
    - Branch if greater than zero
- `blez`
    - Branch if less than or equal to zero
- $ `bgtz` and `blez` just use *one* register

> [!tip]+ Branch operations are key when implementing `if` statements and `while` loops
> - **Labels** are (addresses of) memory locations
>     - Assigned to each label at compiled time

> [!question]+ How does a branch instruction work?
> ![](https://i.imgur.com/FXNXuSL.png)
>
> - Used to produce `if` statement behaviour
> - If `$t0 == $t1`:
>     - Go down to `end` label and execute code there
> - Code right below `beq` handles what happens if they are *not* equal to each other
> Alternate implementation using `bne`:
> ![](https://i.imgur.com/3aNWSEB.png)
> - Now does it the opposite way
> - If `$t0 == $t1`:
>     - Continue executing code right below `bne`
> - Otherwise:
>     - Code to execute is below where `end` label is

### Branch’s Immediate (`i`) Value

> [!question]+ How does a branch instruction get translated into machine code?

![](https://i.imgur.com/juK4fd9.png)

> [!info]+ Branch statements are **I-type instructions**
> - % Different from the I-type instructions seen so far
>     - Have been looking at I-type in an arithmetic context
>         - `i` is the immediate, constant value which is added/subtracted/whatever to a register value
>         - Something that is put into ALU to do an arithmetic operation
> - & There is still an arithmetic computation happening
>     - Now: `i` stores an *offset*

> [!info]+ The immediate value `i` is a 16-bit **offset**
> - i.e., ==*relative address* to *add to the current instruction*== if branch condition is satisfied
>     - How much further in memory do I go from where the program counter currently is?
> - ==Not the absolute address== (like with jumps)

- & Calculated as the difference between the current PC value and address of the instruction you are branching to
    - & Stored here as *# of instructions*
        - Not # of bytes
        - Instead: the difference in instructions
            - How many instructions you want to go forward or back
        - ? What is the difference between instructions and bytes?
            - & One instruction is *4 bytes*
                - e.g., if `i` is 4, then you go forward 4 instructions
                    - Equal to 16 bytes
    - & Does not store the trailing `00` if it is not necessary
        - Similar to jump instructions
        - Want to be able to go forwards or backwards some distance
        - If you know that you are always going by a certain number of instructions
            - Distance is always 4, 8, 12, 16 bytes
                - A certain multiple of four
            - Do not want to waste 2 bits storing `00`
        - In the architecture:
            - Right before going to ALU, there is a shift left by 2
                - A mechanism in the datapath
            - Used whenever trying to add this offset to PC
            - If you branch by 1 instruction, the immediate value `i` would be 1
                - After shifting left by 2 bits, the processor calculates a byte offset of 4
                - → Moving the program counter to the next instruction
- & `i` value can be *positive* or *negative*
    - If you are jumping `i` instructions forward, or
    - Jumping `i` instructions backward, respectively
- % Number of instructions is the same as the *number of lines*
    - Every line is occupied by a single instruction
    - Can say how many lines forward you want to go
    - Basically what is stored in the `i` value

#### Calculating the `i` Value

- Offset is computed differently *depending* on the implementation
    - i.e., if PC is incremented by 4 *before* or *after* the branch offset calculation
- Can either:
    - Calculate distance by saying what is the location we are going to *minus* label location we are at right now
        - Take that and distance will be measured in bytes
        - (Arithmetic) shift right by 2 to get number of instructions, or
    -  Look at the label of the next instruction (current PC + 4)
        - Assume that we are going to the next instruction
        - For calculating a branch:
            - Subtract next instruction from label location we are going to

> [!summary]+ Calculating the `i` value
> - If PC is incremented first (and branch target address is relative to current PC):
>     - `i = (label location - (current PC)) >> 2`
> - If branch offset is calculated first (and relative to incremented PC):
>     - `i = (label location - (current PC + 4)) >> 2`

> [!note]+ Assumption for this course
> `i` is computed as
> ```assembly
> i = (label - (current PC)) >> 2
> ```
> - Corresponds to simulator we use for course (MARS, Saturn)

#### `i` In Simulation

Use a simple program in MARS to confirm this:

```assembly
.text
main:    addi $t0, $zero, 1
         beq $t0, $zero, END  # Line 3
         addi $t1, $zero, 1
END:     addi $t3, $zero, 1
```

> [!question]+ What will `i` be for `beq`?
> - When `beq` gets converted:
>     - Converts value of `END` into an actual offset number
> - `END` is *two* lines away
>     - i.e., `END` is 2 instructions down from branch instruction
> - In MARS:
>     - 16 least significant bits of machine code instruction are $0000\,0000\,0000\,0010$

> [!summary]+ This is the way these things store the value in machine code.
> - First six bits are opcode for whatever branch instruction
>     - Since all I-type instructions → first six bits are some non-zero value (reserved for R-type)
> - Next 10 bits are for two register values
>     - If instruction is `beq`:
>         - Comparing two values
>     - If `bgtz` for example:
>         - Only needs one register
> - Then, 16 bits at the end
>     - Talking about an offset you need to add to current location to get to where you need to go if condition is satisfied

### Conditional Branch Terms

- The **branch is taken**
    - When branch condition is met
- The **branch is not taken**
    - When branch condition is not met
    - → Go to the next line
    - ? What is the next PC in this case?
        - The usual $\text{PC}+4$

<!-- break -->
- ? How far can a processor branch? Are there any constraints?
    - Offset is 16 bits
        - Split between going forwards and backwards
        - i.e., signed
    - & Can go forwards $2^{15} = 32,768$ instructions, or
    - & Backwards $2^{15}$ instructions
- ? How much is this in *bytes*?
    - $2^{17}$ bytes forwards
    - $2^{17}$ bytes backwards
    - Bytes is just $4\times$ number of instructions

## Comparison Instructions

![](https://i.imgur.com/XN25Cm4.png)

- Check to see if one register is greater than or less than another register
    - Instead of having to compare if something is greater than zero or less than zero
- Result gets put into a destination register
- % Comparison operations store a `1` in the destination register if the less-than comparison is true
    - Stores a zero in location otherwise
- Useful in combination with branch instructions that only depend on one register
    - e.g., `bgtz` (branch if something greater than zero — in this case, 1)
- If you want to see if something is greater than or equal to a value:
    - Can use one of these (in combination with `bgtz`), or
    - Do a subtraction operation and then test and stop

> [!note]+ There is a signal that goes to the program counter called `PCWrite`
> - `PCWrite`
>     - Writes a new value
> - Another called `PCWriteConditional`
>     - Writes a new value to the PC (calculated by ALU)
>         - Only if particular condition you are testing ends up being satisfied

- e.g., if you want to check if two things are equal:
    - Processor takes both registers
    - Puts it into ALU
    - Subtracts one from the another
        - (Calls a subtract operation)
- Recall:
    - & ALU has four signals: *overflow*, *carry*, *negative*, and *zero*
    - Zero signal comes out of ALU and goes over into an AND gate with `PCWriteConditional`
- If subtraction result is 0:
    - Zero signal goes high
