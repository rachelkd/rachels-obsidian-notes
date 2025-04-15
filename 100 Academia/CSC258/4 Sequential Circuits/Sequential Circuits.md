---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: [Latches, SR Latch]
Week: 4
Module:
  - 4 - Sequential Circuits
Lecture:
  - "10"
  - "11"
  - "12"
Chapter: 
Slides/Notes:
  - "[[flipflops_slides.pdf]]"
Date: 2025-01-27
Date created: Sun., Jan. 26, 2025, 8:28:48 pm
Date modified: Mon., Feb. 24, 2025, 5:13:01 pm
---

# Sequential Circuits

> [!idea]+ Something to consider
>
> - Computer specs use terms like “8 GB of RAM” and “2.2GHz processors”
> - ? What do these terms mean?
>     - **RAM**
>         - Random Access Memory
>         - 8 GB = 8 billion bytes
>     - 2.2 **GHz**
>         - 2.2 billion clock pulses per second
> - ? What does this mean in circuitry?
> - ? How do you use circuits to store values?
> - ? What is the purpose of a clock signal?

## Two Kinds of Circuits

- We have dealt with [[Week 2 - Circuit Creation|combinational circuits]]
    - Circuits where the output values are entirely dependent and predictable from current inputs

> [!def]+ Sequential circuits
>
> - Another class of circuits
> - Circuits that also depend on both the ==current== inputs and ==previous state== of the circuit

- → Creates circuits whose internal state can change over time
    - Same input values can result in different outputs
- ? Why would we need sequential circuits?
    - Memory values
        - Memory has ability to remember what its values were in order to be able to hold onto values from one moment to the next
    - Reacting to changing inputs
    - & Anything has requires a *sequence* of operations to be done
        - Needs to remember where you were in the sequence to know where you go next

> [!important]+ Being able to hold onto values, preserve previous outputs, and use that to calculate current output is a big deal!
>
> - We are going to extend our combinational circuits

## Creating Sequential Circuits

> [!info]+ Essentially, sequential circuits are a result of having *feedback* in the circuit.
>
> - ? How is this accomplished?
> - ? What is the result of having the output of a component or circuit be connected to its input?

