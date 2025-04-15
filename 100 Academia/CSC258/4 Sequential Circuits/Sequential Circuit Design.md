---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 4
Module:
  - 4 - Sequential Circuits
Lecture:
  - "12"
  - "13"
Chapter: 
Slides/Notes:
  - "[[flipflops_slides.pdf]]"
  - "[[fsm_slides.pdf]]"
Date: 2025-01-31
Date created: Sat., Feb. 1, 2025, 7:55:06 pm
Date modified: Sun., Feb. 23, 2025, 3:13:28 am
---

# Sequential Circuit Design

![](https://i.imgur.com/tqEIJvq.png)

Now that we know about flip-flops and what they do:

- ? How do we use them in circuit design?
- ? What is the benefit in using flip-flops in a circuit at all?

## Aside: Timing Considerations

### Flip-Flop Timing

> [!info]+ The assumption is that whoever is using a flip-flop is using it properly.
>
> - Input *should not* be changing at the same time as the active edge of the clock
> - Otherwise, you create a **race condition**
>     - Whichever gets there first has an effect on the output value

- **Setup time**
    - Input should be stable for some time before active clock edge
    - i.e., Change input with certain buffer time before clock hits
- **Hold time**
    - Input should be stable for some time immediately after the active clock edge

### Maximum Clock Frequency

![](https://i.imgur.com/y3lkuno.png)

- Have to take into account all delays when calculating the maximum frequency of circuit
- Time period between two active clock edges:
    - Cannot be shorter than **longest propagation delay** between any two flip-flops and **setup time** of the flip-flop
    - & Make sure that your circuit has updated itself fully before putting the next clock cycle in
- If you over-clock your PC:
    - i.e., Tune clock to be faster
    - → Buffer time reduces

## Resetting Inputs

- Flip-flops have unknown state when they are first powered up
    - @ Need a convenient way to initialize them
- **Reset signal**
    - Resets flip-flop output to be 0
    - Unrelated to $R$ input of SR latch
- **Synchronous reset**
    - Output is reset to 0 only on the ==active edge of the clock==
- **Asynchronous reset**
    - Output is reset to 0 ==immediately== (as soon as asynchronous reset signal becomes active), ==independent of the clock signal==

## Example 1: Registers

- Recall [[Adder Circuits|adders]]:
    - Takes multi-bit binary numbers
    - Adds them together
    - Result comes out

> [!goal] When you do a calculation, you probably want to store your output somewhere.

### Shift Registers

![](https://i.imgur.com/ORkXTWE.png)

- **Shift register**
    - A series of D flip-flops
    - Can store a multi-bit value
        - e.g., 16-bit integer
- Data can be *shifted* into this register one bit at a time
    - Over 16 clock cycles
    - e.g., If you are trying to send in a 16-bit number, you are going to have to wait 16 clock cycles to get each bit of the number in

![](https://lastminuteengineers.com/wp-content/uploads/arduino/74HC595-Shift-Register-Working.gif)

<u>Example</u>. Shifting in $0101\,0101\,0101\,0101$

![](https://i.imgur.com/4fdTPqP.png)

### Load Registers

- **Load register**
    - Load a register’s values all at once by feeding signals into each flip-flop

![](https://i.imgur.com/fhFb6lr.png)

*A 4-bit load register.*

> [!obs]+ You have all these registers; you want to be able to tell which registers to load, and which not to load.
>
> - Introduce the **D flip-flop with enable**

![](https://i.imgur.com/CK90iSp.png)

- **D flip-flop with enable**
    - Control when this register is allowed to load its values
- **Enable** signal:
    - Flip flop initially had two inputs:
        - Clock: When things are about to change
        - D: New value written into this flip-flop
    - Enable tells you whether you are storing a value or not
    - Even if clock is saying that it can change, enable signal indicates that flip-flop should/should not hold onto the value it had before

![](https://i.imgur.com/ZZQxpsJ.png)

- Implement the register with these special D flip-flops:
    - & → D flip-flops will maintain values in register until overwritten by setting EN high
- When you have multiple rows of D flip-flops:
    - Enable signal indicates which rows will be set to a new value and which rows hold onto previous values
- % Enable is a *single* signal
    - Goes to all of the D flip-flops in a *single* register
    - Says that this register is going to update all of its bits
    - If this register is turned on, then all of the other registers are implied to turn off

## Example 2: Counters

### Counters

- Consider the **T flip-flop**

![[Latches vs Flip-Flops#T Flip-Flop]]

- & Output is inverted when input $T$ is high

![|240](https://i.imgur.com/b3mqZHz.png)

- ? What happens when a series of T flip-flops are connected together in sequence?
- ? How do we make something where every other bit toggles when the bit to the right goes from 1 → 0
    - Negative-edge transition
        - Most of our flip-flops assume a positive-edge transition
        - We are looking for something that will cause T flip-flop to switch at the moment when bit to the right goes from 1 to 0
    - & Connect the *output* of one flip-flop to the *clock* input of the next

![](https://i.imgur.com/vcJROcx.png)

*A 4-bit **ripple counter**.*

- 4-bit **ripple counter**
    - **Asynchronous** circuit
        - First (left-most) bit is synchronous
        - Every other bit follows an extra delay
        - At every flip-flop:
            - Delay as inputs cause outputs to change
            - Propagation delay from $\overline{Q}$ to next bit’s clock input
    - Timing is not quite synchronized with rising clock pulse
    - Cheap to implement, but
    - Unreliable for timing

<!-- break -->
- A chain of T flip-flops
    - T flip-flop on the left is the least significant bit
        - i.e., Ones digit
        - Goes back and forth based on the clock
    - $ The left flip-flop is the only one that gets its clock input from the clock
        - Everything else gets its clock input from the previous flip-flop’s $\overline{Q}$
- When $Q$ goes from high to low, $\overline{Q}$ goes from 0 → 1
    - Tracing image left to right:
        - Left-most $Q$ goes from 1 to 0 → Second flip-flop will see its clock signal go high
        - Second flip-flop $Q$ goes from 1 to 0 → $\overline{Q}$ goes from 0 to 1 → Third flip-flop $Q$ will toggle
        - And so on

> [!info]+ Timing diagram
> ![](https://i.imgur.com/rsyt1fV.png)
>
> - % Note the ==propagation== delays
>     - Over enough bits, the delays will build

- If you read each flip-flop as a binary number:
    - Starts off $0000 \to 0001 \to 0010 \to 0011 \to 0100 \to \dots$
    - $ This is a **counter**

#### Synchronous Counter

![|588](https://i.imgur.com/9Y7HTG5.png)

- **Synchronous** counter
    - With a slight delay
    - Could be synchronized even more by having each AND gate combine outputs of all previous flip-flops
- ? What are the circumstances that will cause a certain bit to toggle?
    - If you are bit 8, you know you are going to change when every previous bit is high
        - i.e., Bit 4, 2, 1 are all high
    - Generalizing:
        - & If all less-significant bits are high, then this bit is going to change as soon as the clock hits

## Example 3: Counters

- Another case where a counter is also useful is a **timer**
    - If you set a 5 min. timer, you want to have a circuit where you can load a value, and it will count down to zero

> [!goal] Create a circuit that can load a value into it, and then count

![](https://i.imgur.com/mISp622.png)

- Counters are often implemented with:
    - A **parallel load**; and
    - **Clear** inputs
        - An ==asynchronous== reset,
- Load signal:
    - Allows circuit to take some value from $R$ signals
    - Load it into the counter
    - Once load is turned off, then circuit can start counting from that value

> [!info] Loading a counter value is used for countdowns
