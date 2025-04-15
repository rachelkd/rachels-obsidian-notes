---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Instruction execution, Instructions, PC, Program counter]
Week: 9
Module:
  - 6 - Processors
Lecture:
  - "25"
  - "26"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-03-10
Date created: Wed., Mar. 12, 2025, 1:00:58 am
Date modified: Sat., Mar. 15, 2025, 8:35:44 pm
---

# Instruction Architecture

## Understanding Instructions

- **Instructions**
    - 32-bit binary strings that encode:
        - The operation to perform i.e., **opcode**
            - First 6 bits
        - Other details needed to perform it
            - Remaining 26 bits

> [!note]+ For 64-bit architectures:
> - Instructions are **64** bits long

- Recall:
    - 32-bit processor dictates how big register is
        - Each data value is 32-bits long
    - 64-bit processor says each data value is 64-bits long
- Instructions have this same
    - How big your processor is dictates how wide your registers are
- % We will stick to the 32-bit architecture (to make things for managable)

Other details:

- ? Where are instructions and data values stored?
    - **Main memory**
        - Part of the reason why Big-O analysis counts every time you access a variable or call a function
            - Accessing a variable → accessing something from memory and how expensive it is
            - Executing a function → going into the instruction part of memory → pulling one of these instructions and executing that
    - **Stack** is also stored in memory too
        - More on stack later
- & Instructions are stored *separately* from data values
    - Often identified as the **==`.text`==** segment of memory
- Data values occupy the rest of memory
    - **==`.data`==** segment
- & First instruction to be executed in a program is usually identified with a label **==`main:`==**

> [!important]+ We are using *memory* to store *instructions*.
> - Need to consider instruction execution

## Instruction Execution

In addition to doing whatever operation we are meant to do, we need to do **instruction execution** every time we want to go back to the start state in our [[The Control Unit|control unit]] FSM.

- Responsible for loading up our opcode
- Need to make sure that this is part of the steps of every operation
    - No matter what that operation may be

> [!important] All programs are translated into a sequence of instructions.

To execute these instructions, the control unit continually does the following steps:

1. **Instruction fetch**
    - Bring the next instruction from memory and place it into the *instruction register*
        - i.e., Load the next instruction from memory and store it somewhere that will feed in the opcode to our control unit
        - i.e., Fetch this instruction from memory → put it into our instruction register
            - Will immediately send opcode value to control unit
            - So opcode is ready when we go back to start state (step 2)
2. **Decode instruction**
    - Based on the instruction’s type, determine what operation to perform
    - i.e., Once it is in the instruction register, it will feed an opcode value into control unit and go back up to start state
        - New opcode will tell us what to do (step 3)
3. **Execute instruction**
    - Read the values (contents) of any registers needed from the register file
    - Perform any computations needed in the ALU
    - Access memory if we need to read or write data
    - Write back any data that needs to be stored in memory or registers
    - i.e., With this new opcode → Will know which branch to go down
        - → Go down this branch
        - → Does all the operations
        - Last thing it needs to do is fetch the next instruction
        - Needs to update control unit’s concept of what its current instruction is
            - Have something that is looking at instructions and move to the next one (step 4)
4. **Move** (or jump) to the next instruction in memory
    - If there is a location in memory that control unit is looking at, it needs to update the address of its instruction to look at the next one in the sequence
    - Then back to step 1

These steps lead us to this diagram:

