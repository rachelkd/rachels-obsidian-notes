---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
Module:
  - "[[1 - Transistors]]"
Week: 1
Lecture:
  - "2"
  - "3"
Chapter: 
Slides/Notes:
  - "[[transistors_f2024_slides.pdf]]"
Date: 2025-01-08
aliases: []
Date created: Wed., Jan. 8, 2025, 1:10:54 pm
Date modified: Sat., Feb. 22, 2025, 8:31:09 pm
---

# Transistors

> [!important] See [[Lab 1 Preparation]].

## Introduction to Transistors

- **Transistors**
    - Basic building blocks of all computer hardware
        - Control electrical signals
        - Can manipulate technology instead of just turning it on and off physically
        - Can make devices that turn electricity on and off automatically
            - i.e., Use existing electrical values to figure out whether to turn a gate on or off
    - Invented by William Shockley, John Bardeen, Walter in 1947
        - Replacing previous vacuum-tube technology
        - Won Nobel Prize for Physics in 1956
    - Used for applications such as:
        - Amplification
        - Switching
        - Digital logic design

## What Do Transistors Do?

> [!summary] Transistors connect Point A to Point B, based on the value at Point C
>
> - If value at Point C is high, A and B are connected
>     - ![|300](https://i.imgur.com/saWUeWP.png)
> - If value at Point C is low, A and B are not connected
>     - ![|300](https://i.imgur.com/EwpjJlH.png)

### Transistors Are Electrical Faucets

- If `C = 1` (is on), then the output is connected to $0V$
    - ![|492](https://i.imgur.com/YinnhWp.png)
- If `C = 0` (is off), then the output is connected to a different value i.e., $5V$.
    - ![|535](https://i.imgur.com/g0Z4K8n.png)

- Only one can be on at a time, but not both
- Does not work if neither faucet is on

> [!info] See [[Electricity Basics]].

## Using Transistors

### How Transistors Work

> [!def]+ Transistor
>
> - Device made of *[[Electricity Basics#Resistance is Futile|semiconductor]]* material
>     - Can act as a conductor in some cases and an insulator in others
> - A faucet for electricity

- Can be implemented in multiple ways:
    - **MOSFET**
        - Metal Oxide Semiconductor Field Effect Transistor
        - Have three wires that define their behaviour:
            - **Source**, **drain**, **gate**
- Want current to flow from source → drain

> [!info]+ Two main types of MOSFETS we look at in this course
>
> 1. **nMOS transistor**
> 2. **pMOS transistor**

> [!summary]+ nMOS Transistor
>
> - Allows electricity to ==conduct== between the source and the drain when a ==positive voltage (5V) is applied to the gate==
>
> ![|300](https://dwma4bz18k1bd.cloudfront.net/tutorials/NMOS-versus-PMOS-Enhancement-Mode.gif)

> [!summary]+ pMOS Transistor
>
> - Allows electricity to ==conduct== between the source and drain when a ==negative voltage (logic-zero) is applied to the gate==
>     - i.e., Act as a closed switch

![The symbol of (a) a PMOS transistor and (b) an NMOS transistor|381](https://i.imgur.com/ljZwZxg.png)

*The symbol of (a) a PMOS transistor and (b) an NMOS transistor. We use this version for our transistor circuit diagrams.*

- Note that the circle symbol for the pMOS transistor indicates inverting:
    - Low voltage (Logic state 0) → Transistor state ON
    - High voltage (Logic state 1) → Transistor state OFF

![NMOS v.s. PMOS Diagram.](https://i.imgur.com/jbBvt1l.png)

*NMOS v.s. PMOS Diagram.*

## Transistors to Gates

- **Gate voltage**
    - Determines whether the source and drain are connected
- No current flows between source and drain unless source voltage is high
    - Have to have voltage that is trying to make current happen between source and drain
    - Need another voltage to turn on the gate to let current go through

| $V_{DS}$ | $V_{GS}$ | $I_{DS}$                                         |
| -------- | -------- | ------------------------------------------------ |
| Low      | Low      | <mark style="background: #FF5582A6;">Low</mark>  |
| Low      | High     | <mark style="background: #FF5582A6;">Low</mark>  |
| High     | Low      | <mark style="background: #FF5582A6;">Low</mark>  |
| High     | High     | <mark style="background: #BBFABBA6;">High</mark> |

*nMOS Truth Table.*

### Revisiting the Faucet Analogy

![|462](https://i.imgur.com/i2tXVGb.jpeg)

- One tap is turned on if A is high
- Other tap is turned on if A is low
- Imagine both taps are connected to a source of water
    - Left tap: cold water
    - Right tab: hot water
- $Y$ is the drain
- If A is high, one faucet will turn on and the other will not
    - $0V$ faucet is the one that turns on when A is high
    - ![|400](https://i.imgur.com/L0fPz7d.png)
- If $A$ is low, the $\neg A$ faucet will turn on
    - ![|400](https://i.imgur.com/87QXJRX.png)

![](https://i.imgur.com/WQIebMj.png)

- Left faucet is an analogy for nMOS transistor
    - Something that will turn on when input is high
    - In this case, “something” is low voltage coming to the output
- Right faucet is analogy for pMOS
    - Feed high voltage to output when input is low

### Making Gates

> [!note]+ Transistors are not simply on/off switches.
> - [[Logic Gates and Operators|Digital logic gates]] (AND, OR, NOT) are created by a ==combination of transistors==

![](https://i.imgur.com/fMoIEXC.png)

*NOT gate circuit.*

> [!info]+ If $A$ is high
> - ==Bottom transistor is connected==
> - Top transistor is disconnected
> - Output $Y$ is connected to low voltage ($0V$, or ground)

> [!info]+ If $A$ is low
> - ==Top transistor is connected==
> - Bottom transistor is disconnected
> - $Y$ is connected to high voltage ($V_{CC}$)

> [!note]+ Physical Data
> - “High” input i.e., $V_{CC}$
>     - $5V$
> - “Low” input i.e., ground
>     - $0V$
> - Switching time
>     - $\approx$ 120 picoseconds
> - Switching interval
>     - $\approx$ 10 ns
> - NAND is the most common logic gate

## From Transistors to Gates

> [!important]+ Making a gate from transistors is easy. Remember:
>
> - Every gate has ==one output== and ==one or more inputs==
> - Every combination of input values ==must connect the output to either high voltage ($V_{CC} = 5V$)== or ==low voltage ($\text{Ground} = 0V$)==
>     - ! Not connecting the output to high voltage *is not* the same as connecting the output to low voltage!

^76115c

> [!question] What input combinations connect the output to high voltage, and what combinations connect it to low voltage?

### Example. Making Two-Input AND Gates

Consider the truth table for an AND gate:

| A   | B   | Y   |
| --- | --- | --- |
| 0   | 0   | 0   |
| 0   | 1   | 0   |
| 1   | 0   | 0   |
| 1   | 1   | 1   |

- Two inputs → One output
- Both A and B are high $\iff$ Output is high
- If A is low or B is low,
    - Output Y is connected to the ground

![](https://i.imgur.com/AZKJPEm.png)

![](https://i.imgur.com/K25FwTT.png)

*AND Gate.*

### What Gates Do These Make?

> [!note]+ Notes
> - Assume that all the wires labeled A and B refer to the same A and B source voltages.
> - $V_{CC}$ means “common collector voltage” (high voltage $5V$)

![](https://i.imgur.com/Xl3Wc29.png)

- Y is connected to high voltage $\iff$ A and/or B is high
- Y is connected to low voltage $\iff$ A and B are low
- → OR gate

![](https://i.imgur.com/QsBsb3j.png)

- Y is connected to high voltage $\iff$ (A is high and B is low) or (A is low and B is high)
- Y is connected to low voltage $\iff$ (A and B are high) or (A and B are low)
- → XOR gate

### Multiple-Input Gates

> [!question]+ What if we wanted a 3-input OR gate?
> - Output for an OR gate is low $\iff$ All inputs are low
> - If any of the inputs are high, then output is connected to 5 volts ($V_{CC}$)

![](https://i.imgur.com/L5NJLIF.png)

*Three-input OR gate circuit diagram.*
