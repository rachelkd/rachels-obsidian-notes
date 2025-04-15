---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [big-endian, little-endian, load, store]
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "31"
  - "32"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-24
Date created: Sat., Apr. 12, 2025, 2:43:34 am
Date modified: Mon., Apr. 14, 2025, 8:08:10 am
---

# Memory

## Interacting with Memory

- All of the previous instructions perform operations on registers and immediate values
    - ? What about memory?
- All programs must *fetch values from memory* into registers
    - Operate on them
    - Then *store values back* into memory

> [!summary]+ Two things you ever want to do with memory
> - **Load** something from memory
> - **Store** something into memory

> [!info]+ Memory operations are **I-type instructions**
> With the form:
>
> ![](https://i.imgur.com/EwUD1Iw.png)

- First letter of instruction:
    - `l` if *loading*
    - `s` if *storing*
- Second letter:
    - `b` for (one) *byte*
    - `h` for *half word* (two bytes)
    - `w` for *word* (four bytes; full 32 bits)
- First (local data) register
    - Register that is involved in this operation
    - If loading:
        - Where you will store value from memory
    - If storing:
        - Register that contains the value you want to put into memory
- Second register; register storing address of data value in memory
    - Register that tells you the *address* in memory that you are going to access
- Offset
    - Go forward $x$ many bytes from the starting address (stored in register after offset in instruction)
    - e.g., `12($s0)` says go forward 12 bytes from the address in `$s0`
    - % Most of the time, this value is 0, but sometimes it is not

### Loads vs. Stores

- **Load** and **store** are seen from the perspective of the *processor*
    - Looking at memory

<!-- break -->
- & **Loads** are **read** operations
    - We load i.e., *read* from memory
    - We *load* a value *from* a memory address into a register
- & **Stores** are **write** operations
    - We *store* i.e., *write* a data value from a register *to* a memory address
    - Store instructions ==do not have a destination register==
        - Therefore do not write to the register file

### Memory Instructions in MIPS Assembly

![](https://i.imgur.com/WQWek93.png)

### Load and Store Instructions

![](https://i.imgur.com/NwPDr2R.png)

> [!note]+ Note
> - `b`, `h`, `w` correspond to *byte*, *half word*, and *word*
> - `SE` corresponds to *sign extend*; `ZE` stands for *zero extend*

- $ There are more *loads* than *stores*
    - Two more loads $\because$ there is a `u` version

> [!info]+ `u` says that it is signed or unsigned
> - Happens when you pull down a *byte* or *half-word*
>     - 8 bits from memory, or 16 bits from memory, respectively
> - ! But storing it in a 32-bit register
>     - & Need to indicate whether you fill the rest of the bits with zeros (unsigned) or ones if the number is negative (signed)
> - % Default assumption is that it is a *signed* value

- $ No `u` for `lw`
    - When you pull down 32 bits (a word), there is nothing to pad
    - No extra bits to fill in
- $ No `u` for the store operations
    - Just taking a byte and putting it into a byte
    - Not filling up a full 32 bits
    - No sense that you are filling in 32 bits at a time

> [!note]+ When you are storing stuff, it always takes the ==lowest byte== or ==lowest half word== in the register

![](https://i.imgur.com/oaJOUfD.png)

### Alignment Requirements

- When you access memory:
    - Assumes that if you are putting a word in memory, you are aligning it according to divided sections of *four*
    - e.g., cannot put a word into memory at odd address
    - If putting word data into memory:
        - Looks at all of memory as being divided up into word-sized chunks
        - Can only put this 4-byte value into one of those chunks aligned with the way it divides it up

> [!warning]+ Misaligned memory accesses result in errors
> - *Word* accesses (`lw`, `sw`) should be **word-aligned**
>     - Divisible by 4
> - *Half-word* accesses should only involve half-word aligned addresses
>     - *Even* addresses
> - No constraints for byte accesses

#### Legal Memory Alignment

![](https://i.imgur.com/s1kfFpI.png)

*Same sections of memory, seen from the viewpoint of different memory accesses.*

- **Address error exception**
    - Fetching words and half-words from invalid addresses
    - → Cause processor to raise an address error exception
- % This is also why *addresses stored in PC* need to be *divisible by 4*
    - Instruction fetches are *word accesses* → need to be word-aligned

> [!question]+ How are the *bytes within a word* or half-word stored?

## Little Endian vs. Big Endian

> [!note]+ This comes up in interview questions.
> - Want to test if you know a bit about computer architecture
> - Might ask:
>     - Which do you prefer? Little endian or big endian?

Say we want to read a word (4 bytes) starting from address $X$.

> [!question]+ How do we assemble these multiple bytes into a larger data-type?
> - When you fetch from memory address 0:
>     - Getting 4 bytes: byte 0, 1, 2, 3
> - When bytes are brought over to register:
>     - ? How does it fill in your register with those four byte?

![|200](https://i.imgur.com/33Y8SQW.png)

![](https://i.imgur.com/fnxt34o.png)

- Two ways to look at these bytes in memory:
    - Fill in register with bytes starting with the one at the address you are referencing e.g., byte 0, or
    - Can look at address 0 as the *least significant* byte
        - Most significant byte would be byte D
        - → Want to put byte A in the LSB, and
        - Byte D in the MSB
- % This is unique to each processor

> [!info]+ Endianness is about byte order in *memory*
>
> > [!example]+ Storing the 4-byte hex value `0x12345678` starting at address `0x00`
> > Big endian stores it as:
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> > Address | Value
> > --------|-------
> > 0x00    | 0x12   ← most significant byte
> > 0x01     | 0x34
> > 0x02     | 0x56
> > 0x03     | 0x78   ← least significant byte
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> > Little endian stores it as:
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> >
> > Address  | Value
> > --------|-------
> > 0x00    | 0x78    ← least significant byte
> > 0x01    | 0x56
> > 0x02    | 0x34
> > 0x03    | 0x12    ← most significant byte

- **Big Endian**
     - *Most significant byte* of the word (at address $X$) is stored first
    - Second most significant byte at address $X + 1$ and so on
- **Little Endian**
    - *Least significant byte* of the word is stored first (at address $X$)
    - Second least significant byte at address $X+1$ and so on

![](https://i.imgur.com/u2VuqPi.png)

*Big Endian Example*

![](https://i.imgur.com/T8GN0xd.png)

*Little Endian Example*

- Depending on how processor displays memory:
    - Might see these things in order from LSB to MSB, or
        - (Little Endian)
    - Might see these things backwards
        - (Big Endian)

### MIPS Endianness

- & MIPS processors are **bi-endian**
    - Can operate with either big-endian or little-endian byte order
- % MARS simulator uses the same endianness as the machine it is running on
    - X86 CPUs are little-endian, for instance

## Reading from Devices

- **Offset value** is useful for objects or stack parameters
    - When multiple addresses are needed *relative to* a given **base memory location**

```c
sw $t0, 10($t1)
```

- Offset is `10`
- Base memory location is stored in register `$t1`

<!-- break -->
- When you are trying to make something show up on bitmap display:
    - Involves writing certain pixel values into specific locations in memory
- Have a processor that will have the ability to connect with many different devices
    - e.g., peripheral devices (keyboard and mouse), displays (monitor)
    - @ Need to have a way to send/get values to those devices without having a million different wires coming into processor each from a specific device

> [!info]+ Each of these devices have the ability to send values into memory
> - Not into processor directly
> - Each one is designated in a specific location in memory that it is going to write to
>     - If it is an input device
>     - If output device:
>         - A designated place to send values to in memory for device to read

- & Memory is also used to communicate with outside devices
    - Such as keyboards and monitors
    - Known as **memory-mapped IO**
    - Invoked with a **trap** or function
- Memory location for these devices depend on which ones you’ve got configured and which ones you have put in
    - Also why you need drivers to be able to do this stuff
        - So it has a connection between things that it gets from output port and where in memory it is going to write these values
        - So process can end up reading them

> [!note]+ When it comes to the project…
> - One input device:
>     - Keyboard
>     - Writes into two locations in memory
>         - Flag
>             - Sets to high when new key has been pressed
>         - Value of the key that was pressed
>         - % Check to see if key was pressed; if it has → get key value from other location in memory → do a series of branch statements depending on what the key value was
>         - After the read, sets the flag to low again
> - One output device:
>     - Display something on the screen
>     - There is an address in memory that correlates to the different locations on the bitmap display
>     - If you write a pixel value to some location in memory:
>         - Top left pixel of display will change to whatever colour pixel value is
>     - If you write to the next pixel over:
>         - Will colour the next pixel on the display
>     - A sequence of integer values that you can write into memory that will make these pixels change colour

- Memory is one long column of spaces
    - Can write one integer in a time
    - Different rows and columns in display get mapped to *one, long strip* of integers

### Trap

![](https://i.imgur.com/EwA5bpS.png)

![](https://i.imgur.com/AwzSwyI.png)

- **Trap instructions**
    - Send system calls to the OS
    - e.g., interacting with the user, exiting the program

|              | Trap                           | Syscall                       |
| ------------ | ------------------------------ | ----------------------------- |
| Type         | Hardware/software mechanism    | Specific use of a trap        |
| Purpose      | Transfer control to kernel     | Request OS service            |
| Triggered By | Errors, interrupts, or syscall | Program (intentionally)       |
| Example      | Divide by zero, invalid access | `read()`, `write()`, `exit()` |

- % Trap is similar to `syscall`, but not quite the same

- `trap` and `syscall`
    - Certain operations are built into processor that you can invoke
    - Instead of having to implement everything from scratch
    - e.g., print an integer on screen instead of draw a pixel
- Sleep, for example:
    - Goes to the clock
    - Depending on the time, it will go to the clock to find out how many cycles correlate to a 1 ms pause, for example
        - Will resume whatever the next line was after
    - Will invoke a *rate divider*
        - Rate divider will take in whatever value you pass to know how many clock cycles to count before it gives control back to you

## Memory Segments and Syntax

- Some processors:
    - Instruction and data memory are in separate blocks
- For this one:
    - In the same contiguous block
    - Processor puts all the instructions at the beginning part of memory unit
    - Data is put into the second part
- % Instruction section is much smaller than data section
- & Instruction section comes first; data section comes after
- In code:
    - ! Opposite way around
    - & Declare variables at top of program in section labelled `.data`, then instructions after in section labelled `.text`

> [!info]+ Programs are divided into two main sections in memory
> ![](https://i.imgur.com/xqNAThs.png)
>
> - `.data`
>     - Indicates start of the *data values* section
>     - Typically at beginning of program
> - `.text`
>     - Indicates the start of the program instruction section

> [!info]+ Within instruction section are program *labels* and branch addresses
> - `main:`
>     - Initial line to run when executing the program
>     - % Does not have to be first line in instructions, but usually the sensible way to do it
> - Other labels are determined by the function names used in one’s program

### Labelling Data Values

- Data storage:
    - At beginning of program
    - & Create labels for memory locations that are used to store values
    - Always in form `label    .type    value(s)`
- `.data` section
    - Has a bunch of labels
    - Labels equivalent to variable names

![](https://i.imgur.com/lZydTPT.png)

- `.space` is just saying how much room you have
    - Can fill it up however you need, depending on what you are using it for
    - If you are using `array2` to store characters:
        - Can store 40 characters
        - Each character is one byte
    - If using `array2` to store integers:
        - Can store 10 integers
        - Each character is four bytes