![](https://i.imgur.com/yPH1Qpm.png)

- Recognize some parts of this diagram:
    - Registers in the center
    - Memory is the whole thing on the left
    - ALU is on the right
    - Registers feed into ALU
        - Value can come back into the registers, or
        - Send value to memory (in data memory)
- Now, we are just saying:
    - & Memory can store *instructions* we want to execute as well
        - In addition to data
- Need to be able to fetch an instruction (from instruction memory)
    - → Put it into our instruction register (in the middle)
    - As soon as it is loaded into instruction register:
        - Opcode (first 6 bits) will go into control unit
    - → Control unit will send more signals to tell it to what to do
- $ The instruction register also provides valuable information to the rest of the processor
    - Recall: Have a 5-bit address to tell us source A, B of ALU
        - Who provides this 5-bit address?
            - If writing: where is the address coming from?
        - Data is coming to and from ALU
            - i.e., Register contents are fed into ALU
            - That is fed back to registers
        - ? But what supplies the 5-bit address?
    - & **Instruction registers** tell the registers which registers they are operating on
        - i.e., what ALU source A, B and destination register is going to be
    - & Remaining 26 bits tell registers which registers ALU is operating on, or supply values to ALU directly

## The Program Counter (PC)

> [!info]+ Steps 1 and 4 of instruction execution assume that control unit knows where to find current instruction in memory.
> - Instruction fetch, move (or jump) to next instruction in memory
> - We refer to the address of the instruction in memory
>     - Every time we are done with one instruction, we need to get the next one
>     - → Whatever address of the current instruction was, we need to move to the next instruction address and fetch that
> - Need something that stores address of what instruction we are currently looking at
> - If they are all in sequence in memory, need an address variable that can always be incremented to go from first instruction to next and so on
> - The thing that stores the address is a special register: **program counter**
>     - Just like how instruction register stores instructions

> [!def]+ Program counter
> - Stores the location (memory address) of the current instruction
> - Special register

- When done executing one instruction → Update program counter to increase address stored in there → PC now points to next instruction

![](https://i.imgur.com/520m1il.png)

- Program counter has an address
    - Keeps incrementing every time an instruction is completed
- Instruction register (in middle) stores the instruction
    - But needs something that says *where* the instruction it is going to store is located
- → Address is fetched into instruction register
- → Instruction is fed into control unit

These are just a logical extension of one idea:

- Main idea:
    - Control unit needs to execute more than one instruction
        - More than just what lab 6 does
        - (Which is only step 3 of [[#Instruction Execution]])
- Every thing just follows from this idea:
    - Need to have input coming into control unit
        - i.e., Opcode
    - Opcode has to come from somewhere
        - i.e., Instruction register
    - Instruction register stores the current instruction
    - Sequence of instructions is in instruction memory
    - Program counter tells processor where it is in the sequence

## Updating the Program Counter

> [!question]+ How does the program counter get updated? (Step 4 of instruction execution)
> - Usually instructions are executed in *sequential* order
>     - i.e., one after the other

- Need to update the address that PC stores
- ! Does not mean that PC is incremented by one each time
    - We have to say what every address in memory refers to
        - When talking about memory:
            - Has certain amount of address bits and data bits
            - Address bits refer to location in memory
        - ? How big should one location be?
            - A single bit? A full register (32 bits)?
    - & Memory locations are **byte-addressable**
        - Each **byte** has its own unique address
        - One byte = 8 bits
        - A byte is a unit of space that a single memory address refers to
        - A byte is the smallest amount of memory that you want to draw at one time
            - A single bit is silly; too small
            - 8 bits is the smallest
                - % One byte = 8 bits is the length of a single character (ASCII characters)
                    - If reading a character, just pull down one byte — one location in memory
                - % Integers are 32 bits long (Can store up to $2^{32} - 1$)
- If you are trying to pull down a string (a series of characters in a row):
    - Would get memory location 0, 1, 2, 3, …
- If you are trying to pull down an integer:
    - Would pull down 4 bytes at a time
    - Integer is 32 bits long
    - If every location stores 8 bits, to pull down an integer, you need to store *four* successive locations in memory
        - Since one location is one byte = 8 bits, so four memory locations can store one integer
- Since instructions are 32 bits long (4 bytes):
    - & Instructions addresses would be at $0, 4, 8, 12, 16$, etc.

> [!info]+ The program counter needs to be **incremented by 4** each time it needs to fetch the next instruction.
> - Every instruction ends with the PC update and next instruction fetch

- ! Exception to the $+4$ rule:
    - We do not always execute instructions in *sequential* order
        - Think about if-else statements, loops, function calls
    - Two chunks of instructions; do one or the other
        - PC needs to do the next bit, or jump over to do the next chunk after that
- & Some instructions change the PC differently by jumping to locations in memory
- ? How does it jump?
    - & **==ALU==** needs to calculate the new PC value
    - Branches, jumps, function calls are executed this way
- Recall:
    - One of the signals that the control unit sends is ALU source A, B
    - Source A can say: “I want the first input of ALU to be one of two things: register or program counter”
        - Processor datapath shows a wire from PC to ALU for times you need to update PC to point to next location in memory

![](https://i.imgur.com/PVj6jWh.png)

## Program Counter Integration

![](https://i.imgur.com/Pfar8Wu.png)

- PC feeds into memory unit
    - Tells memory where current instruction is
- Instruction is fetched and put into instruction register
- Instruction register provides 6-bit output which is the control unit opcode input
    - Also provides registers information about what operations are happening
- Final stage:
    - & *ALU* will take PC and update it to the next location of the next instruction in memory

<!-- break -->
- ALU:
    - Traditionally talked about as the thing that processes registers
        - 95% of the time it will
    - But also a few cases where inputs to the ALU is the program counter
        - Will get some value and $+4$ to move down to next instruction, or
        - Add some other value if it needs to jump forwards/backwards a chunk

## Control Unit Signals

Recall the control unit signals from [[The Control Unit#Control Unit Signals|control unit]].
Now the other signals to the control unit make more sense.

![|400](https://i.imgur.com/xRDw9sm.png)

- ==`PCWrite`==
    - Write the ALU output to the program counter
    - Means:
        - “I am finished what I was doing before. I need to update the PC to whatever location it is going to fetch from next, so that I can fetch that instruction and store it in my instruction register”
        - → `IRWrite`
- ==`PCWriteCond`==
    - Write the ALU output to the PC if the ALU’s *zero* condition is true
        - Does the ALU have a zero value coming out of it?
        - If it does, whatever condition it was checking was true → Write PC and jump to some location
- ==`IorD`==
    - Signal whether we are fetching an *instruction* or *data* from memory
        - `IorD = 0` → Fetch an instruction (so that value can go into instruction register with `IRWrite` signal)
        - `IorD = 1` → Fetch data value, so that it can go to the registers
    - Signal mostly asks where processor is getting the from:
        - Is it getting the address of this memory location from the program counter (instruction), or ALU (data value)
- ==`IRWrite`==
    - Load the instruction register
    - Whenever I fetch from memory — there is an instruction — store instruction in the instruction register, so that its contents can go to the rest of the datapath
- ==`PCSource`==
    - Set the source of the PC value
    - From the ALU or from the instruction?

### Control Unit Signal Summary

> [!summary]+ Control unit signal summary
> - `PCWrite`
>     - Write ALU output to PC
> - `PCWriteCond`
>     - Write the ALU output to the PC, only if the *Zero* condition has been met
> - `IorD`
>     - For memory access
>     - Short for “instruction or data”
>     - Signals whether memory address is being provided by the PC (for instructions) or an ALU operation (for data)
> - `MemRead`
>     - Processor is reading from memory
> - `MemWrite`
>     - Processor is writing to memory
> - `MemToReg`
>     - Register file is receiving data from memory
>     - Not from ALU output
> - `IRWrite`
>     - Instruction register is being filled with a new instruction from memory
> - `PCSource`
>     - Signals whether the value of the PC resulting from a jump, or an ALU operation
> - `ALUop` (3 wires)
>     - Signals the execution of an ALU operation
> - `ALUSrcA`
>     - Input A into the ALU is coming from the PC (`ALUSrcA = 0`), or
>     - Register file (value `1`)
> - `ALUSrcB` (2 wires)
>     - Input B into the ALU is coming from the register file (`ALUSrcB = 0`),
>     - A constant value of 4 (`ALUSrcB = 1`),
>     - Instruction register (`2`), or
>     - Shifted instruction register (`3`)
> - `RegWrite`
>     - Processor is writing to the register file
> - `RegDst`
>     - Which part of the instruction is providing the destination address for a register write
>     - `rt` versus `rd`

### Example Control Unit Signals

Consider the assembly code:

```assembly
addi $t7, $t0, 42
```

- Set register #15 = Reg #8 + 42
    - i.e., Take register `t0`,
    - Add a number of value `42` to it, and
    - Store result in register `t7`

<!-- break -->
- `PCWrite = 0`
- `PCWriteCond = 0`
- `IorD = X`
- `MemWrite = 0`
- `MemRead = 0`
- `MemToReg = 0`
- `IRWrite = 0`
- `PCSource = X`
- & `ALUOp = 001` (add)
- & `ALUSrcA = 1`
- & `ALUSrcB = 10`
- & `RegWrite = 1`
- & `RegDst = 0`

## How Things Fit Together Now

![](https://i.imgur.com/ze0B0iO.png)

- Recall our goal:
    - Find out where our **datapath signals** were coming from
    - Someone had to be operating it; correcting values from one place to another
- **Control unit** is a FSM that sends out signals every clock cycle to tell datapath what to do
    - The control unit has many different branches from its start state ($2^{6}$ possible ways)
- ? How does control unit know where to go?
    - Getting an input **opcode**
    - 6-bit value that tells control unit what branch to take
- ? Where is the opcode coming from?
    - **Machine code**
    - A large 32-bit instruction

> [!goal]+ All you need to do is make the **machine code** instruction.
> - Machine code will get opcode into control unit to make the datapath signals

## Intro to Machine Code

- **Machine code** instructions:
    - 32 bits long
    - Made of a sequence of `0`s and `1`s that do not make intuitive sense to humans, but
    - Make sense to the *processor*
        - Could construct these instructions by hand, but most are created when programs are compiled
- As soon as next instruction is fetched from memory:
    - & Rest of the processor starts executing that instruction
        - Control unit receives the opcode it needs
        - Registers receive source and destination addresses
        - ALU receives the operation to perform (sometimes)
- At any one time, instruction memory is filled with these 32-bit instructions
    - Someone had to have crafted these instructions
    - Say you wanted to do that

### Decoding Instructions

Say that we have fetched this 4 byte (32-bit) instruction:

$$00000000 \quad 00000001 \quad 00111000 \quad 00100011$$

> [!question]+ What is it telling us to do?
> - What it does depends on other things
> - Are we working on Intel machine, AMD chip, PS5, Xbox?
>     - Each look at this instruction and interpret it in a different way to tell you what it does
> - This is specified (among other things) in the **Instruction Set Architecture**

- **Instruction Set Architecture (ISA)**
    - Implemented by a given processor
- **Processor microarchitecture**
    - There are different ways to implement a given ISA in hardware
    - Every piece of hardware has its own set of microinstructions that can do whatever operations that the hardware needs to focus on
        - e.g., Xbox and PS5 have more graphical-intensive things
        - AMD, Intel have more generalized functions vs. GPUs

> [!note]+ We use the **MIPS ISA** in our lectures.
> - More on what MIPS is later

### Instruction Decoding

> [!info]+ Each instruction (a.k.a. **control words**) can be broken down into sections that contain all the information needed to execute the operation.

- **Control word**
    - A way of saying that this 32-bit instruction is an instruction that can be executed, and
    - Has a particular operation that it encodes

> [!example]+ Unsigned subtraction: `subu $d, $s, $t`
> $$00000000 \quad 00000001 \quad 00111000 \quad 00100011$$
> - Translates into:
>     - Unsigned subtraction between two registers (`$d`, `$s`)
>     - Storing results in the third register `$t`
>
> ![](https://i.imgur.com/hKM897J.png)
> - Look at the first six bits:
>     - All zeroes → Represents a *register operation*
> - ? Which operation are we doing?
>     - Look at the last six bits from any kind of register operation
>     - Tells you what is being sent to the ALU
>     - $100011$ → Unsigned subtraction
>         - Look at this code in a table
>         - Every operation has a particular code
>
> The 20 bits in the middle are divided up into 5, 5, 5, and 5.
>
> ![](https://i.imgur.com/FpmrAut.png)
> - **Source**
>     - First five and the next 5
>     - Subtracting register `t` from register `s`
>     - We have 32 registers in the register files
>         - Need 5 bits to specify what the first register is, and another 5 to specify the second
> - **Destination**
>     - The bits at `ddddd`
>     - Also need 5 bits to specify what the destination register is
>         - Since there are 32 registers
> - % The last five bits are discarded, because they are not needed
> - $ $00000$ → Register 0
> - $ $00001$ → Register 1
> - $ $00111$ → Register 7
>
> $$\text{Register 7} = \text{Register 0} - \text{Register 1}$$

> [!note]+ Instruction length is usually constrained by the bus width
> - e.g., 32-bit architecture, 64-bit architecture

## Instruction Registers

- **Instruction register**
    - Stores the 32-bit instruction fetched from memory
    - Needs to decode the instruction

![](https://i.imgur.com/NDFSVMe.png)

- Instruction register knows to divide this into different parts:
    - First 6 bits (opcode) specify the operation type
        - i.e., What branch in the FSM does control unit need to travel down
        - Every branch will send out signals to the datapath to tell it how to execute this unsigned subtraction
    - Last 6 bits go to ALU to tell it that it is doing unsigned subtraction
        - Can think of this like the select bits for the mux in the Lab 6 ALU
    - Middle three 5-bit values are sent to the register file
        - Register file has addresses to tell it what inputs $A, B$ are going to be for the ALU
            - The first two 5-bit values: $00000, 00001$
        - Third 5-bit value tells us that we are taking the *results* of the ALU and where we are going to store it
            - $00111$
- % All register operations have the same opcode
    - As far as control unit is concerned:
        - Signals sent for adding two registers and storing value in a third sends the *same* signals as subtracting two registers, or doing an AND
    - Always tells muxes that feed the ALU to get values from registers
    - Always tells register file to write
    - Always tells `MemToReg` signal that we are getting value from ALU (not memory) and put it in the registers instead

## Opcode List

![](https://i.imgur.com/ck7CNWM.png)

This table shows the MIPS instructions and the opcode for each instruction.

- ? What do the yellow codes indicate?
    - & **R-type** instructions
    - % In these cases, opcode is actually $000000$
        - Six digits listed in the table specify the **function code**
        - Last 6 digits in the instruction
- ? What do the `i` instructions mean?
    - e.g., `xori`
    - `i` stands for *immediate*
    - Machine code version of saying a constant
    - Instead of taking two registers and XOR-ing them together, you are XOR-ing one register with a constant
    - ? Where is that constant coming from?
        - R-type instructions break down the instruction into which registers it operates on
        - Instructions that use a constant/immediate value (I-type) break down the instruction to allow for a constant value
            - (See [[#MIPS Instruction Types]])

## MIPS Instruction Types

![](https://i.imgur.com/fcMFifW.png)

- **R-type** instructions:
    - Opcode is $000000$
    - Function is the last 6 bits
    - `shamt` is the *shift amount*
- **I-type** instructions:
    - Opcode is one of the white six-digit codes from table above
- **J-type** instructions:
    - Opcode is one of the green six-digit codes from table
    - & Indicates a jump operation
        - While branches (`b-` instructions) are testing a condition to set the program counter to another location or not:
        - J-type instructions say: just go to another address in memory
        - Gets the address from the rest of the 26 bits in the instruction

### R-type Instructions

![](https://i.imgur.com/4Nn7PaB.png)

- **R-type instructions**
    - Short for *register-type* instructions
        - Because they operate on the registers, naturally
    - Have fields for specifying up to *three* registers and a *shift amount*
        - Two source registers `rs` and `rt`
        - One destination register `rd`
        - Unused fields are usually filed with all zeroes
    - `shamt`
        - Stands for shift amount
        - Stores the number of bits processor needs to shift by (in the case processor is doing a shift operation)
            - Otherwise, `shamt` can be anything, since we are not going to be using it

> [!important]+ The ==opcode== for all R-type instructions is $000000$.
> - Functionf ield specifies the *type* of operation being performed
>     - e.g., `add`, `sub`, etc.
>
> ![](https://i.imgur.com/NYINvah.png)

### I-type Instructions

![](https://i.imgur.com/66PLvzB.png) ![](https://i.imgur.com/wpJtsvh.png)

- **I-type instructions**
    - Instructions have a 16-bit **immediate** field
        - Field is a *constant* value, which is used for:
            - An immediate operand
            - A branch target offset, or
            - A displacement for a memory operand
    - Registers are `rs`, `rt`
        - e.g., If doing arithmetic operation:
            - Source `rs` will be added to `immediate` then stored in `rt`
        - If doing a branch:
            - Check to see if `rs` and `rt` are equal
            - If they are, add `immediate` to program counter
        - If doing something with memory:
            - Whatever value `rs` is
            - Take this address in memory (`rt`) and add `immediate` offset to find out where to write register value to
        - All these operations involve some constant coming from instruction itself

> [!note]+ Branch target offset operations:
> - Immediate field contains the *signed difference* between the current address stored in the PC and the address of the target instruction
> - Offset is stored with the two lower order bits dropped
>     - Dropped bits are always `0` since instructions are always *word-aligned*
>         - i.e., Addresses evenly divisible by 4

### J-type Instructions

![](https://i.imgur.com/U6G6f2P.png)

> [!summary]+ There are only *two* J-type instructions
> 1. Jump `j` ($000010$)
> 2. Jump and link `jal` ($000011$)

- **J-type instructions**
    - Use the 26-bit coded address field to store the target address to jump to
- When writing new destination address in the program counter:
    - First four bits of program counter are left unchanged
    - Last two bits of program counter are set to $00$
        - Since instructions are word-aligned and their addresses always end in $00$
    - Middle section is set to the 26 bits provided by the instruction

## MIPS ISA Attributes

> [!important]+ R-type MIPS instructions have three operands.
> - 2 **source** registers
>     - Acting as data inputs for that instruction
> - 1 **destination** register
>     - Acting as output as in the result of the operation applied on the two source operands will be stored (written) into that destination register

> [!important]+ It is a **load-store** architecture.
> - There are only specific instructions that allow memory access
>     - i.e., Loads and stores
> - Cannot add a value stored in a register with a value stored in memory
>     - Need to first load that value from memory into a register
>         - With an earlier instruction

## Final Notes about the Datapath

![](https://i.imgur.com/PVj6jWh.png)

- We have outlined the major components of this datapath
    - Registers
    - ALU
    - Memory
    - Instruction register
    - Program counter
    - Muxes

> [!question]+ What are the other components of this datapath?
> ![](https://i.imgur.com/ice0cou.png)
