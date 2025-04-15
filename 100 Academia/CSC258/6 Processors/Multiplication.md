---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 7
Module:
  - 6 - Processors
Lecture:
  - "20"
Chapter: 
Slides/Notes:
  - "[[processor_slides.pdf]]"
Date: 2025-02-26
Date created: Sat., Mar. 1, 2025, 4:53:25 pm
Date modified: Sun., Apr. 13, 2025, 3:11:08 pm
---

# Multiplication

- Multiplication (and division) operations are always more complicated than other arithmetic or logical operations
    - e.g., $+/-$ or AND, OR

> [!summary]+ Three major ways that multiplication can be implemented in circuitry:
> 1. **Layered rows of adder units**
> 2. **An adder/shifter circuit**
> 3. **Booth’s Algorithm**

## Binary Multiplication

### Recall How Multiplication Works

![](https://i.imgur.com/9AZGUbe.png)

In binary:

![](https://i.imgur.com/E6bOfP1.png)

- Do not have to do it with the top digit multiplied by the whole bottom number
- Can take the bottom digit and multiply with whole top number

![](https://i.imgur.com/nThbye7.png)

> [!obs]+ When you multiply two binary numbers together and take the ==AND== operation of both, it does the same thing
> - Why we use $A \cdot B$ notation for “$A$ and $B$”

- Essentially, when you do $101 \times 1$, it is the same as a bitwise AND

### Binary Multiplication is a Bunch of AND Operations

> [!important] All multiplication is is a bunch of AND operations

If we wanted to write this method of multiplication in a general notation, it would look like this:

![](https://i.imgur.com/Dl93NcA.png)

![](https://i.imgur.com/ioZHL7A.png)

- The black columns do not mean anything
    - Just there to make it easier to see which term corresponds with what

- & Implementing this in circuitry involves the summation of several AND terms
    - AND gates combine the input signals
    - Adders combine the outputs of the AND gates

> [!info]+ This implementation results in an array of adder circuits to make the multiplier circuit
> - This can get a little expensive as the size of the operands grows
> - $n$-bit numbers → $\mathcal{O}(1)$ time, but **==$\mathcal{O}(n^{2})$ size==**
>     - LSB has to multiply with all $n$ bits of the other number
>     - Next bit has to multiply with all $n$ bits of the other number
>     - And so on until the $n$-th bit multiples with all $n$ bits of the other number
>     - $n^{2}$ AND gates

- Speed-wise:
    - Performs really well
    - Ripples do carry a *delay* when one adder sends it to the next and so on
    - But whole thing will do its calculation (with the delays) in one clock cycle
    - Still consider to be $\mathcal{O}(1)$ time

> [!question] Is there an alternative to this circuit?

![|300](https://i.imgur.com/ioZHL7A.png)

- Notice:
    - First row of AND gates is being added with the second row of AND gates
    - Those full adder outputs provide an input to second row of full adders
    - Full adder result provides inputs to the final third row of full adders
- If it is doing an add, and another add, and another add, can we just have one row of adders?
    - Have results feed back into itself

## Accumulator Circuits

> [!question] What if you could perform each stage of the multiplication operation, one after the other?
> - This circuit would only need a single row of adders and a couple of shift registers
> - ? How wide does register $R$ have to be?
> - ? Is there a simpler way to do this?

![|300](https://i.imgur.com/Dl93NcA.png)

- Calculate each row individually, one clock cycle per row
    - First clock cycle: Calculate first row
    - Second clock cycle: Calculate second row
    - Third clock cycle: Calculate the third row
    - Fourth clock cycle: Calculate the last fourth row
- Each time you do, add it to the register that is going to maintain an accumulating sum
- Make sure that every time you do, it is going to align things in the proper column

![](https://i.imgur.com/JRebX2e.png)

- Idea:
    - Take register B and shift it to the left by one
    - Leftmost bit e.g., $b_{3}$ pops out of the carry $C_{out}$
    - Do bitwise AND of that single bit (e.g., $b_{3}$) and all of $a$ from Register A
    - The very first time: Register is 0
    - Add the result from bitwise $1 \times n$ AND and add it to the register
    - Take that result and shift it to the left
    - Take the next bit from Register B e.g., $b_{2}$
    - Take the bitwise AND with all of $a$
    - Add the bitwise result with the register $R$ value shifted by one and add to register $R$
    - And so on

### Accumulator, Illustrated

![|350](https://i.imgur.com/G4Z3E3B.png)

- First clock cycle:
    - $b_{3}$ gets shifted out
    - Bitwise AND with all bits of $a$
    - Put it into our register for long term storage
    - Shift value at bottom over by one bit

![|350](https://i.imgur.com/ypPmhEd.png)

- Second clock cycle:
    - Take out $b_{2}$
    - Bitwise AND with all bits of $a$
    - Add it to the register
    - Shift everything to the left
        - Both rows shift to the left
        - So far, the bottom row has been shifted left twice
        - Top row has been shifted once

![|350](https://i.imgur.com/Jots8zh.png)

- Third clock cycle:
    - Take next digit $b_{1}$
    - AND with all digits from $a$
    - Add it to the register
    - Shift everything to the left

![|350](https://i.imgur.com/qiqewvX.png)

- Fourth clock cycle:
    - Take last digit $b_{0}$
    - AND with all digits from $a$
    - Add it to the register
    - Do not shift everything to the left!

> [!info]+ If all you are concerned about is space, then this makes things more compact!
> - Instead of having an $n \times n$ circuit of $n$ gates in each row and $n$ rows:
>     - This will be in **==$\mathcal{O}(n)$ space==** and **==$\mathcal{O}(n)$ time==**
> - Instead of having $n$ gates and $n$ rows:
>     - We take away the rows
>     - Do each row on its own
>     - So all we need is a single row of adders
>     - And doing each row of calculation in a single clock cycle

> [!info]+ This is called an **accumulator** because you are doing one operation one row at a time.
> - Overall product is something you *accumulate* one clock cycle after another
> - Tradeoff:
>     - Can have something $\mathcal{O}(1)$ time, but $\mathcal{O}(n^{2})$ space
>     - Or have this accumulator with $\mathcal{O}(n)$ time and $\mathcal{O}(n)$ space

> [!question]+ Is there a more efficient way to do this?

## Booth’s Algorithm

> [!abstract]+ Devised as a way to take advantage of circuits where *shifting is cheaper* than adding, or where *space* is at a *premium*
> - Based on the premise that when multiplying by certain values (e.g., 99):
>     - Can be easier to think of this operation as a difference between two products
>         - e.g., $100x - 1x = 99x$

- ? How do we figure out how this stuff works?
    - $1234567 \times 9999$ In decimal:
        - When we have some number like $9999$, we would break it down into:
            - $1234567 \times 10000 = 12345670000$
                - $1234567$ shifted by 4
            - $1234567 \times 1 = 1234567$
                - $1234567$ shifted by 0

Consider the shortcut method when multiplying a given decimal value $X$ by $9999$:
$$
X \times 9999 = (X \times 10\,000) - (X \times 1)
$$
Consider the equivalent problem in binary:
$$
X \times 00\,1111 = (X \times 01\,0000) - (X \times 1)
$$

- Can break down $1111$ into $1\,0000$ - $1$
- Every time you find a number like $00\,1111$
    - *Add* a component of $X$ aligned with the fifth column
    - *Subtract* a component of $X$ aligned with the last column

### Idea of Booth’s Algorithm

- Whenever you see a number go from $01$, add a component of $X$ to the left side of $01$
    - e.g., $0\bbox[ grey ]{ 0\,1 }111$
- Whenever you see a number go from $10$, subtract a component of $X$ from the $1$ of this $10$
    - e.g., $00\,111\bbox[ grey ]{ 1 }$
        - There is an implied zero at the end of the number

> [!summary]+ The idea is triggered on cases where ==two neighbouring digits in an operand== are ==different==.
> - If digits at $i$ and $i - 1$ are $0$ and $1$:
>     - Multiplicand is *added* to the result at position $i$
> - If digits at $i$ and $i - 1$ are $1$ and $0$:
>     - Multiplicand is *subtracted* from the result at position $i$
> - Result is always a value whose size is the sum of the sizes of the two multiplicands

![](https://i.imgur.com/2EzvQtR.png)

- Go through $A$ from left to right until we find a place where two digits side-by-side are $01$

![](https://i.imgur.com/VuFgux4.png)

- Need to add a component of $B$ to the column that aligns with the $0$ in the highlighted digits

![|986x313](https://i.imgur.com/jw8gBrZ.png)

- Then, keep reading $A$ to find a place where digits are $10$

![](https://i.imgur.com/sCnmoAJ.png)

- Then, *subtract* a component of $B$ from the $1$ column
    - → Two’s complement of $B$, then add it to that column
    - Need to *sign extend* the two’s complement to make sure the top component of $B$ and bottom component of $B$ are aligned
    - Top row will be extended with zeroes

![](https://i.imgur.com/wfq7LxJ.png)

- Add these two numbers together

![](https://i.imgur.com/R58gqbu.png)

> [!important]+ This is the idea behind Booth’s Algorithm.
> - Take advantage of numerical structure
> - Narrow down the number of operations you need to do

> [!question]+ How do we know if this is going to create any kind of savings? Great for this particular number, but what if $A = 0101010$?
> - Just as bad as previous accumulator circuit
> - Would take $\mathcal{O}(n)$ time because you need to do addition/subtraction $n$ times
>     - One for every transition
> - But that is not how numbers work
> - If you think about the most common number that shows up on your computer, it is 0
>     - Number used to clear everything to
>     - 0 shows up the most
> - Next most common number would be $1/-1$
> - Not going to get random strings of $0,1$‘s commonly occurring throughout computer
> - & Distribution is centred around $0, 1, -1$
>     - When it doesn’t, it usually is centred around long strings of $1$
>     - e.g., $15, 255$

### Implementing Booth’s Algorithm in Hardware

- We need to make this work in hardware:
    - Option 1:
        - Have hardware set up to compare neighbouring bits at every position in $A$
        - With adders in place for when bits do not match
        - Problem:
            - ! This is a lot of hardware
            - Which Booth’s Algorithm is trying to avoid
    - Option 2:
        - Have hardware set up to compare two neighbouring bits
        - Have them move down through $A$, looking for mismatched pairs
        - Problem:
            - ! Hardware does not move like that
            - Cannot scan
    - & Option 3:
        - We cannot move our $i$‘s across, but we can fix our $i$’s and move the number
            - Can *shift* number
        - Look at two digits in the number, and shift number to look at all neighbouring pairs of digits to find transitions from $0 \to 1, 1 \to 0$
        - & Have hardware set up to compare two neighbouring bits in the lowest position of $A$
            - Looking for mismatched pairs in $A$ by shifting $A$ to the right one bit at a time
        - This could work!
            - & But accumulated solution $P$ would have to shift one bit at a time as well
                - So when $B$ is added or subtracted, it is from the correct position

### Steps in Booth’s Algorithm

> [!summary]+ Steps in Booth’s Algorithm
> 1. Designate the two multiplicands as $A$ and $B$, and the result as some product $P$
> 2. Add an extra zero bit to the right-most side of $A$
>     - Last digit has an implied $0$ coming after it
>     - If comparing neighbouring digits, when we get to the last digit, we need to compare it to its neighbour $0$
> 3. Repeat the following for each original bit in $A$:
>     1. If the last two bits of $A$ are the same, do nothing
>     2. If the last two bits of $A$ are $01$, then add $B$ to the highest bits of $P$
>     3. If the last two bits of $A$ are $10$, then subtract $B$ from the highest bits of $P$
>     4. Perform one-digit arithmetic right-shift on both $P$ and $A$
> 4. Result in $P$ is the product $A \times B$

### Booth’s Algorithm Example

> [!example] $(-5) \times 2$

- Steps 1, 2:
    - $A = -5 \to 11011$
    - Add an extra zero to the right
    - → $A = 11011\,0$
    - $B = 2 \to 00010$
    - $-B = -2 \to 11110$
    - $P = 0 \to 00000\,00000$
    - This is the setup
- Step 3 (repeat 5 times):
    - Check last two digits of $A$
        - $1101\,\bbox[ grey ]{ 10 }$
    - Since digits are $10$, *subtract* $B$ from the most significant digits of $P$:
        - ![|250](https://i.imgur.com/CwiKGPU.png)
    - Arithmetic shift $P$ and $A$ one it to the right
        - $A = 111011$
        - $P = 11111\,00000$
- Step 3 (repeat 4 more times):
    - ![](https://i.imgur.com/uSOePS3.png)
- Step 3 (repeat 3 more times):
    - ![|749x445](https://i.imgur.com/j3qD5XT.png)
- Step 3 (repeat 2 more times):
    - ![](https://i.imgur.com/ZcY8Wok.png)
- Step 3 (repeat 1 more time; final time):
    - ![](https://i.imgur.com/FSL5Sni.png)

> [!note]+ Final product
> $$
> \begin{align}
> P & = 11110110 \\
>  & = -10
> \end{align}
> $$

- Number of times you loop through step 3 is equal to the number of digits in original $A$

> [!info]+ Time and space
> - $\mathcal{O}(n)$ time
> - $\mathcal{O}(n)$ space
>     - Adder and shifter with $n$ bits in length
> - Only savings compared to accumulator is that the ==number of operations== you do
>     - i.e., how many times you are going to add
>     - Just changing the constant of number of add operations in a single time

## Reflections on Multiplication

- A popular version of this algorithm involves:
    - Copying $A$ into the lower bits of $P$
    - So, testing and shifting only takes place in $P$
    - Also good for maintaining original value of $A$
- Multiplication is not as common of an operation as addition or subtraction
    - But occurs enough that its implementation is handled in the hardware
    - Rather than by CPU
- Most common multiplication and division operations are powers of 2
    - & For this, a **shift register** is used instead of the multiplier circuit

## Barrel Shifter Unit

> [!note]+ Shifter time
> - In the past:
>     - Said shifters take $\mathcal{O}(n)$ time
>     - If you shift by $n$ bits, it takes $n$ clock cycles to shift it $n$ bits
> - Another circuit will shift in *one* clock cycle:
>     - **Barrel Shifter unit**

![](https://i.imgur.com/Dyr59tb.png)

- **Barrel shifter**
    - *Shifts* and *rotates* $D$ to the left by $S$ bits
        - $S_{1}S_{0} = 01 \implies Y = D_{2} D_{1} D_{0} D_{3}$
        - $S_{1}S_{0} = 11 \implies Y = D_{0}D_{3}D_{2}D_{1}$

> [!note]+ This is a **purely combinational circuit**, unlike the shift registers in the lab.
> - Tradeoffs:
>     - Save a lot in time
>     - More expensive in terms of space

## Expanding Our View to ALUs

- & There is no one way of making an ALU
    - No one way of making multiplier
    - No one way of implementing shifter
    - All depends on circumstances and needs to meet

![](https://i.imgur.com/CBZp3vr.png)

> [!question]+ Where are these inputs coming from? Where are the outputs going to?
> - [[The Storage Unit]]

^3c093e
