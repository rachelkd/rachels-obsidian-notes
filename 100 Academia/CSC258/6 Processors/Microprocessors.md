---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 7
Module:
  - 6 - Processors
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-02-24
Date created: Thu., Feb. 27, 2025, 3:19:05 pm
Date modified: Fri., Feb. 28, 2025, 4:16:35 am
---

# Microprocessors

- So far:
    - Have been talking about making devices
        - Adders, counters, registers

> [!goal]+ Ultimate goal
> - Make a **microprocessor**

- ? What is a processor?
    - Core of any kind of computation device
    - Processes things
    - More specifically: processes data
    - Need something that can store data and then process it
    - Do this enough times → we have computation

- **Microprocessor**
    - Digital device that *processes* input
        - Can store values
        - Produces output
        - According to a set of on-board instructions

## Final Destination

![](https://i.imgur.com/NWBK629.png)

- This is what we are aiming for
- Looks messy, but this is nothing we have not seen before
- ALU
    - ALU needs to process data
    - Values of `A`, `B`
- Values of `A`, `B` come from the register file
    - Has 32 registers
    - Going to supply the values for `A`, `B` that go into the ALU
- ALU produces a single output that goes back to the registers
    - Registers provide a value to ALU → ALU computes → Output goes back to the registers
    - Repeat
- ? Where does the register get its values from?
    - Fetched from **main memory**
        - On the left
        - Where the memory stick comes from
        - Memory stick can hold hundreds of thousands of registers
    - If you need to do any kind of computation on the main memory registers:
        - ALU does not talk to main memory directly
        - Fetch things from memory into registers held locally
        - Put it in the ALU
        - Wash, rinse, repeat
        - Result gets fed back into memory
- & Memory is considered to be long-term storage
- Fetch things into the local registers
    - Considered quick-access storage
- Put those values into the ALU multiple times until you come up with your calculation
- Feed that output back into memory
- ? What is the **instruction register**?
    - Holds the instruction that we are currently executing
- ? What is the **program counter**?
    - Also a register
    - Main job is the hold the address of the instruction that we are executing
    - Address would tell us where in memory is the instruction we are looking at
    - Address would get fetched into the instruction register
    - Then, instruction gets executed
- & ALU can choose its inputs from multiple sources
    - To figure out which source will provide the input to `A`, `B`, it has *multiplexers*
    - Multiplexer whose output goes into `A`:
        - Decides if input comes from registers or from program counter
    - Multiplexer whose output goes into `B`:
        - Decides if input comes from registers, instruction registers, or the constant 4
    - These two multiplexers determine what goes into the ALU for our computation
- $ There are also multiplexers going into the registers, memory, program counter

- **Control unit**
    - Big finite state machine
    - Sends signals to all of the data path (below)
    - Tells data path what to do
    - This is what part 2 of Lab 6 is about
        - Coming up with a FSM that can control all of this datapath
- **Datapath**
    - Says what is connected to what
    - Does not actually do anything
    - % Can think of data path as a car, and the control unit as the driver

## Deconstructing Processors

> [!idea] The above diagram can be represented in three major parts

![](https://i.imgur.com/zJQiJyr.png)

- **Storage unit**
    - Thing that stores information
    - Memory and registers
- **Arithmetic unit**
    - Thing that processes information
    - ALU
- **Controller unit**
    - Tells storage and arithmetic units what to do
    - What values will it fetch from storage?
    - How is it going to process them in the arithmetic unit?
    - How is it going to be stored back in the storage unit?
- **Datapath**
    - Connection between things that store information and things that process it
    - Control unit tells the data path how things work

## Datapath vs. Control

> [!def]+ Datapath
> - Where all data computations take place
> - Often a diagram version of real wired connections

> [!def]+ Control unit
> - Orchestrates the actions that take place in the datapath
> - Control unit is a big FSM that instructs the datapath to perform all appropriate actions

### Datapath Example

![](https://i.imgur.com/7LztPIu.png)

Recall Lab 4:

- Created an ALU and a register

![](https://i.imgur.com/2ThGVOo.png)

- ALU stores its output into the register
- Register provides an input value to the ALU
- ALU takes in some value from outside (`A`) and some value `B` from the output of the previous operation

> [!question]+ What if there was more than one register available to store ALU output values?
> - How do we indicate which register to write to?
> - How do we determine the ALU inputs?

- Need to find a system for this
- Have to say:
    - ALU is going into two registers
    - Two registers both feed into ALU

### Lab 4 ALU with Two Registers

![](https://i.imgur.com/kumFDOa.png)

- Only thing that is different from previous circuit with one register:
    - Moved registers above to make it clear that both registers are inputs to the ALU
    - As well as outputs
- Essentially, the same thing
    - ALU feeds its value back up into the two registers `RA` and `RB`
- Since there are more than just two values
    - (Lab 4: One input was from register, one input was from ‘outside’)
    - We have three things:
        - Value of `X` from outside
        - Two registers
    - & Need some multiplexers to say which one of the three values is going to be the input on the left of the ALU and which is the input on the right

![|378x229](https://i.imgur.com/ayciA2o.png)

> [!info]+ Things to consider
> - With two registers, we need to ==indicate which register== (if any) will ==store the ALU output value==
>     - & Register **write/enable signals** `LdRA` and `LdRB` handle this
> - Also need to indicate whether `X`, register `A`, or register `B` will provide input values to the ALU
>     - & **Multiplexers** determine the inputs `A` and `B` of the ALU by setting the values for `SelxA` and `SelAB`
>     - We can make this into any configuration we want
>     - This is just something to start with

### Datapath Signals

![|378x229](https://i.imgur.com/ayciA2o.png)

- If we want to operate this thing to move data around:
    - Use input signals to operate this *datapath*:
        - `LdRA` = Load register A
        - `LdRB` = Load register B
        - `ALUop` = Add vs. multiply
        - `SelxA` = First ALU input
            - 0 → Input comes from `X`
            - 1 → Input comes from `RA`
        - `SelAB` = Select second ALU input
            - 0 → Input comes from `RA`
            - 1 → Input comes from `RB`

> [!question] How do we use this to perform a calculation?

### Example: Calculating $x^{2} + 2x$

- Assume we have an external value `X`
    - Supplied from outside the datapath
    - ? How do we use datapath to calculate $x^{2} + 2x$?
        - Cannot do this in a single clock cycle
        - Maybe over a series of clock cycles, we can build up to this calculation
- Need to coordinate datapath components:
    - **ALU**
        - To add, subtract, and multiply values
    - **Multiplexers**
        - To determine what the inputs should be to the ALU
    - **Registers**
        - To hold values used in the calculations
    - Need to control these things in a sequence to come up with this overall calculation

### One Way to Calculate This

> [!note]+ This is just one approach
> - Several ways that $x^{2} + 2x$ could be calculated on this datapath
> - But all need to start with the same one

- To calculate $x^{2} + 2x$:
    - Load `X` into `RA` and `RB`
        - `X` is coming in from one source
        - In order to do $x^{2}$ or $2x$, we need ALU to have `X` on both inputs
    - Multiply `RA` by `RB`
        - Do $x^{2}$ first
        - Store the result in `RA`
            - Can also put it in `RB`
    - Add `X` to `RA`
        - Still have `X` coming in from outside
        - Store result in `RA`
        - Gives us $x^{2} + x$
    - Add `X` to `RA` again
        - Result from ALU is $x^{2} + 2x$

> [!question] How do we make these operations happen?
> - How does this translate into actual signals?

#### Making the Calculation

- Take the four steps → Translate into four sets of control signals

![|378x229](https://i.imgur.com/ayciA2o.png)

| High-level steps            | Control signals                                                       |
| --------------------------- | --------------------------------------------------------------------- |
| Load `X` into `RA` and `RB` | `SelxA` $=0,$<br>`ALUop` $= A,$<br>`LdRA` $=1,$<br>`LdRB` $=1$        |
| Multiply `RA` and `RB`      | `SelxA` $=1,$<br>`SelAB` $= 1,$<br>`ALUop` $=$ `Mult`<br>`LdRA` $= 1$ |
| Add `X` to `RA`             | `SelxA` $= 0,$<br>`SelAB` $=0,$<br>`ALUop` $=$ `Add`<br>`LdRA` $=$ 1  |
| Add `X` to `RA` again       | `SelxA` $= 0,$<br>`SelAB` $=0,$<br>`ALUop` $=$ `Add`                  |

> [!question]+ Who sends these signals?
> - If you know these are the control signals needed to make this calculation happen:
>     - & Tell a FSM to do it for you

### The Control Unit

- Know which signals need to be turned on and off at every clock cycle
- **Control unit**
    - Essentially a giant Finite State Machine
    - Synchronized to system-wide signals
        - **`clock`**, **`resetn`**
- Have it start from 1
- Have it go down four states
- At each state:
    - Outputs the **datapath control signals**
        - `SelxA`, `SelAB` $\implies$ Control mux outputs (ALU inputs)
        - `ALUop` $\implies$ Controls ALU operation
        - `LdRA`, `LdRB` $\implies$ Controls loading registers RA, RB

- Some architectures also output a **done** signal
    - When computation is complete
    - Yet another output
        - Not shown in our datapaths
- Control unit is a FSM that will do this
    - The idea that you have to control the signals is silly
    - Control unit sends these signals to the datapath
        - Let it know what values to operate on, operation to do, where to store values
- FSM will know to send out one set of signals after another for every clock cycle
- After four clock cycles:
    - End result will be $x^{2} + 2x$

![|734x573](https://i.imgur.com/K6Fom7b.png)

- We look at the whole datapath
    - Registers, multiplexers, ALU
    - Will all be in that one box
- Have some external output `X` come into the datapath
    - Internally will store all of this information, process it, and have these results come out of the right side once some calculation is complete
- In order to determine how this datapath performs its calculation:
    - Needs finite state machine to *send signals*
    - A succession of signals will move data around to make whatever calculation you want happen
- ? How does an FSM do this?
    - Tells you what registers you want to operate on
    - Tells you what operation you want to do
    - Where it gets stored at the end
- & All of these things are operated off the clock
    - Registers are updated when clock hits
    - FSM has sent out its signals
    - Once clock comes, it sets up the next set of signals
    - & FSM has flip-flops that are recording what state it is in
    - Every time the clock comes, it goes from one state to the next to the next
        - Each time sending signals to the datapath to let it know what needs to happen next
- Can imagine that the datapath are receiving these signals from FSM
    - Datapath is ready
    - Knows what registers it is accessing
    - Those have already sent their values to ALU
    - Results come out of ALU
    - Already ready to store into one of the registers
    - Just waiting for the clock to tell it that it can store this value and make the next calculation
- FSM is doing the same thing
    - Has a particular set of flip-flops telling what state it is in
    - Based on that state:
        - Already sending all of the (center) signals to the datapath to tell it what to do
    - As soon as clock comes, it knows that datapath has made that operation happen
    - → Move to next state
    - → Send out the next set of signals
- There are also optional signals, `go` and `done`
    - For whenever the operation starts or stops
    - External to the design choices that we are looking at
        - → We do not really think about these two that much when putting together our circuit

## Microprocessors

> [!idea]+ The idea of a datapath and what it is, and the Finite State Machine is a ==high-level view== of what a **microprocessor** is
> - We look at each of these different parts in more detail to say:
>     - When we make our storage devices, how do we do it?
>     - How is memory structured?
> - We have not discussed how we can store a large amount of registers to store information in an efficient way

- This datapath example is a combination of the major devices we have discussed so far
    - Registers to store values
    - An ALU (adders, shifters, etc.) to process data
    - Finite state machines to control the process
- & **Microprocessors** are the basis of all computing since the 1970‘s
    - Can be found in nearly every sort of electronics

### Microprocessor Components

- To understand the full microprocessor:
    - Visit each major component in depth
    - Starting with the [[The Arithmetic Unit|arithmetic unit]]

![](https://i.imgur.com/JKIKzIS.png)

- Although we have already looked at the ALU, we have not designed it in a way that is the most practical
    - Easier to conceive of
    - But if doing this in real life, what would you actually have to think about when putting an ALU together
