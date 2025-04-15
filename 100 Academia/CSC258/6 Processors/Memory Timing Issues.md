---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Memory]
Week: 8
Module:
  - 6 - Processors
Lecture:
  - "23"
  - "24"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-03-07
Date created: Fri., Mar. 7, 2025, 4:57:46 pm
Date modified: Fri., Mar. 7, 2025, 11:36:46 pm
---

# Memory Timing Issues

## Register vs. Memory Access

- Unlike registers, memory access can take ==multiple clock cycles== to complete

> [!question]+ Why does memory access require more time?
> - Consider the delays related to flip-flops:
>     - Gate delays
>     - Propagation delays
>     - Setup time
>     - Hold time
> - & Memory units are *physically* distant from the rest of the processor
>     - ! → Bigger delays!

- @ To understand this better, we look at memory read and write operations in more detail

## RAM Memory Signals

> [!summary]+ Inputs
> - **$\text{Read/}\overline{\text{Write}}$ enable**
>     - Or $R / \overline{W}$, or two separate signals
>     - Memory write:
>         - Memory is modified if this signal is *low*
>     - Memory read:
>         - Memory is read if this signal is *high*
> - **Data In**
>     - The data to write i.e., store in memory if $R / \overline{W}$ is *low*

> [!summary]+ Outputs
> - **Data Out**
>     - The data read from memory if $R / \overline{W}$ is *high*

> [!info]+ Additional signals needed for memory units
> Inputs:
> - **Address Port**
>     - Takes in $m$ bits to address $2^{m}$ memory locations
> - **Chip Enable**
>     - Activates memory for read or write
> - **Output Enable**
>     - Accompanies data read
>     - Activates tri-state buffers

### Example: Asynchronous SRAM* Interface

