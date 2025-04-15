---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Finite State Machines, FSM, State Machines]
Week: 5
Module:
  - 5 - Sequential Circuits and Finite State Machines
Lecture:
  - "14"
  - "15"
Chapter: 
Slides/Notes:
  - "[[fsm_slides.pdf]]"
Date: 2025-02-05
Date created: Wed., Feb. 5, 2025, 12:34:07 am
Date modified: Wed., Mar. 5, 2025, 2:39:44 pm
---

# State Machines

## Designing with Flip-flops

> [!idea]+ State machines
> - You have flip-flops that govern how a thing should behave next when it gets another input

From [[Sequential Circuit Design#Example 1 Registers|Sequential Circuit Design]]:

- *Counters* and *registers* are examples of how flip-flops can implement useful circuits that store values
- ? How do you design these circuits?
- ? What would you design with these circuits?

> [!important]+ Sequential circuits are the basis for *memory*, *instruction processing*, and any other operation that requires the circuit to remember past data values
> - Past data values are also called **states** of the circuit

- Sequential circuits use ==combinational logic== to determine what the *next* state of the system should be
    - Based on the past state and current input values
- The circuits we have seen so far already have states
    - i.e., Already a version of finite state machines

### State Example: Counters

- Counters:
    - Each state is the current number
    - On each clock tick:
        - Circuit **transitions** from one state to the next
        - Based on the inputs

![|449x179](https://i.imgur.com/KeiYnLe.png)

- What happens if you do nothing?
    - If counter is told not to count, it is going to stay in that state
    - i.e., Hold value of $000$
- If given a signal to increment counter, then change values to be $001$
- Every time the circuit is told not to increment, it stays in that state
- Every time it moves, it goes to the next

> [!note]-  FSMs are different from the FSMs introduced in [[CSC236]]
> - Understanding of these *states* and *transitions* are the same
> - There is no invalid input!
>     - Not trying to reach an end state or accepting state like in CSC236
> - We are creating a ==map== based on current state and input and what state to move to

## State Tables

![](https://i.imgur.com/gk5PpyW.png)

- **State tables**
    - Help illustrate how the states of the circuit change with various input values
    - % Transitions are understood to take place on clock ticks

![](https://i.imgur.com/ANP32uZ.png)

*Same table, but with the actual flip-flop values instead of state labels.*

- % Flip-flop values are both inputs and outputs of circuit here

## Finite State Machines

> [!impl]+ In CSC258, finite state machines are models for an actual circuit design
> - States represent internal states of the circuit, which are stored in the flip-flop values
> - May have seen finite state machines before to describe the grammars of a language, or to model sequence data
>     - Not the case here

- **Finite state machine**
    - Abstract model that captures the operation of a sequential circuit

> [!summary]+ A FSM is defined — in general — as:
> - Finite set of states
> - Finite set of transitions between states
>     - Triggered by inputs to state machine
> - Output values that are associated with each state or each transition
>     - Depending on the machine
> - Start and end states for the state machine
>     - Rarely occurs
>     - Idea of these machines is that they operate forever
>         - e.g., Traffic lights: Red → Green → Yellow → Red

### Example 1: Tickle Me Elmo

- Toy reacts differently each time it is squeezed
    - First squeeze → “Ha ha ha… that tickles”
    - Second squeeze → “Ha ha ha… oh boy”
    - Third squeeze → “HA HA HA”

<!-- break -->

> [!question]- What are the inputs?
> - There is one input
> - Squeeze its belly → Input is $1$
> - No squeeze → Input is $0$

> [!question]- What are the states of this machine?
> - Starting state:
>     - When toy has not been squeezed for a while
> - Move to state that says “that tickles”
> - Move to another state that says “oh boy”
> - Moves to another state that it stays in where it laughs manically

> [!question]- How do you change from one state to the next?
> - See above for transitions
> - Squeeze once: “That tickles”
> - Squeeze twice: “Oh boy”
> - etc.

![](https://i.imgur.com/aCdCtY0.png)

### More Elaborate FSMs

- Normally:
    - FSM has more than one input
    - Will trigger a transition based on certain input values, but not others
- If you have more than one input e.g., two-input system, then every state would have a branching factor of 4
    - i.e., Handle $00, 01, 10, 11$ for every state
- ! Might have input values that do not cause a transition
    - Keep the circuit in the same state i.e., transitioning to itself

### Example 2: Alarm Clock

- Internal state description:
    - Starts in neutral state until timer signal goes off
        - → Clock moves to alarm state
    - Alarm start continues until:
        - Snooze button is pushed → Move to snooze state
        - Alarm is turned off → Move to neutral state
        - Timer goes off again → Move to neutral state
    - In snooze state, clock turns to alarm state when timer signal goes off again

### Example 3: Traffic Lights

![](https://i.imgur.com/gjzANtz.png)

## FSM Design

> [!important]+ FSMs are based on the idea that we are trying to store and represent the state of our circuit, and how this thing changes state as it receives certain inputs
> - Implies that all of these FSMs are different
>
> > [!question]+ How do we go about designing a FSM?
> > - FSMs vary from one implementation to another
> > - Is there a procedure to use for any FSM from *idea* to *implementation*?

> [!tip]+ Design steps for FSM
> 1. & Draw state diagram
>     - You have some idea of a circuit you want to design
>     - Know how it is supposed to behave in response to certain inputs
>     - ? Questions to consider:
>         - What kind of states does this circuit have?
>         - How does it transition from one state to the next?
>         - What input values have to happen to make that transition?
>         - Have you managed to cover every set of possible input values from every state, so that no matter what state you are in, you know what state you are going to next?
> 2. & Derive state table from state diagram
> 3. & Assign flip-flop configuration to each state
>     - Number of flip-flops needed is $\lceil \log_{2}\left( \text{\# of states} \right) \rceil$
> 4. & Redraw state table with flip-flop values
> 5. & Derive combinational circuit for output and for each flip-flop input

- State is a high-level concept of how a thing behaves
    - At the low level, we have flip-flops
        - Each state is represented by a particular set of values on flip-flops
        - As you move state to state, you take whatever flip-flop values were before and setting them to be another set of values to represent “previous state, next state”

### Example 4: Sequence Recognizer

> [!goal]+ Recognize a sequence of input values, and raise a signal if that input has been seen
> - e.g., Three high values in a row
>     - Understood to mean that input has been high for three rising clock edges
>     - Assumes a single input $X$ and single output $Z$

- Example:
    - UPC barcode
        - Encoding that translates into a sequence of numbers below the barcode
        - A thick bar is just a bunch of thin bars next to each other → Looks like a thick bar
        - Bits on the left and right-hand side that are *alignment bits*
        - White = 0, Black = 1
        - On the left, there is always a sequence that begins with 01010
            - i.e., white, black, white, black, …
        - Based on sequence, it knows the width of bars → Uses it to interpret the rest of the bars to determine sequence of numbers
        - Only way that this works is doing a **sequence recognizer** to recognize the first 01010 bars
            - → Knows that there is an alignment → Recognizes the rest of the sequence as a set of encoded numbers

### State Diagram

![](https://i.imgur.com/TYCNRZH.png)

- States are labeled with the input values that have been seen up to now
    - $000$ is when you have seen 0 three times in a row
    - $111$ is when you have seen 1 three times in a row
- Transitions between states are indicated by values on transition arrows
- Every state has two input values that it can get → Two transitions it has to handle
- At some point:
    - Going to create an output for state $111$
    - Since we are looking for the condition that someone has “held a button” for three clock cycles, for instance

### State Table

![](https://i.imgur.com/eFhMxn2.png)

- Make sure state table lists:
    - All states in state diagram, and
    - All possible inputs that can occur at that state

### Assign Flip-flops

- & Flip-flops are the circuit units that are responsible for actually storing states
    - Single flip-flop can store two values — $0, 1$ — thus, two states

> [!question]- How many states can be stored with each additional flip-flop?
> - One flip-flop → Two states
> - Two flip-flops → Four states
> - Three flip-flops → Eight states
> - …
> - Eight flip-flops → $2^{8} = 256$ states
> - If you have six or seven states, you will still need three flip-flops
>     - Just will not be using two or one state, respectively

- In this example, we need to store eight states
    - $3 = \log_{2}(8 \text{ states})$

![](https://i.imgur.com/3P7vxu6.png)

- Based on previous state (from $Q$ outputs) and whatever input into combinational circuit, it determines the next state value and puts it into the $D$ inputs in flip-flops
- All this stuff is synchronized from clock

### State Table

![](https://i.imgur.com/n8pFPir.png)

- Usually:
    - States have names that do not map over to flip-flops so easily
- ! Need to represent each of the states as a flip-flop value
- In this example:
    - Make the actual value stored in flip-flops the same as the state values/labels in the table

![|300](https://i.imgur.com/BkKtjdF.png)

- Four inputs, three outputs

### Circuit Design

> [!tip]+ Treat the three outputs as three different circuits we have to make
> - Similar to what we did for 7-segment [[Decoders#7-Segment Decoder|decoder]]
> - → Make a **[[Karnaugh Maps|Karnaugh Map]]**

#### Karnaugh Map for $F_{2}$

![](https://i.imgur.com/eP0WaLu.png)

$$
F_{2} = F_{1}
$$

> [!note] Left-hand side is the *new* value for flip-flops; right-hand side is implied to be the *previous* value

#### Karnaugh Map for $F_{1}$

![](https://i.imgur.com/jW5RWZV.png)

$$
F_{1} = F_{0}
$$

#### Karnaugh Map for $F_{0}$

![](https://i.imgur.com/j9zmcgA.png)

$$
F_{0} = EN
$$

#### Resulting Circuit

![](https://i.imgur.com/43ClGZz.png)

- ? Where did our combinational circuit go?
    - The “green box” are just the wires
    - Takes the outputs and does some combinational circuit on them to calculate inputs into flip-flops
- Circuit will record the states and make state transitions happen based on input
- ? What about the output value $Z$?
    - $Z$ should go high when $EN$ has been high for three clock cycles in a row

#### Output $Z$

> [!thm]+ Boolean equation for $Z$
> $$
> Z = F_{0} \cdot F_{1} \cdot F_{2}
> $$

- This equation is an example of a **Moore machine**

## Two Types of FSMs

### Moore Machine

> [!def]+ Moore machine
> - Output for the FSM depends solely on the ==current state==
>     - Based on entry actions

- Most of the circuits we are going to make in this course are Moore machines

### Mealy Machine

> [!def]+ Mealy machine
> - Output for the FSM depends on the ==state== and the ==input==
>     - i.e., Output based on *transition* from one state to another
>     - Based on input actions

- ? If a button is never pressed, then you constantly get zeros
    - Looks like state is not changing, but it is constantly changing to the same value

## Race Conditions: Timing and State Assignments

> [!caution]+ When assigning states, you need to consider the issue of timing with the states
> - When changing the value of a flip-flop, do not set the value to something new at the same time the clock comes.
> - Race condition:
>     - ? Which one of these values gets updated first?
>     - Need setup time i.e., put value into flip-flop before clock comes

![](https://i.imgur.com/NeYO0i1.png)

- If recognizer circuit is in state $011$ and gets a $0$ as input:
    - → Moves to state $110$
- Supposedly, the first and last digits change “at the same time”
- If it goes $011 \to 010 \to 110$:
    - [p] You are okay
- If first flip-flop changes first i.e., $011 \to 111 \to 110$:
    - **Intermediate state**
        - Output would go high for an instant
    - ! Could cause unexpected behaviour

> [!check]+ Two possible solutions
> 1. & Whenever possible, make flip-flop assignments such that ==neighbouring states differ by at most one== flip-flop values
>     - Intermediate state can be allowed if output generated by those states is consistent with the output of starting or destination states
> 2. & If the intermediate states are unused in the state diagram, you can set the output for these states to provide the output that you need
>     - Might need to add more flip-flops to create these states

### Example 5: Mouse Clicks

> [!goal]+ Design a circuit that takes in two signals.
> - Signal $P$
>     - High if user is pressing the mouse button
> - Signal $M$
>     - High if mouse is being moved
> - Based on the inputs, indicate whether user is:
>     - Clicking,
>     - Double-clicking, or
>     - Dragging the mouse on the screen

![](https://i.imgur.com/qxq1HRh.png)

- Transitions indicate the values of $P$ and $M$
- Output depends on the state
    - → **Moore machine**
