---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [ALU]
Week: 7
Module:
  - 6 - Processors
Lecture:
  - "20"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-02-26
Date created: Thu., Feb. 27, 2025, 3:19:05 pm
Date modified: Tue., Mar. 11, 2025, 8:09:41 pm
---

# The Arithmetic Unit

## Arithmetic Logic Unit

- The first microprocessor applications were *calculators*
    - Recall unit on [[Adder Circuits|adders and subtractors]]
- These are part of a *larger structure* called the **arithmetic logic unit** (ALU)
    - Responsible for the processing of all data values in a basic CPU

### ALU Inputs

- ALU performs all of the *arithmetic* and *logical* operations covered in course so far
    - e.g., Bitwise XORs, bitwise ORs

![](https://i.imgur.com/KG3c4l6.png)

*Standard diagram for any kind of ALU*

- ? Could you have an ALU that takes more than two values?
    - Usually, yes, conceivably
    - Practically, no
    - Almost all ALUs are seen in this binary way:
        - Combine two inputs into one
    - If you want to combine three, you combine two of them, take the result, and combine the third
    - Just the standard way ALUs are designed
- Variations include:
    - Multiple ALUs in a processor that handle operations in different parts of the processor
    - Essentially, we look at having one core ALU that does everything
    - Any arithmetic or logic operation is going to be computed in this core ALU

- Two inputs `A`, `B` to ALU
- Result `G` coming out of the bottom that combines the two inputs in some way

- Input carry bit $C_{in}$:
    - Used in operations such as incrementing an input value or overall result
- Input $S$:
    - Represents select bits that specify the operation to perform
    - In this case: $S_{2}, S_{1}, S_{0}$
    - Sometimes renamed as $ALU_{op}$

### ALU Outputs

- Output signals $VCNZ$
    - **Flags**
    - Indicate special conditions in the arithmetic result

> [!info]+ $V$
> - **Overflow** condition
> - Result of the operation could not be stored in the $n$ bits of $G$
>     - Meaning that result is incorrect
> - e.g., Adding two large positive numbers
>     - Result can look like a negative number
>     - Same with adding large negative numbers
>         - Result can look like a positive number

> [!info]+ $C$
> - Carry-out bit
> - Used to detect errors in unsigned arithmetic
> - Operation performed has a bit going out of the left-hand side of the ALU

> [!info]+ $N$
> - Negative indicator
> - Indicates that $G$ is a negative value

> [!info]+ $Z$
> - Zero-condition indicator

### ALU Block Diagram

In practice, an ALU is going to look like a large box:

![](https://i.imgur.com/4vHrlfV.png)

- $n$ inputs for $A$
- $n$ inputs for $B$
- $n$ outputs for $G$

- In addition to data inputs and outputs, this circuit also has:
    - Outputs indicating the different conditions
    - Inputs specifying the operation to perform
        - Includes carry input and selects
        - Similar to `Sub` in the [[Adder Circuits|adder/subtractor]]

### ALU Disclaimer

> [!note]+ There are many ways that the ALU can be implemented
> - All implementations do the same general functions
>     - Arithmetic and logical operations
> - Operations that ALU can perform, how is performs them, and specific input and output signals can vary
> - & No standard way of making ALUs

- Will give a couple of implementations (design principles)
    - Keep in mind that others are possible as well

## ALU Design

### ALU from the Labs

(Labs 3 and 4)

![](https://i.imgur.com/o4iKCo2.png)

- In order to support 8 operations, needed an 8-to-1 mux
    - Each operation you might have wanted to perform were on the left
    - ALU would choose between these different *module* operations using `ALUop` as select bits
- Since we were using the same ALU for labs 3, 4:
    - Easy to swap out certain functions for others
    - e.g., Remove module 5 and replace it with some other module operation

- ? Why is this not the best way to approach things?
    - When we discussed subtractors:
        - Could make a subtractor by inverting bits and adding one
        - But a lot of the structure in a subtractor duplicates what was in an adder
    - & ALU designs need to find ways to avoid duplicating logic and reducing the footprint
    - Tools like Quartus reduce these designs automatically when mapping to gates

### Revisiting the “A” of ALU

- When building the ALU operations from scratch:
    - & We start with the arithmetic side
- Fundamentally, side is made of an adder/subtractor unit

![|575x242](https://i.imgur.com/Vw5zdsM.png)

- Full adder circuit on the bottom
- Input $X$ goes into adders
- Input $Y$ coming in from above
- This structure allows us to change back and forth between adder and subtractor
    - If `Sub` was off, then $Y$ goes straight through XOR gates into adder circuit
        - Nothing changes, adds $X + Y$
    - `Sub` signal on → XOR gates invert inputs and adds 1 on the right (two’s complement)
        - Subtractor
    - Must more efficient than having two adder and subtractor modules
        - Duplicating lots of the structure

In the lab, we had to implement an adder and $A + 1$.

- ? Is there a better way to do $A + 1$ that does not require duplicating the entire adder circuit?
    - Instead of `Sub` signal, we had an `Increment` signal
    - Another signal on the right that when turned on, would replace $Y$ with value 1
    - Can use same adder to do addition, subtraction, and incrementing

> [!tip]+ The yellow layer allows us to change the inputs to the adder to make the circuit into a subtractor.
> - If you did more things to the layer, you could also make it into a counter
>     - Counts up each time by replacing $Y$ with value $1$
> - Can make it into a decrementer
>     - Replacing $Y$ with value $-1$

#### Arithmetic Components

![|551x257](https://i.imgur.com/yNsc2QU.png)

> [!tip] In addition to addition and subtraction, many more operations can be performed by manipulating what is added to input $A$, as shown in diagram above.

- $B$ input logic is the block with the set of XOR gates in diagram above, for example

- One of the inputs $A$ goes into $n$-bit parallel adder
    - We do not touch the $A$ input
- Second input $B$
    - ? What if we did certain things to $B$ like we did with the adder/subtractor circuit?
        - Without creating a duplicate of many adder circuits

#### Arithmetic Operations

- If the input logic circuit on the left sends $B$ straight through to adder:
    - Result is $G = A + B$

- ? What if $B$ was replaced by all ones instead?
    - Result of addition operation: $G = A - 1$
    - All ones is the same as $-1$ in signed notation
    - Reverse counter/decrementer
- ? What if $B$ was replaced by $\overline{B}$ and $C_{in}$ was high?
    - Result of addition operation: $G = A - B$
- ? What if $B$ was replaced by all zeroes?
    - Result: $G = A$
    - Not interesting, but useful

> [!important] Instead of a `Sub` signal, the operation you want is signaled using select bits $S_{0}$ and $S_{1}$.

#### Operation Selection

![](https://i.imgur.com/rMaNMEJ.png)

- Can say:
    - Instead of having a single `Sub` bit telling us to switch between adding and subtracting
    - Have two bits $S_{1}, S_{0}$
        - Select between different things we can do to our second input $B$

> [!question]+ What about the carry bit?
> - Some operations (subtraction - 1, transfer) do not seem so useful yet
> - Because we are not thinking about $C_{in}$ which is also one of the inputs in ALU

![](https://i.imgur.com/2o6hw7a.png)

- None of these required us to create duplicate circuits
- Based on the values on the select bits and carry bit:
    - & Can perform any number of basic arithmetic operations by manipulating what value is added to $A$

### The “L” of ALU

> [!info]+ We also want a circuit that can perform **logical** operations, in addition to *arithmetic* ones.
> - ? How do we tell which operation to perform?
>     - & Add another select bit

> [!tip]+ If $S_{2} = 1$, then the output of the logic circuit block appears at the ALU output.
> - Multiplexer is used to determine which block goes to the output
>     - Logical block or arithmetic block

![](https://i.imgur.com/FmkR1ew.png)

- While the arithmetic part has lots of opportunities in reductions in terms of circuitry use:
    - Not much to do on the logical side
- Logical circuits are usually the most reduced form

### ALU: Arithmetic and Logic

![](https://i.imgur.com/hrboIVS.png)

- Arithmetic circuit takes in *two* select bits to decide which operation to do
- Logical circuit that also takes *two* select bits to decide which logical operation to do
- Third select bit $S_{2}$ that chooses if we are doing an *arithmetic* operation or *logical*
    - Says which block output is going to the output $G$

> [!question]+ What about [[Multiplication|multiplication]]?