![](https://i.imgur.com/3hzeJjB.png)

- **\*SRAM**
    - Static random access memory
    - Do not need to worry too much about RAM vs. SRAM vs. DRAM vs. SDRAM

| $\overline{\text{Chip Enable}}$ | $\text{Read} / \overline{\text{Write}}$ | $\overline{\text{Output Enable}}$ | Access Type      |
| ------------------------------- | --------------------------------------- | --------------------------------- | ---------------- |
| $0$                             | $0$                                     | $1$                               | SRAM Write       |
| $0$                             | $1$                                     | $0$                               | SRAM Read        |
| $1$                             | $X$                                     | $X$                               | SRAM not enabled |

<!-- break -->
- SRAM takes in an address
- $n$-bit data:
    - Both sending values *in* and *out*
    - Different from how Lab 7 does it
- $\text{Read}/\overline{\text{Write}}$ signal
- $\overline{\text{OE}}$:
    - Indicates if it is actually going to be reading the data signal
- $\overline{\text{CE}}$:
    - More about all of these chips being turned on
    - Cannot turn them all on at the same time
    - Do not need to worry about this signal too much

## Asynchronous RAM: Timing Waveforms

- These signals
    - Have to be managed
    - Need to be sending various signals at various times to make these operations happen
    - & *Timing* is involved here

![](https://i.imgur.com/dXY6tyQ.png)

- & Each memory read and write is done in stages
- & Each stage requires a certain amount of time

## Memory Timing Constraints

- **Data bus** signal
    - Indicates when valid data values are available for reading or writing

> [!question]+ Why aren’t these aligned with $\overline{\text{CE}}, \overline{\text{OE}}, \text{R} / \overline{\text{W}}$?
> - Data bus signal accounts for *delays*
>     - & Time needed to send address or data to memory
>     - & Time to turn $R/\overline{W}$ signal on
>     - & Time to keep data/address on after read or write

- Following signal diagrams illustrate these timing constraints
    - & From the perspective of the memory unit

### Timing Constraints: Reading from Memory

- If you are reading from memory:
    - ! Cannot just send all signals at the same time
        - e.g., Address of which location you are trying to read, write signal
    - There is a certain ordering that needs to happen

![](https://i.imgur.com/BwuQuQq.png)

- You need to send your *address*
    - Before you send read signal
    - Since address needs to go for a certain amount of time before the data is going to be valid
    - If you send your address and expect data to come immediately, it is going to be bad
        - For the first moment, it is going to have the data from the previous read operation
        - After: Certain period of time where data values are still in *flux*
            - 32 bits are coming into these wires from a distance
            - Not all going to arrive at the same time
            - Certain period of **setup time** you need to do to be able to wait for all data values to come in
            - Then you can start reading data from data lines
- This delay is different from what we’ve seen from the past
    - Past:
        - Change a flip-flop; delay is negligible compared to clock speed
    - These delays might require you to take multiple clock cycles just to read a single data value
        - Send address, wait a little bit (for previous value to go away and data bits to settle)
        - After certain amount of time, then you can read what is actually coming from memory and store it into your register
        - Do it too fast → Data value will be corrupted; not going to be what you actually have stored

> [!def]+ Read cycle time: $t_{RC}$
> - Minimum time needed between two read cycles
>     - (min. 10 ns)

> [!def]+ Address access time: $t_{AA}$
> - Time between sending data address and receiving data values

> [!def]+ Output hold time: $t_{OHA}$
> - Time that output data is still valid after sending the next data address

- & This is an **address-controlled read cycle**
    - $\overline{\text{OE}}$ is activated

<!-- break -->
- These are all things you need to incorporate into your design
    - FSM has to wait for how many clock cycles it needs for data to show up

### Timing Constraints: Writing to Memory

Writing is even more interesting.

- You have *three* things to set:
    - Address (of memory location)
    - $R / \overline{W}$ signal
    - Data in (that you are trying to into memory)
- ! Cannot send all three things at the same time
    - If you send all three and are lucky:
        - Address and data will get set up first
        - Then, write signal will turn on

![](https://i.imgur.com/2wAPVeW.png)

- & Need to wait for a certain amount of time before turning on write signal
    - If write signal came before address:
        - Previous address will now have a new value written to it
    - You are sending 32 bits of data to address signal
        - Need all bits to settle before write signal turns on
        - Otherwise, if even one bit is not set up properly, will write to the wrong address
    - $t_{SA}$
- & Need to turn on write signal for a certain period of time
    - To be sure that data is going to be set up
    - $t_{WP}$
- & Need to give enough time to be certain that the data has been written into memory
- & Then, turn off write signal
- & Then, turn off address

> [!def]+ Address setup time: $t_{SA}$
> - Time for address to be stable before enabling write signal

> [!def]+ Address hold time, $t_{HA}$
> - Time for address to be stable after enabling write signal

> [!def]+ Write pulse width: $t_{WP}$
> - Time needed to turn on the write signal to be sure that data has been written

> [!def]+ Data setup time: $t_{SD}$ (to Write End)
> - Time needed for data-in value to be written to memory

> [!def]+ Data hold time: $t_{HD}$ (from Write End)
> - Time that data-in value remains available after write signal

- ? What if we turn off the write signal and address at the same time?
    - If address goes first, then it is still writing to memory at some unintended address
- This timing is one of the big things when it comes to memory
- Memory operations tend to take the longest
    - When calculating $\mathcal{O}(n)$: Calculating how many data operations are you doing
    - How many times are you accessing variables?
    - Every time you access a variable, all of these delays need to be programmed in
    - Make sure that this variable is being written properly
        - Not changing the wrong memory location by accident
    - & Data or memory operations are the most expensive
        - This is why we count these steps when doing time analysis

## Memory Data Transfer

![|300](https://i.imgur.com/T0COh7U.png)

> [!def]+ Load-store architecture
> - Fetch values from memory into the register,
> - Process these values using the ALU,
> - When our overall calculation is complete, we return values back to memory
>
> This is known as a **load-store architecture**.

- % Much more to a processor than this, though

## The Processor Datapath Diagram

![](https://i.imgur.com/PVj6jWh.png)

- **Registers** in the center
- **Memory** on the left
    - Can take its values, store it in registers
    - Registers take values into ALU
    - ALU can send it back into registers as much as it needs to
    - When done, ALU sends things back to memory for *long-term storage*

We now know how computation *works*, but:

> [!question]+ How does it know what computations to do? What controls the memory, registers, and ALU?
> - [[The Control Unit]]
