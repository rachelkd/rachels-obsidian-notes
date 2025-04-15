---
dg-publish: true
tags: [lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 2
Module:
  - "[[1 - Transistors]]"
  - "[[2 - Combinational Circuit Design]]"
  - "[[3 - Logic Devices]]"
  - 4 - Sequential Circuits
Lecture:
  - "10"
  - "13"
  - "4"
  - "7"
Chapter: 
Slides/Notes: 
Date: 2025-01-13
Date created: Mon., Jan. 13, 2025, 1:17:36 pm
Date modified: Sun., Feb. 23, 2025, 3:47:47 pm
---

# Midterm Short Answer Questions

## Transistors

From Week 2

> [!question]- True or False? If you increase the resistance of a circuit, the current increases.
>
> - False

> [!question]- Which transistor connects the source and drain when the gate input is high?
>
> - nMOS

> [!question]- Materials are either conductors, insulators, or…
>
> - Semiconductor

> [!question]+ What gate is created by the following?
> ![|195x264](https://i.imgur.com/P0SWmBZ.png)
>
> > [!check]- Solution
> >
> > - NAND Gate
> >     - AND gate with a NOT gate at the end

> [!question]+ Which gate is this one?
> ![|266x320](https://i.imgur.com/Dw2r0N6.png)
>
> > [!success]- Solution
> >
> > | A     | B     | W   | Y   |
> > | ----- | ----- | --- | --- |
> > | **0** | **0** | 1   | 0   |
> > | **0** | **1** | 0   | 1   |
> > | **1** | **0** | 0   | 1   |
> > | **1** | **1** | 0   | 1   |
> >
> > $W = \overline{A} \cdot \overline{B} = \overline{(A + B)}$
> >
> > - $ The circuit on the right that has input $W$ and output $Y$ is a NOT gate
> >
> > $Y = A + B$

## Combinational Circuit Design

From Week 3

> [!question]+ How can you express a two-input XOR gate as a combination of NAND and NOT gates?
>
> > [!check]- Solution
> >
> > | A   | B   | Y   |
> > | --- | --- | --- |
> > | 0   | 0   | 0   |
> > | 0   | 1   | 1   |
> > | 1   | 0   | 1   |
> > | 1   | 1   | 0   |
> >
> > ![|367x174](https://i.imgur.com/CX99jiL.png)
> >
> > - Two NOT gates, two AND ands connecting $\overline{A} B$ and $A \overline{B}$, then take two terms in OR gate to output $Y$
> >
> > - Recall:
> >     - De Morgan’s: $(\overline{W} + \overline{Z}) = (\overline{WZ})$
> >
> > $$
> > \begin{align}
> > Y & = \overline{A}B + A \overline{B} \\
> > & = \overline{\overline{\overline{A}B}} + \overline{\overline{A\overline{B}}} \\
> > & = \overline{\left( \overline{\overline{A}B} \right) \cdot \left( \overline{A\overline{B}} \right) } && (\text{De Morgan's})
> > \end{align}
> > $$
> >
> > ![|307x147](https://i.imgur.com/Zzapc05.png)

> [!question]+ How can you implement a NOT gate from a 2-input NAND gate?
> ![|337x149](https://i.imgur.com/pWNN17Z.png)
>
> > [!success]- Solution
> > ![|340](https://i.imgur.com/ICS0Lwo.png) ![|340](https://i.imgur.com/fT3VRTr.png)

> [!question]+ Write $Y$ is SOM form.
> ![|216x230](https://i.imgur.com/4AJKKz7.png)
>
> > [!success]- Solution
> >
> > - Have to have four minterms
> >     - One for each output where $Y = 1$
> > - Minterm is an AND expression of all the inputs
> >
> > $$
> > Y = \overline{A} \cdot \overline{B} \cdot C + \overline{A} \cdot B \cdot \overline{C} + A \cdot \overline{B} \cdot \overline{C} + A\cdot B\cdot C
> > $$
> > $$Y = m_{1} + m_{2} + m_{4} + m_{7}$$

> [!question]+ Given the minterms below, can you fill in the truth table?
> ![|388x130](https://i.imgur.com/5lEkbyu.png)
>
> > [!success]- Solution
> >
> > - $Y$ goes high in six cases
> > - Just go down the table and match the minterms to the expression
> >
> > ![|239x353](https://i.imgur.com/JOkBVmY.png)

> [!question]+ What is the most reduced boolean expression, in SOP form, of the function from the truth table?
> ![|232x347](https://i.imgur.com/17OHKT9.png)
>
> > [!success]- Solution
> >
> > - Start off with SOM approach
> >     - $$Y = m_{0} + m_{1} + m_{2} + m_{5} + m_{7} + m_{8} + m_{9} + m_{10} + m_{13} + m_{15}$$
> > - Create Karnaugh map
> >
> > ![|327x201](https://i.imgur.com/UQ9le7Q.png)
> > ![|330](https://i.imgur.com/SvQok0g.png)
> >
> > $Y = \underbrace{ \overline{C} D }_{ \text{Vertical column} } + \underbrace{ BD }_{ \text{Middle box} } + \underbrace{ \overline{B} \, \overline{D} }_{ \text{Corners} }$
> >
> > - An alternative grouping:
> >     - ![|300](https://i.imgur.com/Casqnus.png)
> >     - $Y = \overline{B} \cdot \overline{C} + B \cdot D + \overline{B} \cdot \overline{D}$

## Logical Devices

From Week 4.

### Question 1

> [!question]+ How do you write the unsigned number 78 as an 8-bit binary number?
>
> - Divide 78 by 2 repeatedly
> - Remainder will fill in digits from right to left
> - $0100\,1110$

> [!question]+ What is the two’s complement of $0110\,1101$?
>
> - One’s complement + 1:
>     - $1001\,0010 + 1 = 10010011$

> [!question]+ What is the sum of $0110\,1101$ and $0110\,1101$?
>
> - Multiply by 2, shift it over to the left?
> - $1101\,1010$

### Question 2

> [!question]+ What groupings are in the K-map? What logic equations do these groupings represent?
> ![|500](https://i.imgur.com/vN2RCHa.png)
>
> > [!success]- Solution
> > ![|500](https://i.imgur.com/NgqZORO.png)
> >
> > $$\overline{A} \cdot \overline{B} + \overline{C}$$

### Question 3

> [!question]+ How would you implement the $A > B$ output of the 2-bit comparator above out of 1-bit comparators and a minimal number of gates?
> ![|150](https://i.imgur.com/nuoVtlQ.png) ![|150](https://i.imgur.com/NdRHc4B.png)
>
> > [!success]- Solution
> >
> > - Consider the implementation of the $A == B$ signal
> >     - ![|300](https://i.imgur.com/FCPsUts.png)
> > - $A > B$ signal follows the same idea
> >     - As soon as you see that the first digit of $A$ is greater than first digit of $B$, then you know $A > B$
> >     - Second case:
> >         - First digits are the same
> >         - Second digit of $A$ is greater than the second digit of $B$
> >     - ![|500](https://i.imgur.com/koi9pny.png)

## Flip-flops

From Week 5.

### Question 1

> [!question]+ What are the output values from $Q$ and $\overline{Q}$ given the following inputs on $S, R, C$?
> ![|400](https://i.imgur.com/6e61X2x.png)
>
> ![|401x210](https://i.imgur.com/AilHChB.png)
>
> > [!check]- Solution
> >
> > - & You have to know what $S, R, C$ do
> >     - $S$ sets the output $Q$ high
> >     - $R$ sets output $Q$ low
> >     - $C$ is the signal that indicates “you are only allowed to change the signal when $C$ is high”
> >
> > Consider the first row:
> >
> > ![|400](https://i.imgur.com/AwQ4ZcT.png)
> >
> > - Clock is saying we can change the output
> > - $S, R$ are both turned off
> >     - Not going to change the outputs; maintain values from before
> >     - ![|400](https://i.imgur.com/AIqAxos.png)
> >     - ! We do not really know the value of $Q$
> >         - Since the right part ($\overline{S} \; \overline{R}$ latch) maintains previous values when $\overline{S} = 1, \overline{R} = 1$
> >         - Cannot known what previous values are at the beginning
> >     - ![|301x206](https://i.imgur.com/Wpt4wSg.png)
> >
> > Consider the second row of inputs:
> >
> > ![|400](https://i.imgur.com/GRwihUL.png)
> >
> > - Clock is high → Can change output
> > - Set is high, reset is low → Going to make $Q$ high
> >     - Can trace if unsure
> >     - Top-middle wire is $0 \because 1_{[=S]} \text{ NAND } 1_{[=R]} = 0$
> >     - Then, since at least *one* input to the top-right NAND gate is 0, then $Q = 1$
> >     - $Q = 1$ goes into the bottom-right NAND gate $\implies \overline{Q} = 0$
> > - Intuition:
> >     - Clock is allowing us to change things
> >     - Set is high
> >     - Reset is low
> >     - Should set output to high
> >
> > ![|257x176](https://i.imgur.com/cdh69Or.png)
> >
> >
> > Consider what happens if we turn the clock off ($C = 0$):
> >
> > - Clock $C = 0$ makes both outputs of the left-side NAND gates to be 1
> > - If both middle wires are 1, then the output is not going to change!
> >     - $1$ going into the NAND gate keeps the output the way it was before
> > - Maintains $Q = 1, \overline{Q} = 0$
> >     - From previous values
> >
> > Consider what happens if we turn $S$ off:
> >
> > ![|400](https://i.imgur.com/979Uojc.png)
> >
> > - Nothing happens
> >     - $\because C = 0$
> >
> > ![|400](https://i.imgur.com/YDxf7LQ.png)
> >
> > Consider what happens if we turn reset on:
> > ![|400](https://i.imgur.com/4UAHNSQ.png)
> >
> > - Nothing happens $\because C = 0$
> >
> > ![|400](https://i.imgur.com/TFmEU3K.png)
> >
> > Consider what happens if we set $C = 1$ again:
> > ![|400](https://i.imgur.com/BsbeqL8.png)
> >
> > - Top-middle wire is still going to have output 1
> > - Bottom-middle wire is going to be 0
> > - As soon as that happens, $\overline{Q}$ changes from 0 → 1
> > - That changes the output of $Q$ to 0
> >
> > ![|400](https://i.imgur.com/xqoIjfA.png)
> > ![|400](https://i.imgur.com/4xMYbgV.png)

> [!note]+ We have undefined outputs in the first row. What happens when your computer restarts?
>
> - Goes through everything in memory and resets it to 0

### Question 2

> [!question]+ Given the circuit below and the input waveform, what will the outputs be on $Q_{L}$ and $Q_{F}$? What other info do you need?
>
> > [!check]- Solution
> > ![|300](https://i.imgur.com/u00oOol.png)
> >
> > ![|400](https://i.imgur.com/nf53os4.png)
> >
> > - Starts off with $C, D$ both low, then $D$ goes high, then $C$ goes high afterwards
> >     - % Have to understand that $D$ is going to provide the values for these two circuits, and $C$ is the synchronization/clock
> >     - One of the things that we assume is that these circuits are triggered off a positive-edge
> >         - Default assumption if not told explicitly otherwise
> >     - % Another thing to ask is: ==What are the starting values?==
> >         - $Q_{L}, Q_{F}$ both start at 0
> > - These two circuits must obviously be different
> >     - Clue:
> >         - $ Diagram of circuit on top has a different clock than the one on the bottom
> >         - Bottom diagram has an arrow going into the clock
> >             - Diagram symbol for a flip-flop
> >             - Latch does not have that arrow; flip-flop does
> >         - & Top circuit is a latch; bottom circuit is a flip-flop
> >         - Main difference:
> >             - & Latch output can change ==as long as== clock is high
> >                 - i.e., For the duration of the clock being high, it can change any time in that range
> >             - & Flip-flop output only changes when the clock goes high i.e., ==on the positive edge==
> >
> > ![|500x228](https://i.imgur.com/RDnFZyy.png)

### Question 3

> [!question]+ Assuming the $Q$ outputs of both flip-flops start off low, what will the value of $X, Y$ be over the next few clock cycles?
>
> - Assume positive-edge trigger
>
> ![|300](https://i.imgur.com/OeRsHX4.png)
>
> ![|314x96](https://i.imgur.com/ryn92LH.png)
>
> > [!check]- Solution
> >
> > - & Figure out what the circuits are. Latches or flip-flops?
> >     - Both are flip-flops $\because$ diagrams have an arrow on clock
> >     - Trigger off the positive edge
> >         - @ Every time there is a positive edge, need to figure out what the values of $X, Y$ will be
> > - ? Both start off low. When first edge comes, what is the output going to be?
> >     - ? Where do each of these things get their input from?
> >         - Both get input from respective $\overline{Q}$
> >         - If both outputs $Q = 0$, then $\overline{Q} = 1$
> >         - When clock hits, both should go from $0$ and $0$ → $1$ and $1$
> >         - Then, outputs of $Q$ are both 1 and $\overline{Q} = 0$
> >         - New $\overline{Q} = 0$ is going to be set up on their inputs
> >
> > ![|400](https://i.imgur.com/YCPf5pN.png)
