---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 3
Module:
  - "[[3 - Logic Devices]]"
Lecture:
  - "8"
Chapter: 
Slides/Notes:
  - "[[devices_slides.pdf]]"
Date: 2025-01-22
Date created: Wed., Jan. 22, 2025, 1:18:29 pm
Date modified: Wed., Feb. 5, 2025, 10:16:28 pm
---

# Adder Circuits

> [!goal] Create a circuit that can add two numbers together

> [!def]+ Adder
> - Also known as binary adder
> - Small circuit device that ==adds== two digits together
> - Combined together to create **iterative combinational circuit**

> [!summary]+ Types of Adders
> 1. Half adders (HA)
> 2. Full adders (FA)
> 3. Ripple Carry Adder

## Aside: Binary Math

> [!info]+ Review of Binary Math
> - Each digit of a decimal number represents a power of 10
>     - $258 = 2 \times 10^{2} + 5 \times 10^{1} + 8 \times 10^{0}$
> - Each digit of binary number represents a power of 2
>     - $01101_{2} = 0 \times 2^{4} + 1 \times 2^{3} + 1 \times 2^{2} + 0 \times 2^{1} + 1 \times 2^{0} = 13_{10}$

### Decimal to Binary Conversion

> [!question]+ How would you represent 11 in binary?
> - Keep dividing by 2
> - Write the remainders

| Number | Quotient = $\frac{\text{Number}}{2}$ | Remainder = Number % 2 |                             |
| ------ | ------------------------------------ | ---------------------- | --------------------------- |
| 11     | 5                                    | ==1==                  | Least significant bit (LSB) |
| 5      | 2                                    | ==1==                  |                             |
| 2      | 1                                    | ==0==                  |                             |
| 1      | 0                                    | ==1==                  | Most significant bit (MSB)  |

### Hexadecimal Numbers

- Base 16 numbers
    - 0 to 9 as in decimal, and
    - $10 \to A$
    - $11 \to B$
    - $12 \to C$
    - $13 \to D$
    - $14 \to E$
    - $15 \to F$
- $1530_{10} = 0000\,0101\,1111\,1010_{2} = 0\text{x}05\text{fa}_{16}$
    - Hex numbers are typically expressed as `0x_____`

### Unsigned Binary Addition

