---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Memory, RAM, Register files, Registers]
Week: 8
Module:
  - 6 - Processors
Lecture:
  - "22"
  - "23"
  - "24"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-03-03
Date created: Mon., Mar. 3, 2025, 1:14:18 pm
Date modified: Sun., Apr. 13, 2025, 3:53:05 pm
---

# The Storage Unit

From [[Multiplication#Expanding Our View to ALUs|last lecture]]:

![[Multiplication#^3c093e]]

- Also known as:
    - The **register file**, and
    - **Main memory**

## Memory and Registers

- *Processor* has *registers* that ==store a single value==
    - Program counters, instruction registers, etc.
    - (See [[Microprocessors]])

![|986x527](https://i.imgur.com/NWBK629.png)

- There are also units in the CPU that store *large* amounts of data for use by the CPU:
    - **Register file**
        - Small number of fast memory units that allow multiple values to be read and written simultaneously
        - Short-term storage
        - e.g., If you need to do some operations with ALU: load whatever values you need into register file
            - Registers can do operations with ALU
            - When done: Store in long-term storage (main memory)
    - **Main memory**
        - Long-term storage
        - Larger grid of memory cells that are used to store the main information to be processed by the CPU
        - This is the memory stick
- If you think about the register file having 32 registers, main memory would have hundreds of thousands of registers
- ? Where are programs and program variables stored?
    - In the main memory
    - Processor will fetch data values from main memory as needed → Put in registers → Do register operations → Send it back to main memory
    - Can almost think of register files as scrap paper
- Registers are embedded inside of the processor
    - i.e., inside the CPU
    - Would not be able to access the registers directly
    - Only microprocessor can control and access these registers
- As a programmer, you only have access to main memory
    - The memory stick
- The processor fetches values from memory

### Analogy

> [!idea]+ If registers are books:
> - *Register file* is the pile of books on your *desk*
>     - Small in number, but available for quick access
> - *Main memory* is like a book on campus
>     - If you want information, you have to go to the library, find it, then bring it back home, put it on your desk
>     - Larger capacity, but takes time to access
> - *Hard drive*/*cache* is a book in a library in Ottawa
>     - Need to transport to Ottawa, and bring it back, storing it in your local library for a little bit, before you bring it back home, and put it on your desk
> - Information from the *Internet* is like getting a book from China

## The Register File

> [!def]+ Register file
> - Stores the data that the processes uses for quick calculations

![](https://i.imgur.com/Pbh8lm7.png)


- For the example we use in this course:
    - Typical register file contains **32 registers**
        - This number will vary depending on your *architecture*
        - 32-bit architecture: Register file limited to 32 registers
            - The fact that they are both 32 is a coincidence
            - Number has more to do with the length of an instruction and how many register values an instruction can access
        - 64-bit architecture: Has way more than 64 registers
- All registers share a single set of input and output wires
    - Like an apartment building with a single shared entrance
- Register file is kind of like an array
    - A bunch of registers put together

> [!question]+ How do we specify a register to read or write?
> - If we wanted to access contents of this:
>     - Need to provide the **address**

### Writing to Registers

- To write a register:
    - Need to ==activate the Write signal== for the desired register
    - Assume we have the number of the register in this file
        - (From $0$ to $31$ in binary)
    - Number is called the **address** of the register

> [!question]+ Given the register’s address, how do we turn on the write signal for that register?
> - Use a **demux** to specify a single register
>     - Based on that register’s address
> - Also known as a **one-hot decoder**
>
> ![|366x335](https://i.imgur.com/mD6teoZ.png)

- Get the address that comes into the demux
    - 32 registers → 5-bit address ($2^{5} = 32$)
- If you are writing to one register, need to have its address decode itself
    - e.g., If $0\,0000$, turn on register 0
        - If $0\,0001$, turn on register 1
        - If $1\,1111$, turn on register 31
- **One-hot decoder**
    - A demux with the value 1
        - Assume this to be the Write enable signal
    - 5-bit address says which register will get a value of 1
        - Rest will be value 0

- See [[Multiplexers#Demultiplexers|demultiplexers]], [[Decoders#^88700d|one-hot decoder]]

#### One-hot Decoders (for Writing)

![](https://i.imgur.com/1neI6CV.png)


- **One-hot decoder**
    - Takes in a $m$-bit binary address to ==activate one of the $2^{m}$ registers== in the register file
- Think of registers as individual boxes in a mailroom
    - Look at the number to figure out where mail needs to go
- & Address will activate one of the registers and tell it to store the value that is coming in
    - & Rest of them will have Write signal turned off → Not take in any data

### Reading from Registers

> [!question]+ How do you read from registers?
> Consider the same approach that was used in writing:
> - **Multiplexer** with address as select bits
>
> ![|327x343](https://i.imgur.com/uq6Dg62.png)

- Recall Lab 6, Part Two:
    - Had four registers
    - Had an ALU with two inputs
        - Needed to be able to say what its inputs were
    - The inputs into the left and right side of the ALU were *two multiplexers*
    - Each mux could see all four registers
    - Could choose which of the four registers was going in to leftor right-hand side of ALU
        - Using two-bit selects
- We use a 5-bit address
    - Since we have $32 = 2^{5}$ registers
    - If we had 4 registers, then address would be 2-bits (like Lab 6)

### Register File Functionality

![](https://i.imgur.com/SwbEYfY.png)

- Orange:
    - To do with writing
- Green:
    - To do with reading

<!-- break -->
- If reading i.e. read signal is high:
    - Have two inputs are $m$-bits long
    - Difference between $n$ and $m$
        - $n$, how wide a single register is vs. $m$, the length of the address
    - & Register A is a 5-bit address to get the $n$-bit value from Register A
        - i.e., Indicates which register is providing the input to ALU input A
    - & Register B is a 5-bit address to say which register is going to provide the input to ALU input B
- Same as the diagram before in [[#Reading from Registers]]
    - But need to provide two addresses instead
    - One that supplies the mux going into left side of ALU
    - Another that provides the right side of the ALU
- Provide an address for A $\implies$ Value goes into input A of ALU
- Provide an address for B $\implies$ Value goes into input B of ALU

<!-- break -->
- If writing i.e. top left $\text{Read} / \overline{\text{Write}}$ signal is low:
    - No longer reading, so the four bottom green inputs and outputs are not used
    - But **destination register** is used
        - $m$-bit address that indicates what register we are writing to
        - e.g., 5 bits in 32-bit architecture
    - Also provide the *data*
    - ? How many bits is the data?
        - 32-bit architecture $\implies$ Each register is 32 bits long
        - 64-bit architecture $\implies$ Each register is 64 bits long
        - While destination register (5-bit address) input is 5 bits, ==data is 32 bits==

### Handling Multiple Registers

- Register unit in our microprocessor stores 32 registers
    - Each register stores a 32 bit value

> [!question]+ How do we access or update a single register?
> - Need to specify ALU input A and ALU input B when performing a *read operation*
> - Need to specify a register to write to (and data value to write) when performing a *write operation*

- & Both are done by specifying the *address* of each register
    - Among the 32 available
    - Each address is 5 bits ($\log_{2}32$)

### Register File - Write Operation

![](https://i.imgur.com/qT4MaXR.png)

> [!note] `Data`, `A`, `B`, and all registers ($R_{0}$ to $R_{3}$) have the same bitwidth (e.g., $n$ bits).

- When doing write:
    - Load Enable signal is turned on
    - → One of these registers will be written to
    - Which one depends on the value that comes into the one-hot decoder
        - Two-bit address will activate one of these four registers depending on the address value
            - If $00$, it will turn on $R_{0}$
            - If $11$, it will turn on $R_{3}$
- Similar to Lab 6, Part Two
    - Except, in the lab:
        - Had different load signal for each register
        - Had to turn them on manually
- Imagine if the number of registers increases e.g., 8 or 16 register
    - ! FSM would be huge because it has a separate output signal for every one of those registers
    - Not practical; does not scale
- Instead:
    - & Decoder helps us say which write enable signal turns on

![](https://i.imgur.com/sQzRk3J.png)

### Register File - Read Operation

![](https://i.imgur.com/ZVDQMhD.png)

- Assume you are doing a read i.e. $R / \overline{W} = 1$
    - Right-hand side gets activated
    - Load enable signal is turned off
        - No registers will be updated with new value
- Will have 2-bit address for ALU input A and another 2-bit address for ALU input B
    - Selects pick which registers is going into the ALU on leftand right-hand sides

## Electronic Memory (RAM)

- Largely the same thing as register files
    - In terms of having addresses that specify a location
    - Data that goes into values of that address
    - Write/read signal that says if you are writing or reading from that location
    - & Made up of a *decoder* and *rows of memory units*

![](https://i.imgur.com/AYokqZe.png)

- **Address width**
    - $m$ such that there are $2^{m}$ rows
    - Decoder takes in an address of $m$ bits
        - Address indicates which one row of the $2^{32}$ memory locations it is writing to
- **Data-width**
    - $n$ such that each row contains $n$ bits
- ? What is the size of this memory?
    - $2^{m} \cdot n$ bits $\implies 2^{m} \cdot \frac{n}{8}$ *bytes*

There are two differences — one minor, and one major difference.

1. There are way more rows than registers in a register file
    - Memory is meant to store things long-term; does not work with ALU directly
        - Whereas, register file is meant for ALU doing some processing
    - You take things from memory → Send to register → Process that data → Send it back to main memory
    - Instead of the address width $m$ being 5 (like in register files), $m$ is closer to 32
        - Have the power of $2^{32}$ registers
    - Address goes into decoder → Decoder has $2^{32}$ outputs → Sets high signal for the corresponding output to write to the memory location at that address
    - If reading:
        - Activate one of the data output lines for reading
        - 32 data lines are going to come from any one of these registers
        - Will have a signal that says which one we connect to
    - (Minor difference)

### Connecting to Memory Units

![|300](https://i.imgur.com/MPPS5sE.png)

- Two things to store values:
    - Registers
    - Memory

While registers are like books on your desk,

- Main memory is like one long street with nothing developed
- Can be divided into sections
- Fill up the street with houses, buildings
    - This is **`malloc`**
- `malloc`
    - Says: “I need a section of memory”
    - Looks to find where this is space for a specified number of *bytes*
        - 8 bits
        - A byte is the smallest amount of memory that you can address
            - Smallest parcel of land that you can take on this street
            - Like buying land, there is a minimum size lot that you can buy

Up until this point, we would say:

- Memory has a value that we want to operate on
    - Everything starts off being in memory
- If you need to do an operation on a memory value:
    - Memory would send that value to the registers
    - Registers would take all memory values that they fetched, then send ones that they are starting to process to ALU
    - Similar to Lab 6: Load registers first, then you can send those values to ALU, and put the output value back into registers
- ? Once you’ve got your calculation, then what are you supposed to do?
    - As far as the lab was concerned, that was the end
    - In real world: Once you’ve gotten that value, something needs to use it
    - Eventually, that value is stored somewhere
        - Not in register, because that is used for temporary calculations, but it has to go somewhere more long-term
    - & Store back into memory

> [!info]+ Memory values are read to the registers, and then processed by the ALU.
> - Results are eventually sent back to memory
>
> ![](https://i.imgur.com/K61qbXJ.png)
> - Every time you store a *variable* in a programming language, it is stored in memory
>     - *Program* is also stored in memory
> - Processor (registers and ALU) are for when you need to do an operation
>     - Get values A and B for ALU from memory → to registers → processed in ALU → sent back
>     - Result is what you see when you run debugger

- You might think that there are two sets of lines
    - One for reading from memory, and
    - One used for writing
    - Actually ==not true==…

> [!important]+ Memory units use the ***==same $n$-bit wires==*** to both send and receive data.
> ![](https://i.imgur.com/pcP5x19.png)
> - One set of data values between memory and processor
> - Line is a *bi-directional* wire
>     - Can use same wire to read from memory and write to memory
>
> <!-- break -->
> - One set of wires for the address
> - One set of wires for the data for both reading and writing
> - Wires that connect processor to memory:
>     - Need to feed into memory (to write) and come out of memory (to read)

> [!danger]+ *Conflicts* arise when multiple sources write to wires at the same time.
> - Need a way to ensure that memory unit does not write to these common wires at the same time that the processor does
>     - Wires that are being used to bring values from memory into registers are the *same* wires being used to send values back into memory

- Not only do we have the issue of values colliding on this wire, but
    - ? If all memory locations are connected to this one set of wires to write to, then what do we do?
        -  Best so far is to have a mux that picks one memory location to go out
        - That is fine, except we are always still writing something
            - Mux only says *which* row is going to go out (to processor), but does not *stop* the value from coming out
        - & Need a way to “cut it off” and stop writing a value from memory onto these wires

### Controlling the Flow

> [!info]+ When reading or writing a memory location, we use the **tri-state buffer** gate
> ![](https://i.imgur.com/t0ZGy3r.png)

| WE  | A   | Y   |
| --- | --- | --- |
| 0   | X   | Z   |
| 1   | 0   | 0   |
| 1   | 1   | 1   |

- **Tri-state buffer**
    - & Sets the output to the input, but only when a *third* signal — **write enable** — is high
    - Will physically cut out the output from the input
    - When `WE` write enable signal is low:
        - Buffer output is a **high impedance** signal
            - Output is ==neither connected to high== voltage or to the ==ground==
            - Output $Z$
- When making transistor circuits:
    - Output has to be connected to something
    - If you do not connect output to high, it does not mean that output is low
    - It is going to be connected to $Z$

> [!question]+ Why would we use this?
> ![](https://i.imgur.com/K61qbXJ.png)
> - Main issue is that all memory locations are connected to the right side wire
>     - At any time, regardless of whether you are reading something or not, something would be sending values out of the right
>     - Need some way of cutting it off
>         - So you can write something into memory
>         - Values would come up the center wire and not collide with any values coming out from memory location
> - ? What if we took the tri-state buffer, and stuck it on the right-hand side of all these memory values?
>     - Kind of the opposite of a write signal

- You would have a write signal on the left-hand side to say which memory location is going to take a new value
- On the right:
    - Would say which one of these values is actually going to be set out the right wire to the processor
    - If you are not reading from memory, then *none* of them; cut them all off
    - Right wire would not have anything coming through that could interfere with values coming up from the processor to memory

### Data Bus

> [!info] This idea of a *shared set of wires* is called a **data bus**.

- **Data bus**
    - A single common of wires
    - Almost like a channel, or highway of values that connects two areas to each other
    - Passageway that data values can go through in two different directions
- & Tri-state buffers allow us to use a *bus* to communicate in both directions between memory and the processor
- & Anything that writes to the bus goes through a *tri-state buffer*
    - e.g., Buffer for a memory location has *high impedance* when:
        - Processor is writing to memory
        - That memory location is not being accessed

When reading from memory:

- & Only one location can write to a bus at a time
    - Called the **bus driver**
    - Other memory locations must have their tri-state buffers turned off

<!-- break -->
- Tri-state buffer
    - Has a write signal that indicates which memory location gets a new value
    - Has a read signal that tells memory which memory location is allowed to send values out
    - Would only ever do one at a time
        - If writing to memory, all tri-state buffers are cut off
            - → Memory locations would only be taking *in* values, not dumping out values
            - Different from mux, where all values would come into the mux, and we pick one
            - In this case, we can pick *none*

### Tri-state Buffer Use

![|500](https://i.imgur.com/W6Qvk5l.png)

*Processor is on the right side; memory is on the left side.*

> [!note] Assume `data_out` and `data_in` connect to a single memory cell.

- Every memory location would have its data going out
    - Basically like a bunch of flip-flops that are sending its values to the output
    - But now:
        - & Output gets stopped by tri-state buffer

<!-- break -->
- If $R / \overline{W}$ is 1 i.e., *read* and `Memory_Enable` is 1:
    - `data` sends data *from memory to processor*, through the buffer
    - Both conditions need to be met for tri-state buffer to “turn on”
        - Have to be doing a read, and this has to be the only location reading from
- Otherwise, if $R / \overline{W}$ is 0:
    - Doing a *write*
    - `data` brings data from processor to memory
        - Since tri-state buffer is disabled i.e., outputting high impedance
    - AND gate output is 0
    - Tri-state buffer is cut off; disconnecting `data_out` from `data` bus

## Memory vs. Registers

- Memory and register are similar in principle:
    - Both store *data values*
    - Both uses *addresses* to specify which value to access

> [!important]+ They are different in three major ways.
> 1. **Usage**
>     - Memory is *much* bigger
>         - Houses most of the long-term values being used by a program
>     - Registers are local data values
>         - Used *internally* by processor to perform an operation
>         - Like scrap paper for a calculation
>             - Discarded when calculation is complete (with some notable exceptions)
> 2. **Access**
>     - Register access is immediate
>     - Memory units are separate from the main processor
>         - Requires more time for a single access
>         - i.e., Several clock cycles
> 3. **Physical structure**
>     - Registers are stored apart from each other
>         - ![](https://i.imgur.com/Rlhc4aQ.png)
>     - Memory is one large block of space where addresses refer to locations within that block
>         - ![](https://i.imgur.com/h09JOcj.png)
>     - Each register address refers to a single 32-bit register
>     - Each memory address refers to an 8-bit location (one **byte**)