![](https://i.imgur.com/ib61nfN.png)

- Output comes back in
    - Previous output values are apart of the input to the circuit

![](https://i.imgur.com/cymoDbK.png)

- Feedback means a separation of the combinational circuit
    - Combinational circuit takes in whatever inputs it gets
        - Does not differentiate between having them from outside or from storage units
    - → Produces a set of output values
- Storage units:
    - We need to work on a circuit that can actually store values, so
    - Combinational circuit can tell us what the new values in the storage unit will be
    - Storage unit will tell the combinational circuit what previous state was

## Gate Delay

- Up until this point:
    - Assumed that a change of inputs would result in an immediate change of outputs
    - Not strictly true
- Now, that we are discussing sequences of events, we need to talk about **time**

> [!important]+ Outputs do not change instantaneously
>
> - Even in combinational circuits

> [!def]+ Gate Delay or Propagation Delay
>
> - Length of time it takes for an input change to result in the corresponding output change
> - **Gate delay**
>     - Whatever delays take place in the ==gate==
>         - e.g., Transistors need to be able to charge up to change the output in response to the input
> - **Propagation delay**
>     - Things do not change instantaneously on ==wires== that connects output of one to another

![](https://i.imgur.com/ghKeGMZ.png)

- We assume that the outputs change instantaneously
    - Output $Y$ goes at the same instance that $A$ and $B$ go high
- Time is measured as $T$
    - In computers, we talk about *discrete* things
    - There is a sense of time (which is normally *continuous*), but
    - Time is going to be in small intervals in this context

## Feedback Circuit Example

![|300](https://i.imgur.com/VBwW09O.png)

- Some gates do not have useful results when outputs are fed back on inputs
    - AND, OR gates

### AND

![](https://i.imgur.com/GEOgY9K.png)

- If $A = 0$:
    - $Q_{T + 1}$ becomes 0 no matter what $Q_{T}$ was
- ? What happens next for later values of $A$?
    - $Q_{T + 1}$ gets stuck at 0
    - Cannot change

![](https://i.imgur.com/E3qOqO4.png)

> [!note] $Q_{T}$ and $Q_{T + 1}$
>
> - Represent the values of $Q$ at a time $T$, and
> - A point in time immediately after ($T + 1$)

### OR

![](https://i.imgur.com/VgMTQ6z.png)

- If $A = 1$:
    - $Q_{T + 1}$ becomes 1 no matter what $Q_{T}$ was
- ? What happens next for later values of $A$?
    - $Q_{T+1}$ gets stuck at 1

![](https://i.imgur.com/ExQSmSq.png)

### NAND and NOR

![](https://i.imgur.com/s8Ds6P4.png)

> [!info]+ NAND and NOR gates
>
> - NAND, NOR gates with *feedback* have more interesting characteristics
> - → Lend themselves to storage devices
> - Output $Q_{T + 1}$ in NAND, NOR feedback circuits can be ==changed== based on $A$
>     - Unlike AND, OR gate circuits, which get stuck

#### NAND

- Assume we set $A = 0$
    - Then output $Q$ goes to $1$
    - & If we leave $A$ unchanged, we can store 1 indefinitely
- If $A = 1$:
    - $Q$‘s value can change
        - If $Q_{T} = 1$, then $Q_{T + 1} = 0$
        - Then, $0$ is fed back to the input and $Q_{T + 2} = 1$
        - And so on, oscillating
    - ! **Unsteady state**

![](https://i.imgur.com/RpEWz6k.png)

- % Note the different values for $Q_{T + 1}$ when $A = 1$

#### NAND Waveform Behaviour

![](https://i.imgur.com/WlVreto.png)

- Since $A = 0$ at start, $Q$ starts with high output
- As soon as $A = 1$, $Q$ oscillates
    - As fast as physics allows

#### NOR

![](https://i.imgur.com/uoVqjty.png)

- Assume we set $A = 1$
    - Output $Q$ will go to 0
    - & If we leave $A$ unchanged, we can store 0 indefinitely
- If $A = 0$:
    - If $Q_{T} = 0$, then $Q_{T + 1} = 1$
    - Then, $Q_{T + 2} = 0$
    - And so on
- ! **Unsteady state**

![](https://i.imgur.com/lbcGjsQ.png)

> [!obs]+ NAND and NOR Feedback circuits
>
> - Output $Q_{T + 1}$ can be changed
>     - Based on $A$
> - However, gates like these that feed back on themselves could enter an **unsteady state**

> [!goal]+ We need some way of stabilizing states
>
> - When we are interested in changing the value, we can do so in a way that does not cause it to flip back and forth between 0 and 1
> - ? What if we use multiple gates of these types?

## Latches

- Use multiple gates of these types to get more ==steady== behaviour

![](https://i.imgur.com/nrqUosB.png)

- **Latches**
    - One gate will help stabilize the other one
    - Not driving its own value, but rather they are driving each other

### $\overline{S} \, \overline{R}$ Latch

- $\overline{S}$
    - **Set**
        - Turn $S$ off to set
- $\overline{R}$
    - **Reset**
        - Turn $R$ off to reset

<!-- break -->
- Assume that $\overline{S}$ and $\overline{R}$ are set to 1 and 0 to start
    - $\overline{R} = 0$: Reset condition
    - ? What are we “resetting”?
        - Output $Q$

![](https://i.imgur.com/5qeNKiy.png)

- Cannot quickly determine output of $Q$ yet
- Consider the bottom gate with $\overline{R}$ input
    - $\overline{R} = 0$
    - Then, ==output $\overline{Q}$ is 1==
    - Does not matter what the other output is:
        - As long as one input is zero, the output is 1
            - (NAND operation)
            - Output is low *iff* both inputs are high
- Top gate has inputs $\overline{S} = 1$ and $\overline{Q} = 1$
    - & Output $Q$ is 0
    - Verify:
        - $Q = 0$ feeds back into input of bottom NAND gate
        - $0 \texttt{ NAND } 0_{\left[ =\overline{R} \right]} = 1$
        - → Stabilized

![](https://i.imgur.com/E1CPhyJ.png)

- Consider changing input $\overline{R} = 1$
    - ? Does this make a difference?
        - No; input from $Q$ feedback is 0, output $\overline{Q}$ is still 1
        - & Setting $\overline{R} = 1$ keeps the output value $\overline{Q} = 1$
        - Maintains both output values

![](https://i.imgur.com/sH1yZEz.png)

- Consider changing $\overline{S} = 0$ (starting with $\overline{S}, \overline{R} = 1$)
    - Sets output $Q = 1$
        - $\because$ We have one 0 input
    - (Note that $\overline{Q} = 1, \overline{R} = 1$ still)
    - $Q = 1$ feeds into bottom gate
    - → $\overline{Q} = 0$

![](https://i.imgur.com/VSwkctp.png)

- Setting $\overline{S}$ back to 1 keeps the output value $\overline{Q} = 0$
    - Maintains both output values

![](https://i.imgur.com/GiTIUV9.png)

> [!obs]+ Inputs of $11$ maintain the previous output state.
>
> - Circuit remembers its signal when going from $10$ or $01$ to $11$

- ! Going from $00$ to $11$ produces unstable behaviour
    - Output depends on which input changes first

> [!idea]+ Think of this as one light switch that turns the light on, and one light switch that turns the light off.
>
> - One switch only has the ability to turn things on
>     - Set
> - Other switch only has the ability to turn things off
>     - Reset

### SR Latch

![](https://i.imgur.com/joL5LHG.png)

- NOR gates
    - Output high iff inputs are both 0
- In this case:
    - & Circuit remembers previous outputs when going from $10$ or $01$ to $00$

![](https://i.imgur.com/E3SbxTP.png)

- If you want $Q = 1$:
    - Make $S = 1$
- If you want $Q = 0$
    - Make $R = 1$

#### SR Latch Timing Diagram

Recall: Output signals do not change instantaneously.

![](https://i.imgur.com/63Tu2L8.png)

- Always going to be a delay between when you change inputs and outputs
- $S, R$ might start off at 0
- As soon as you change $S = 1$:
    - $\overline{Q}$ changes a moment later
    - A moment after that, $Q$ changes

### More on Instability

- When does *unstable behaviour* occur?
    - When $\overline{S}\,\overline{R}$ latch’s inputs go from $00 \to 11$
    - When $SR$ latch’s inputs go from $11 \to 00$
- & Signals do not change simultaneously
    - Outcome depends on which signal changes first

> [!info]+ Forbidden States
>
> - Due to unstable behaviour, we have **forbidden states**
>     - NAND-based $\overline{S}\,\overline{R}$ latches: $00$
>     - NOR-based $SR$ latches: $11$

- You cannot change two inputs at the same time
    - One always goes first
    - **Race condition**

### Introducing the Clock

- We now have circuit units that can store high or low values
- ? How can we read from them?
    - *When* do we know when the output is ready to be sampled?
    - If output is high, how can we tell the difference between a single high value and two high values in a row?
    - ~ Need some sort of timing signal
        - Let the circuit know when the output may be sampled
        - → **Clock signals**

> [!idea]+ Holding down a key on keyboard
>
> - How does the computer know when to register that you are holding down the ‘X’ button on keyboard and want to input multiple X’s?
> - When you do this, there is a delay:
>     - Press ‘X’ → Computer registers ‘X’ → As you hold, computer asks: Are you sure? → Outputs multiple X’s onto screen

## Clock Signals

- Computer divides time into certain intervals
    - Computers have the smallest atomic unit of time and that is it

> [!def]+ Clock signal
>
> - Regular pulse signal
>     - High value indicates when to update the output of the latch

- Alternates between 0 and 1
    - e.g., In an 8 GHz processor: Oscillates between 0 and 1 8 billion times a second
- ? What does it mean when the clock goes up?
    - Next unit of time has started

![](https://i.imgur.com/JFrnIOb.png)

### Signal Restrictions

- ? What is the limit to how fast the latch circuit can be sampled?
    - Determined by:
        - *Latency* time of transitions
            - Setup and hold time
        - Setup time for clock signal
            - *Jitter*
            - Gibbs phenomenon
        - ![](https://i.imgur.com/9rro8Nj.png)

- & Clock signals are measured as a unit of **frequency**
    - How many pulses occur per second
    - Measured in Hertz (Hz)

### Clocked SR Latch

![](https://i.imgur.com/kUmlr7P.png)

- Adding another layer of NAND gates to the $\overline{S}\,\overline{R}$ latch gives us a **clocked SR latch**, or
    - Gated SR latch
    - & Tells us when we are allowed to change things, and when we are not
- Input $C$ is often connected to a *pulse signal*
    - Alternates regularly between 0 and 1
    - (Clock)
- Helps ==synchronize== that all circuits have the same clock going into it
    - All of them are being told: “Now you can change”
- If $C = 1$:
    - The inputs $S$ and $R$ can reset the outputs of the circuit
- Otherwise, $C = 0$:
    - $S, R$ have no effect
    - $Q, \overline{Q}$ stay the way it was before

#### Clocked SR Latch Behaviour

- Same behaviour as SR latch, but with *timing*

Start off with $S = 0, R = 1$.

![](https://i.imgur.com/3opbFGT.png)

- If clock is high:
    - First NAND gates *invert* those values
    - → Get inverted again in the output

Change $R \to 0$.

![](https://i.imgur.com/0LoVwt5.png)

- If clock is high:
    - Bottom first NAND gate has input of 1
    - That is when things stop
        - & As long as the inputs are 1 and 1, the outputs keep their values from before

Now, set the clock low.

![](https://i.imgur.com/nQn1NHH.png)

- $ Even if the inputs change, the low clock input ==prevents== change from reaching the second stage of NAND gates
    - As long as the two first NAND gates have an input that is 0, ==output is  1==

While the clock is zero, change $S \to 1$.

![](https://i.imgur.com/cmtVmKc.png)

- Since clock is still zero, the output of the first NAND gates are both $1$
- Nothing changes

#### Summary

![](https://i.imgur.com/JmrrVvf.png)

- Typical symbol for a clocked SR latch
    - % Small NOT circle after the $\overline{Q}$ output:
        - Simply notation to use to denote the inverted output value
        - Is *not* an extra NOT gate
- Assume clock is 1:
    - Still have a problem when $S = R = 1$
    - State of $Q$ is ==indeterminate==
    - Set of inputs is considered a **forbidden state**
    - @ Sometimes: Do not want to allow both set and reset on
        - → **D latch**

### D Latch

Also known as gated D-latch.

![](https://i.imgur.com/CFrUk85.png)

- Make inputs to $R, S$ dependent on single signal $D$
    - → Avoid indeterminate state problem
- Value of $D$ now sets output $Q$ low or high whenever $C$ is high

![](https://i.imgur.com/NN6mKOh.png)

- Design is good, but still has problems
    - & ==Timing== issues
    - i.e., **[[Latches vs Flip-Flops#^a37b06|Transparency]]**