![|261](https://i.imgur.com/0Knrmdh.png)

- In decimal addition, we can add large numbers
- In computers:
    - Bits are finite/fixed in width
    - Numbers cannot grow
- 64-bit processor:
    - Integers are 64 bits long

![|300](https://i.imgur.com/22iENe6.png)

- Carry bit gets discarded
- Result is something unexpected

## Half Adders

> [!def]+ Half Adder
> - A two-input, one-bit width binary adder that performs the following computations:
>     - ![|300](https://i.imgur.com/nZ0kjcB.png)
> - Adds two bits to produce a two-bit sum
> - Sum is expressed as a sum bit $S$ and carry bit $C$
>
> ![|200](https://i.imgur.com/UmbA50G.png)

### Half Adder Implementation

- Equations and circuits for half adder units are easy to define
    - Even without K-maps

$$
\begin{align}
C = X \cdot Y  &  & S  & = X \cdot \overline{Y} + \overline{X} \cdot Y \\
 &  &   & = X \oplus Y && (\text{XOR})
\end{align}
$$
- Carry bit $C$ is high only when $X$ and $Y$ are high
- $S$ is high if only $X$ or only $Y$ is high


![|500](https://i.imgur.com/9OdyXsO.png)

## Full Adders

> [!question]+ What if we need to add multi-digit numbers?
> - Not just adding $X$ and $Y$
> - Any digit in the middle has to be able to handle carry
>     - A third input from the digit next door
> - Need to use a **full adder**

- Similar to half adders, but
- Has another input $Z$
    - Represents a *carry-in* bit

> [!note] $C$ and $Z$ are sometimes labeled as $C_{\text{out}}$ and $C_{\text{in}}$.

![|331](https://i.imgur.com/LmqNcP5.png)

- Usually adding two numbers that are a single bit in a multi-digit value
- Send a carry out if it *overflows*
- Also get the carry from the next full adder unit if that one overflowed into us

- When $Z = 0$:
    - Unit behaves exactly like a half adder
- When $Z = 1$:
    - ![|371](https://i.imgur.com/rwTQ0nw.png)

### Full Adder Implementation

![](https://i.imgur.com/wYmQIUp.png)

![](https://i.imgur.com/wRkSjp4.png)

![](https://i.imgur.com/xdGxsyf.png)

$$
\begin{align}
C = X \cdot Y + X \cdot Z + Y \cdot Z && S = X \oplus Y \oplus Z
\end{align}
$$
- $C$ term can also be rewritten as $C = \underbrace{ \bbox[ pink ]{ X \cdot Y } }_{ \text{Carry generate } G } + \underbrace{ \bbox[ lavender ]{ (X \oplus Y) } }_{ \text{Carry propogate } P } \cdot Z$
    - Two terms come from this
        - Carry generate $G$
        - Carry propagate $P$

## Ripple-Carry Binary Adder

- **Ripple-carry binary adder**
    - **Full adder** units chained together in order to ==perform operations== on signal **vectors**
- If we have an $n$ digit number, then we put together $n$ adders
    - Each would handle a single digit

![](https://i.imgur.com/jKI4waJ.png)

### The Role of $C_{\text{in}}$

> [!important]+ Why do we need a full adder for the least significant bit (LSB)?
> - i.e., The rightmost, smallest bit
> - & Enables ==subtraction==
>     - Subtraction is just adding a negative number
>         - $A - B = A + (-B)$
>         - To get $-B$, we invert its bits and add one
>         - The initial $C_{\text{in}} = 1$ provides the $+1$

Assume you have an 8-bit binary number.

> [!example]+ Representing $-1$ in binary
> - Add $1$ to $1111\,1111$
>     - ![|400](https://i.imgur.com/stdlaG1.png)
> - ? What number when added to $1$ gives $0$?
>     - $-1$
> - & $-1$ in binary is $1111\,1111$
>     - Invert $1_{10} = 0000\,0001$ to get $1111\,1110$
>     - Add one to get $1111\,1111$

## Subtractors

- **Subtractors**
    - An extension of adders
    - Perform addition on a negative number

> [!summary]+ Two types of numbers
> - **Unsigned**
>     - All numbers are positive
> - **Signed**
>     - All bits are used to store a *two’s complement* negative number
>     - Signed is more common; what we use in [[CSC258]]
>
> > [!example]- Assume we have an 8-bit binary number. What are the ranges of integers?
> > - **Unsigned range**: $0000\,0000$ (0) to $1111\,1111$ (255)
> > - **Signed range**: $1000\,0000$ (-128) to $0111\,1111$ (127)
> >     - If you do not declare something, it is assumed to be *signed*

### Two’s Complement

> [!question]+ How do you represent negative numbers?
> - **Two’s complement**
>     - Method for representing negative numbers in binary
>     - First step:
>         - Get **one’s complement**
>             - Given number $X$ with $n$ bits, take $(2^{n} - 1) - X$
>             - Negates each individual bit (bitwise NOT)
>         - i.e., Invert all the bits
>     - Second step:
>         - Get **two’s complement**
>             - $\text{One's complement }+ 1$
>             - i.e., Add one
>     - e.g.,
>         - $0100\,1101 \to 1011\,0011$
>         - $1111\,1111 \to 0000\,0001$

- Adding a two’s complement number to the original number produces a result of ==zero==

### Signed Subtraction

- Negative numbers are generally stored in two’s complement notation
- Sum of a number and its two’s complement is ==zero==
    - → Subtraction can be performed by using binary ==adder circuit== with negative numbers

#### Signed 3-Bit Binary Numbers

![](https://i.imgur.com/jrNCNjc.png)

- All of the negative numbers have `1` for the MSB

#### Rules about Signed Numbers

> [!summary]+ Rules about Signed Numbers
> - Largest positive binary number is a zero followed by all ones
> - Binary value for $-1$ has ones in all the digits
> - Most negative binary number is a one followed by all zeroes
> - & There are $2^{n}$ possible values that can be stored in an $n$-digit binary number
>     - $2^{n-1}$ are negative
>     - $2^{n-1}-1$ are positive
>     - One is $0$

- e.g., Given an 8-bit binary number:
    - 256 possible values
    - One is zero
    - 128 are negative values
        - $1111\,1111 \to 1000\,0000$
        - i.e., $-1 \to -128$
    - 127 positive values
        - $0000\,0000 \to 0111\,1111$
        - i.e., $1 \to 127$

> [!example]+ Assume 4-bit signed numbers. Write the follow decimal numbers in binary.
>
> > [!question]- 2
> > - $0010$
>
> > [!question]- -1
> > - $1111$
>
> > [!question]- 0
> > - $0000$
>
> > [!question]- 8
> > - Not possible to represent in 4 digits
>
> > [!question]- -8
> > - $1000$

#### At the Core of Subtraction

- Subtraction of a number is the addition of its negative value
    - Negative value is found using two’s complement process
- e.g.,
    - $7 - 3 = 7 + (-3)$
    - $-3-2=-3+(-2)$

![](https://i.imgur.com/izppEgZ.png) ![|314](https://i.imgur.com/kK2ah4R.png)

![](https://i.imgur.com/fdvlk9y.png)

### Subtraction Circuits

- & We can do a subtraction operation using an adder
    - Need to figure out a way to use two’s complement on the thing being subtracted
- 4-bit subtractor: $X - Y$
    - i.e., $X + \text{two's complement of } Y$
    - $\equiv X + (\text{One's complement of } Y + 1)$
- To get one’s complement of $Y$:
    - Use ==NOT== gates to get one’s complement of $Y$
- To add one:
    - Feed $1$ as carry-in in the least significant adder

![](https://i.imgur.com/ccl7rXS.png)

## Addition/Subtraction Circuit

- ? Instead of doing two circuits for adding and subtracting, what if we just combine them into one?
    - Use ==XOR== gates
- Add a `Sub` signal (for subtraction)
    - `Sub` is 0 $\implies$ Addition
    - `Sub` is 1 $\implies$ Subtraction

![|509](https://i.imgur.com/5u4X9A1.png)

- If `Sub` is 0:
    - $C_{\text{in}}$ becomes 0
        - → No more $+1$
    - Whatever $Y_{i}$ is for $i = 0, \dots, \text{\# of bits } - 1$ will come through the XOR gate
- If `Sub` is on:
    - $C_{\text{in}}$ becomes 1
    - Each XOR gate becomes inverter
        - Anything XORed with $1$ gives you the opposite
        - $0 \oplus 1 = 1$
        - $1 \oplus 1 = 0$

## Overflow Flag

> [!question]+ What happens if we add two positive signed binary numbers $0110 + 0011$?
> - i.e., $6 + 3$
> - Result is $1001$
> - ! But that is a negative number ($-7$)

> [!question]+ What happens if we add two negative numbers $1000 + 1111$?
> - i.e., $-8 + (-1)$
> - Result is $\cancel{ 1 }0111$ with carry-out

> [!attention]+ We need to know when the result might be wrong!
> - Usually indicated in hardware by **overflow** flag
>     - Says: The two values added together have created a negative result
>         - ==Value cannot be trusted==

- More about this in [[Processors|processors]]

## Sign and Magnitude Representation

- Some older processors use **sign and magnitude representation**
- Sign:
    - One bit is designated as the sign ($+/-$)
        - 0 for positive numbers
        - 1 for negative numbers
- Magnitude:
    - Remaining bits stored the positive version of the number
        - i.e., Unsigned
- e.g., Four-bit binary numbers:
    - $0110$ is 6
    - $1110$ is $-6$
    - MSB is the sign
- & Sign-magnitude computation is more complicated
    - Most systems use two’s complement today
