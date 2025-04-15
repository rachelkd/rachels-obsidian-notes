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
Chapter: 
Slides/Notes:
  - "[[flipflops_slides.pdf]]"
Date: 2025-01-31
Date created: Sat., Feb. 1, 2025, 5:23:39 pm
Date modified: Mon., Feb. 24, 2025, 5:13:07 pm
---

# Latches vs. Flip-Flops

## Motivation

> [!idea]+ Calculator Behaviour
> - If you enter $1 + 1$ on a calculator, what happens each time you press the “=” button?
>     - Expectations: ![|center|400](https://i.imgur.com/usG1k9O.png)
>     - ! Latches cannot do this

- When can outputs change in a latch?
    - & When clock signal is high
    - → No way to predict the output value when “=” is pressed

Thus, what you would see when you press the “=” button would be:

![](https://i.imgur.com/Vyisf7W.png)

## Transparency

> [!def]+ Transparent
> - Any changes to its inputs are visible to the output when control signal is 1
>     - i.e., Clock is 1

^a37b06

> [!important] The output of a latch *should not* be applied directly or through combinational logic to the input of the same or another latch when they all have the same control signal.

### Examples

Consider the circuit below.

![](https://i.imgur.com/f7f6HLe.png)

Assume $Q = 0$ at first.

![|408](https://i.imgur.com/s2B1j3D.png)

- Expected:
    - Output flips when clock goes high
- Actual:
    - When clock signal is high:
        - & Output toggles back and forth from $0$ and $1$
        - Until clock goes low away
    - ![](https://i.imgur.com/GvPmdVv.png)
    - → Have no idea what output will be when clock is low again

---

![](https://i.imgur.com/BMkc2Q0.png)

- ? What happens when clock turns 1?
    - Intuitively: Should flip from $01010 \to 00101$
        - Each value should be passed into the next one
    - Actually:
        - ! All values get obliterated except value 0
        - First $D$ input → $\overline{Q}$ → $\overline{Q}$ → $\overline{Q}$ → $\overline{Q}$

## Value-Triggered vs. Edge-Triggered

> [!goal]+ Instead of having all latches flooded by 0 value, we would rather have the clock ==shifting== the latch values over by one.
> ![](https://i.imgur.com/9lucrsw.png)

- Also imagine this happening at the *moment* the clock changes
    - ! Not for duration of the high clock signal
- → Edge-triggered vs. value-triggered
    - **Edge-triggered**
        - Change at the ==moment== the clock is 1
    - **Value-triggered**
        - Change ==as long as== the clock is one

### Canal Model

Also known as airlock model.

![](https://www.rideau-info.com/canal/lock_ani.gif)

- Picture how boats travel through a canal in *stages* — one stage at a time:
    - Boat enters middle section (i.e., the lock)
    - Entry gate is closed
    - Water level is raised
    - Exist gate is opened
    - Boat leaves canal
- Similar idea to airlocks on a space station
    - ![](https://i.imgur.com/GAYRKEk.png)

> [!question]+ How does this work with latches?
> - Want output to change only once when clock changes
>     - & At the moment it changes
> - Solution:
>     - Create a middle section that is set to the input value when clock is high
>     - Sends that value to output when clock is low
>     - → Prevents unwanted feedback and changes to output
>
> ![](https://i.imgur.com/dwWpcrV.png)

## Flip-Flops

> [!def]+ Flip-flop
> - A latched circuit whose output is triggered with the rising edge or falling edge of a clock pulse

### SR Master-Slave Flip-Flop

![](https://i.imgur.com/rVDnwwn.png)

- If clock is on:
    - Left latch is turned on i.e., open
        - Can change inputs $S, R$
    - Middle wires also change
    - Outputs $Q, \overline{Q}$ do not change because right latch is off/closed
- As soon as clock is off:
    - Left latch is turned off
    - Right latch is turned on
    - Outputs $Q, \overline{Q}$ change depending on the right latch inputs from the middle wires before the left latch was closed
    - Can change the inputs as much as you want without changing the outputs $Q, \overline{Q}$
    - → No transparency!

![](https://i.imgur.com/HMFHS4z.png)

- Outputs change when clock goes from 1 → 0

### Edge-Triggered D Flip-Flop

- SR Flip-flops still have issues of ==unstable== behaviour
    - From indeterminate states
        - (See [[Sequential Circuits#Summary]])
- Solution:
    - & **D flip-flops**
        - Connect D latch to the input of a SR latch
        - **Negative-edge triggered** flip-flop
            - Triggered when clock goes from 1 → 0
            - Like the SR master-slave flip-flop

#### Flip-Flop Behaviour

![](https://i.imgur.com/ptIEjs0.png)

- If clock signal is high:
    - Input to first flip-flop is being sent out to the second
    - Second flip-flop does not do anything until clock signal goes down again
    - If it is the very beginning and you are booting your circuit up to start:
        - Not actually have an output of any kind
        - Output $Q = \overline{Q} = Z$
            - If you do not connect your output to high voltage or low voltage
            - Neither 1 nor 0

![](https://i.imgur.com/UO8jvV9.png)

- When clock goes from high to low:
    - First flip-flop stops transmitting a signal
    - Second flip-flop starts

![](https://i.imgur.com/VN9E6Hl.png)

- If the input to $D$ changes:
    - Change is not transmitted to the second flip-flop
        - (Until clock goes high again)

![](https://i.imgur.com/GkSUjOS.png)

- Once clock goes high again:
    - First flip-flop starts transmitted at the same time
    - Second flip-flop stops
        - At the same time

#### Positive-Edged Triggered Flip-Flops

![|668](https://i.imgur.com/0kMGMga.png)

- More common used
    - Choice for this course

## Other Flip-Flops

### T Flip-Flop

![|372](https://i.imgur.com/ZSlW70A.png)

- Similar to D flip-flop, except
- Clock makes it *toggle* whenever $T$ input is high
    - Flip between 0 and 1

![|500](https://i.imgur.com/SjGnUkq.png)

### JK Flip-Flop

Recall in SR latches

- When $S, R$ are both high, there is an inconsistent output
    - Forbidden state
- ? Why don’t we make something that uses the forbidden state?
    - Make that input combination do something useful
    - → **JK Flip-Flop**

![](https://i.imgur.com/PrEArpH.png)

- **JK Flip-Flop**
    - Takes advantage of all combinations of two inputs $J, K$ to produce four different behaviours
        - $J = K = 0 \iff$ *Maintain* output
        - $J = 0, K = 1 \iff$ *Reset* output to 0
        - $J = 1, K = 0 \iff$ *Set* output to 1
        - $J = K = 1 \iff$ *Toggle* output value

![](https://i.imgur.com/L1t9rpI.png)

- Combines the SR flip-flop with the T flip-flop
    - Does the T flip-flop behaviour in the one (previously forbidden) state
    - → Does something useful for all four possible input combinations

## Sequential Circuit Design

- **Sequential circuit design**
    - Similar to creating combinational circuits, with ==extra considerations==
- Flip-flops now provide extra inputs to the circuit
- Extra circuitry needs to be designed for the flip-flop inputs

![](https://i.imgur.com/iVQd9Hf.png)
