---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Control unit]
Week: 9
Module:
  - 6 - Processors
Lecture:
  - "25"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-03-10
Date created: Fri., Mar. 7, 2025, 4:57:46 pm
Date modified: Thu., Mar. 13, 2025, 1:33:21 pm
---

# The Control Unit

## Introduction and Motivation

![](https://i.imgur.com/PVj6jWh.png)

- Recall:
    - Overall, our processor looks like this image
    - Have talked about:
        - Things that *store* information: [[The Storage Unit|Registers]] and [[The Storage Unit|Memory]]
        - [[The Arithmetic Unit|ALU]] operates on this information
        - How we get values from the registers to the ALU
        - How the operation performed in ALU can go back to registers, or
        - Go back into memory
    - $ There are other structures here
        - In-between memory and registers: **instruction register**
            - Holds *instructions*
        - On the very left: **program counter**
            - Address of what instruction we are currently looking at

> [!goal]+ Our main focus in this lecture is the **control unit**.
> - Will also talk about the **instruction register** and **program counter**

- Control unit
    - Already something we have worked with in [[Microprocessors|Week 7]]
    - Lab 6, Lab 7
- & Control unit is a FSM
    - Sends signals to the rest of the datapath to tell it what operations we are doing, where are we getting our register from, where is it going to, etc.
- While registers store information and ALU processes information:
    - & Control unit is the thing that *dictates* what kind of operation is currently happening
    - & How a larger computation is going to be performed by doing a set of these operations in succession

### The Processor Datapath

- Operations are executed by turning off various parts of the datapath on and off
    - → To *direct* the flow of data from the correct source to the correct destination

> [!question]+ What tells the processor to turn on these various components at the correct times?
> - **Control unit**

## The Control Unit

> [!def]+ Datapath
> - The **datapath** of a processor
>     - A description/illustration of how the data flows between processor components during the execution of an operation
>     - *Where* data is coming from i.e., source
>     - *Where* it is going to i.e., destination, and
>     - *How* data is being processed i.e., the operation

> [!def]+ Control unit
> - FSM that controls the datapath
> - & Sends *signals* to various processor components to enact all possible operations
>     - (Green lines in the [[The Control Unit|previous schematic]])

> [!summary]+ TLDR
> - Datapath provides the *medium* that these operations are going to take place
> - Control unit *determines* what kind of operations happen

![](https://i.imgur.com/3MXI4MZ.png)

- White wires:
    - Indicate the *datapath*
    - Where data is allowed to move
    - From storage devices to ALU and back
- Green lines at the top:
    - Indicate the *signals* that control unit is sending in order to operate this datapath and make sure datapath knows what operation is taking place

## How Things Fit Together: Datapath and Control Unit

![](https://i.imgur.com/P1XXmgN.png)

- Datapath signals
    - Say where our source and destination are
    - What operations are being performed on them
- Control unit
    - Provides these signals to the datapath
- % This might seem like a simplistic design, but we are going to be expanding on this
- ? What goes into designing this control unit?
    - We know a little bit from Lab 6
    - Created a FSM that sends signals to the datapath
    - Every clock cycle: sends a different set of signals
    - Each clock cycle does one operation
        - e.g., Moving data from one location into ALU and back up
    - At the end of however many operations → Had the calculation you were trying to do

> [!question]+ What needs to be different from Lab 6?
>
> > [!summary]+ FSM from Lab 6
> > - Only supports one operation
> >     - Calculate $Cx^{2} + Ax + B$
> > - Ends when operation is complete
> > - No input signals to FSM
> >     - Only output; only doing one thing; was not depending on user input
> >     - Just assumed that $X$ was providing our four values and was going to do a single thing from start to end
> > - Only performs ALU and data operations
>
> > [!summary]+ Full datapath FSM
> > - & Supports *hundreds* of operations
> >     - e.g., Might have to condition, subtraction, shifting, logical operations, sending things to and from memory
> > - & Once operation is complete → returns to *start* state
> >     - FSM: Instead of ending, it will go back to the beginning and ask: which operation am I doing next?
> > - & 6-bit **operation code** input
> >     - Says which of the hundreds of operations it is going to do
> >     - When it comes back up to the start state, it has a hundred branches to take
> >         - 6-bit input tells it what to do next
> >     - In order to make sure 6-bit input changes every time (do not want to do same operation over and over again):
> >         - ? Who changes this 6-bit input?
> >             - Not the user
> >                 - User is on the outside of things
> >             - Device/computer has to be updating itself to do this or that next
> > - & Also responsible for setting up next **opcode** input
> >     - (Operation code)
> >     - In addition to ALU and data operations
> >     - Has to find out what its next instruction is
> >         - Before going up to start state
> >     - Needs to set itself up to say:
> >         - “Just finished executing one operation; I have to update my own 6-bit input, so that I know what the next operation is”
> >         - Responsible for getting its next instruction and having that instruction loaded
> >         - → When it goes back to start state, 6-bit input has changed

## Control Unit Design

![](https://i.imgur.com/shx4fco.png)

- Lab 6:
    - Like a single path, from start to finish
    - Ends at the finish
- Control unit:
    - Has a starting point
    - Hundreds of branches from that one starting point (because of this 6-bit opcode)
    - & Control unit receives an **operation code** input to determine which branch to take from state state
    - & Next **opcode** value needs to be ready *before* branch terminates and FSM returns to the start state

## Control Unit Signals

Isolate the control unit from the rest of the processor datapath:

![](https://i.imgur.com/xRDw9sm.png)

- $ Control unit has one input: **opcode**
- Number of different output signals
    - Similar to Lab 6: FSM would send bunch of signals to all parts of the datapath to tell it what to do
- This 6-bit input says what operation control unit has to do once it gets back to the start state
- & Control unit takes in the **opcode** from the current instruction
    - & Sends signals to the rest of the processor
- Within the control unit:
    - & FSM can occupy *multiple* clock cycles for a single instruction
    - & Control unit sends out *different signals* on each clock cycle
        - To make the overall operation happen

Of the *thirteen* signals shown here, you should be familiar with:

- ==`MemRead`==
    - Read from memory
- ==`MemWrite`==
    - Write to memory
- ==`MemToReg`==
    - Is the register value coming from the memory of the ALU?
    - Says: “I need whatever is coming from memory to go into my registers”
    - Could it go somewhere else?
        - Yes, but we have not gotten to that point
    - Regardless:
        - Need a signal to say that value fetching from memory (could go to multiple places);
        - Turn this signal high if destination needs to be register
        - i.e., Fetching memory values → Putting it into registers
- ==`ALUOp`== (3 wires)
    - ALU operation
- ==`ALUSrcA`==
    - ALU source A
    - The select bits that go into the muxes
    - Muxes feed into ALU
    - Just like Lab 6: Two muxes that feed into source A and source B
        - Needed a two-bit value going into each mux to say which register is the *source* of the ALU
    - Same thing here:
        - & ALU could be getting stuff from registers, but could be getting from other places too
        - → Need muxes to say what ALU inputs are going to be
- ==`ALUSrcB`==
    - ALU source B
- ==`RegWrite`==
    - There is always some binary value going into data input for register file
    - Does not mean that we want to be writing all the time
    - Need a signal that says:
        - Whatever value is there, is one that I intentionally mean to write into one of my registers
    - When `RegWrite = 1`: Loads one register with new value

## Control Unit Input: Opcode

Before we discuss the rest of the output signals, we should return to the **opcode** input signal.

> [!def]+ Opcode
> - Tells the control unit what datapath signals to send to the processor
> - i.e., to perform a register op

- ? How does processor know what values to operate on?
- & The last thing that every branch in this FSM has to do is to **set up a new opcode**
    - Can think of all branches converging to the same final stages
    - & Final stage: Set up a new opcode, *before* getting back to start state
        - If we go back to start state and it is the same opcode, we keep repeating the same operation forever
    - Need to have the new opcode ready before going back to start state
- Can think of it like there is a *list of operations* processor needs to perform
    - Every time FSM gets to end, it is going to fetch the next operation from this list
    - These operations are called **instructions**
    - There is one structure (in-between memory and registers) called the **instruction register**

> [!important]+ Opcode comes from a 32-bit **instruction** located in an **instruction register**.
> - Encoded in the instruction:
>     - The type of operation to perform
>         - i.e., Opcode, first 6 bits
>     - And all the additional information about the operation that needs to be provided to the rest of the processor
>         - Remaining 26 bits

- Purpose of the instruction register:
    - Store the current operation meant to perform
    - Provides the 6-bits that go into this control unit
        - i.e., Provides the opcode

<!-- break -->
- ? Where does this *instruction* come from?
    - & Instructions come from memory
    - There is a section of memory that is filled with instructions
    - Memory is not only used to store data values, but also:
        - Used to store *instructions*
    - At the end of any FSM branch:
        - Need to send signals to processor that will fetch a value from memory (from this section)
        - Put it into the instruction register
        - Then go back up to start state
        - & Instruction register will give control unit the opcode it needs

> [!important]+ The control unit is responsible for sending out the sequence of datapath signals needed to execute the current operation.
> - Includes loading the next instruction to run

- There is no way to introduce these things in isolation
    - Instruction registers, control unit, memory, etc.
- If a FSM has to do more than one “branch,” by extension:
    - → Has to get a signal from outside to tell it what branch to take
    - If getting a signal from outside:
        - → Has to be able to change that signal every time it goes back to the start state
    - → Has to change what the input signal to FSM is
    - → List of operations has to be stored somewhere
    - → Need to be able to load up the next instruction every time control unit goes back to start state
    - → List of operations need to be stored somewhere
    - → Need mechanism to fetch them somewhere
    - → Need to have fetched instruction to feed control unit the next opcode

- FSM needs to handle multiple branches
- → There is a logical set of *dependencies* that results in:
    - Having to store a bunch of instructions somewhere, which
    - Will provide the FSM the information it needs to perform each operation in the sequence

See [[Instruction Architecture]].
