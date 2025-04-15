---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [Bit manipulation, Bitwise operations, Flags]
Week: 10
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "17"
Chapter: 
Slides/Notes: 
Date: 2025-03-18
Date created: Mon., Mar. 17, 2025, 6:26:23 pm
Date modified: Mon., Mar. 17, 2025, 11:19:51 pm
---

# Bit Manipulation and Flags

## Introducing Bitwise Operations

- C is a really low-level programming language
    - → Can read and modify each bit in a value directly
    - → Work directly with raw data
        - Ignoring type of the variable
- @ Learn tools — operators that C gives us — to manipulate bits

Recall the Boolean operations that are possible on a single bit.

- `0`
    - False
- `1`
    - True

> [!summary]+ C provides a logical AND operator `&&`. C also provides a *bitwise AND operator* `&`.
>
> > [!obs]- The two operators look identical for a single bit.
> > ![|400](https://i.imgur.com/jBacEBF.png)
>
> ![|400](https://i.imgur.com/JZpKkiM.png)
>
> `1` is `0001` in binary; `8` is `1000` in binary.
>
> - Logical AND operator `&&`
>     - Looks at the whole number
>     - Returns true if both arguments have a value other than 0
> - Bitwise OR operator `&`
>     - Performs the AND operation on each *pair* of bits

> [!summary]+ Logical OR is `||`. C also provides a bitwise OR `|`.
> ![|400](https://i.imgur.com/ohF8Wsk.png)
>
> - Bitwise OR `|`
>     - Performs OR operation on each *pair* of bits
>     - Like bitwise AND

> [!summary]+ Bitwise XOR `^` (exclusive OR)
>
> ```c
> 1 ^ 1 == 0
> 1 ^ 0 == 1
> 0 ^ 1 == 1
> 0 ^ 0 == 0
> ```
> - `1` if and only if exactly *one* operand is 1
> - Result is `0` if both operands are 0 or both operands are 1
>
> <!-- break -->
> - % Exclusive OR is sometimes called **conditional negation**
>     - If first operand is 1 — true — then result is the *complement*
>         - Negation of the second operand
>         - `1 ^ 1 == 0`
>         - `1 ^ 0 == 1`
>     - If first operand is 0 — false — then result is the second operand *without* being negated
>         - `0 ^ 1 == 1`
>         - `0 ^ 0 == 0`

> [!summary]+ Bitwise negation (complement) operation `~`
>
> ```c
> ~0 == 1
> ~1 == 0
> ```
>
> - Takes a single value and flips every bit in the value
>     - `1`s become `0`s
>     - `0`s become `1`s
> - ! While bit `0` becomes bit `1`, the integer $0$ will *not* become integer $1$
>     - C representation of integer $0$ has many bits
>         - 32 bits (on most machines)
>     - & Negation would result in number with 32 1‘s

### Example

- @ Explore how to use bitwise operators in a C program

```c
#include <stdio.h>

int main() {
    // gcc allows you to enter binary constants by prefacing the
    // the number with 0b:
    char a = 0b00010011;    // decimal value: 19

    // Similarly, preface with 0x for hexadecimal constants:
    unsigned char b = 0x14; // decimal value: 20

    // Negation:
    printf("result of negative %x is %x in hex\n", a, ~a);

    // Bitwise AND:
    printf("result of bitwise AND of %x and %x is %x in hex\n",
        a, b, a & b);

    // Bitwise OR:
    printf("result of bitwise OR of %x and %x is %x in hex\n",
        a, b, a | b);

    // Bitwise XOR:
    printf("result of bitwise XOR of %x and %x is %x in hex\n",
        a, b, a ^ b);

    return 0;
}
```

- ? How do you store bits in a variable?
    - C language does not define non-decimal constants
    - `gcc` allows you to enter binary constants by prefacing number with `0b`
    - Can also use **hexadecimal** constants
        - Prefaced with `0x`
        - Hex is convenient because binary numbers are usually large
        - Easy to translate from hex to binary
            - Since one hexadecimal digit is 4 bits
    - % `0x13` is `0001 0011`
    - % `0x14` is `0001 0100`
- & Bitwise operators apply to each bit of the variable
    - Negating `a` will turn all 1s into 0s, and
        - All 0s into 1s
    - Result is `0xEC`
        - `1110 1100`

## The Shift Operator

> [!goal] Given a variable b, set the third bit to 1 and leave the other bits unchanged.

```c
int main() {
    char b = 0xC1;  // 1100 0001
}
```

- & We count bits from right to left starting at position 0
    - Bit at position 3 in our variable `b` has value 0
    - We want `1100 1001`
- & Can use bitwise OR operator
    - `1 | 0 == 1`
    - `1 | 1 == 1`
- & Use the value `8` (`0x08`, `0b1000)
    - Value `8` has a `1` at the third bit
    - Any value we combine with 8 will end up with a 1 at the 3rd bit using bitwise OR
    - ![|400](https://i.imgur.com/D8Axc6i.png)

> [!goal]+ Write an expression that results in 0 if the second bit of b is 0 and a non-zero value if the second bit is 1.
> - i.e., Given a variable b, check if the second bit is has the value 1.

```c
int main() {
    char b = 0xC1;  // 1100 0001
}
```

- & Use the `&` operator
    - Only way that bitwise AND results in `1` is if both bits are `1`
- Use the value `4`
    - Since it has a 1 at the position we want to check

![|400](https://i.imgur.com/UNhRJKo.png)

- If second bit of `b` were 1:
    - `1 & 1 == 1`
- In our example:
    - Second bit is 0
    - `1 & 0 == 0`

### In General

Suppose we want to make these problems more general.

- Solutions so far:
    - Rely on us knowing exactly which position we want to check
    - @ Need a way to create a value with an arbitrary bit set to one
        - & Use C’s **shift operator**

> [!goal]+ Given a variable `b`, set the `k`-th bit to 1, leaving all other bits of `b` the same.
> - Write an expression that results in 0 if the `k`-th bit of `b` is 0, and a non-zero value if the `k`-th bit is 1.

- Two shift operators:
    - Shift left `<<`
    - Shift right `>>`
    - Think of them as arrows pointing in the direction of the shift
- Shift operators take *two* operands
    - First operand (left of the operator)
        - Value to shift
    - Second (right of the operator)
        - Tells us how many places to shift

```
1 << 3 -> 0000 1000

0000 0001 -> 0000 0010 -> 0000 0100 -> 0000 1000
```

- If we shift 1 to the left 3 positions:
    - End up with 2 → 4 → 8
- By replacing second operand with a variable:
    - & Can generate a value with any single bit set

![|400](https://i.imgur.com/5AHlUMi.png)

- Set the `k`-th bit of `var`:
    - & Bitwise OR `var` with `1` shifted `k` times
- Check if `k`-th bit is set:
    - & Bitwise AND-ing `var` with `1` shifted `k` times

> [!info]+ *Setting* and *checking* a bit are two common operations.

![|400](https://i.imgur.com/G7dNmxK.png)

- We can use the *shift operators* on values other than 1
    - e.g., Shift a 1 byte value (8 bits) to the left by 1 position
        - $ Leftmost bit is discarded
        - $ Rightmost bit is set to 0
        - $ Other bits are shifted 1 bit to the left
    - e.g., Shift by 2
        - $ Leftmost two bits are discarded
        - $ Rightmost two bits are set to 0
        - $ Other bits are shifted 2 to the right

### Another Way to Think about Shift Operators

![|400](https://i.imgur.com/I9gLAY4.png)

- $ Left shift by 1 is the same as *multiplying* the number by *two*

![|400](https://i.imgur.com/v23qIh0.png)

- $ Right shift operator works like integer division by 2

## Bit Flags

- **Flag bits**
    - Commonly used by system calls
    - When a single argument is used to transmit data about multiple options
    - Variable is treated as an array of bits
        - Each bit represents an option — or *flag* — that can be turned on and off

 > [!goal] Explore how this concept is used to implement file permissions.

### How Do File Permissions Work in Linux?

Recall:

- Each file has:
    - An owner
    - A group

![|400](https://i.imgur.com/RRyo0hd.png)

- First column:
    - **Permission string**
        - Represents who can read, write, or execute the file
- Third column:
    - Owner of the file
- Fourth column:
    - Group
        - e.g., `instrs`
- Owner and group fields are relevant to permissions
    - & Linux allows us to set separate permissions for the *owner*, *group*, and every other user in the system

Let’s look more closely at the permissions for `syscall_cost`.

```bash title:"Permission string"
-rwxr-xr-x
```

- Leftmost dash `-`
    - Identifies the *file type*
    - `-` means *regular file*
    - `d` means *directory*
    - `l` means *link*
- Owner of the file has read, write, and execute permissions on the file
    - `rwx`
- Members of the group `instrs` has read and execute permissions
    - `r-x`
    - Cannot write the file
- Everyone else also has read and execute permissions
    - `r-x`

### Flag Bits and Permission Strings

> [!question]+ How does this relate to bit manipulation operators and flag bits?
> If you ignore the first dash that represents the file type:
> - Each file permission is essentially an on/off switch
>     - → Can represent each one with a **flag bit**

- Need nine bits to represent each permissions setting
    - Need a variable with enough space for 9 bits
        - Rules out storing it in a byte

We need nine bits to represent each permissions setting, so we need a variable with enough space for 9 bits. This rules out storing it in a byte.  The system calls that operate on file permissions use a mode_t type for the permissions string.

> [!info]+ The system calls that operate on *file permissions* use a `mode_t` type for the permissions string.
> - Some systems:
>     - Defined as an `unsigned int`
>     - 32-bit value
>
> - Permissions will be stored in the 9 low-order bits
>     - i.e., Rightmost bits

See how to turn the permissions string into a sequence of bits.

![](https://i.imgur.com/yh54m50.png)

- Each character in the permissions string becomes a bit in our variable representing the permissions
- Bits 8, 7, 6
    - Represent the read, write, and execute permissions for the *user*
-  Bits 5, 4, 3
    - Represent the permissions for the *group*
- Bits 2, 1, 0
    - Represent the permissions for *everyone else*

> [!question]+ What if we wanted to represent a file with read-only permissions?
> - Permission string
>     - `r--r--r--`
> - Represent with binary value $100100100$

- Could construct that value with bitwise operators

> [!tip]+ System calls that take a permissions variable as an argument use several *constants* to make it easier for us to construct these values.
> ![](https://i.imgur.com/ChIgXdY.jpeg)
> ![](https://i.imgur.com/zY0C0gk.jpeg)
>
> - `chmod`
>     - System call that operates on file permissions
> - `chmod` arguments:
>     - Path to file we want to set permissions for
>     - Mode/permissions to set
> - $ See a set of defined constants
>     - Will make it easier to set a value for the mode
> - % Constants are defined in *octal*
>     - Base 8
>     - Convenient for representing permissions because one digit in base 8 is *three* binary digits

- & Octal constant in C is written with a preceding 0

| Octal       | Permission |
| ----------- | ---------- |
| `04 == 100` | Read       |
| `02 == 010` | Write      |
| `01 == 001` | Execute    |

- Library defines constants for each possible permission

```c
#define S_IRUSR 0000400  /* R for owner */
#define S_IRGRP 0000040 /* R for group */
#define S_IROTH 0000004 /* R for other */
```

*Three constants for setting the read permission.*

- % `IR` stands for “input read”

> [!goal]+ Generate a read-only permission for a file

![](https://i.imgur.com/aWdNSJa.png)

- Create the permission value using bitwise operations in [[#Introducing Bitwise Operations]]
- To set bits:
    - Use bitwise OR with the appropriate constants
- & Combine the two flags for the group and other users using bitwise OR

![](https://i.imgur.com/PBi8EGU.png)

- & Use bitwise AND to check if either bit is set

> [!info]+ Flag bits are a compact, efficient method for representing data with multiple boolean components.
> - Use of individual bits, rather than whole characters or integers, saves *space*
> - Use of bitwise operators is clean and fast

## Bit Vectors

> [!goal]+ Explore the use of bit flags to implement a *set*.
> - If we use each bit in a variable to denote the presence — or absence — of a particular element in the set
>     - Have a very compact implementation
>     - Can use fast boolean instructions as set operations

- Possible set elements are represented by small positive integers

### Example: Set Colours

We might want to store colours in a set.

![](https://i.imgur.com/5YpvHGE.png)

- Give each colour a numeric code
    - → Store codes in the set

![|400](https://i.imgur.com/Pw73y0E.png)

- Set contains small positive integers
- & → Can represent set as an array of bits
    - Where the elements of the set are the indexes into the array
    - Value at a given index location tells us whether element is in the set
    - e.g., $7, 5, 3, 2$ are in the set because *bit array* contains *ones* at these indexes

![](https://i.imgur.com/Vr6DdJI.png)

- ? How to add another element, e.g., 10, to `bit_array`?
    - Use shift operator to create a value where 10th bit is 1 and all other bits are 0
    - Bitwise OR-ing it with `bit_array` adds 10 to the set
- **Bit masking**
    - Common programming technique
    - We are creating a **bit mask**
        - A carefully constructed value where specific elements are set (or not set)
        - Applying the mask to set or unset those values

![](https://i.imgur.com/QnZRCUd.png)

- ? How to remove 10 from the set?
    - Want to bitwise AND a zero in the 10th bit with `bit_array`
        - But if other bits are zero
            - → Side effect of removing all items from set
        - Need to create a value where all of the bits are one except the 10th bit
        - Then bitwise AND that value with `bit_array`

![](https://i.imgur.com/cWzM9lW.png)

- Used an unsigned short to hold the bit array
    - → Range of values: 0-15, inclusive
- → Can double the range of the set by changing type of our variable to `unsigned int`
    - But 31 is pretty small to be the maximum value

![](https://i.imgur.com/n5Gb2rm.png)

- & Can generalize data structure that we use to store the bits by making an array of `unsigned int`s
    - Operations to add a value to a set (and remove) are more complicated

![](https://i.imgur.com/qGUwB46.png)

Suppose we want to set the bit at index 34 to 1.

- Figure out which of the `unsigned int`s in array contains bit 34

![](https://i.imgur.com/A0wDL1Q.png)

- Each `unsigned int` contains 32 bits
- & Use integer division to divide 34 by 32 to get index into array of unsigned ints
    - In this case: `1`

![](https://i.imgur.com/Nur9PP9.png)

- Find the index of the bit in the unsigned int
- Use *mod* operator to find the remainder of the integer division from the previous step
    - `34 % 32 == 2`

![](https://i.imgur.com/OrI4fCH.png)

- To set bit at index 34 to 1:
    - Combine operations from earlier example

To set the bit at index 34 to 1, we combine the operations from the simple example earlier in the video. Now let’s put it all together and fill in the code to implement the set operations.

> [!tip]+ We will make one more alteration to the data structure, and wrap the array in a struct.
> - Two benefits to wrapping array in struct:
>     - Hides implementation of the set
>     - Allows us to use a simple assignment statement to make a copy of the set

```c title:bitsets.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>

#define INTSIZE 32
#define N 4

struct bits {
    unsigned int field[N];
};

typedef struct bits Bitarray;

/* Initialize the set b to empty or all zeros
 * Return 0 if memset fails.
 */
int setzero(Bitarray *b){
  return (memset(b, 0, sizeof(Bitarray)) == NULL);
}

/* Add value to the set b
 *   (set the bit at index value in b to one)
 */
void set(unsigned int value, Bitarray *b) {
    int index = value / INTSIZE;
    b->field[index] |= 1 << (value % INTSIZE);
}

/* Remove value from the set b
 *   (set the bit at index value in b to zero)
 */
void unset(unsigned int value, Bitarray *b) {
    int index = value / INTSIZE;
    b->field[index] &= ~(1 << (value % INTSIZE));
}

/* Return true if value is in the set b, and false otherwise.
 *    Return a non-zero value if the bit at index 'value' is one in b
 *    Return zero if the bit at index 'value' is zero in b
 */
int ifset(unsigned int value, Bitarray *b) {
    int index = value / INTSIZE;
    return ( (1 << (value % INTSIZE)) & b->field[index]);
}

/* Run some simple tests on the above functions*/
int main() {

    Bitarray a1;
    setzero(&a1);

    // Add 1, 16, 32, 65 to the set
    set(1, &a1);
    set(16, &a1);
    set(32, &a1);
    set(68, &a1);

    // Expecting: [ 0x00010002, 0x00000001, 0x000000010, 0 ]
    // Print using hexadecimal
    printf("%x %x %x %x\n",
            a1.field[0], a1.field[1], a1.field[2], a1.field[3]);

    unset(68, &a1);

    // Expecting: [ 0x00010002, 0x00000001, 0, 0 ]
    // Print using hexadecimal 
    printf("%x %x %x %x\n",
            a1.field[0], a1.field[1], a1.field[2], a1.field[3]);

    return 0;
}
```

- Using an array of unsigned ints
- Can choose an arbitrary value for `N`
- Create sets where the range of possible values in the set is quite large

<!-- break -->
- `setzero`
    - Initialization function
    - Uses `memset` library function to fill array with zeroes
- `set`
    - `index = value / INTSIZE`
    - `b->field[index] |= 1 << (value % INTSIZE)`
- `unset`
    - Key difference between `set` and `unset`:
        - For unset:
            - Need to make sure that every bit is a 1 *except* for the value to be unset
    - Set bit we want to a 1, then *negate* the whole value
    - Use AND operator
        - Rather than OR
    - `index = value / INTSIZE`
    - `b->field[index] &= ~(1 << (value % INTSIZE))`
- `ifset`
    - Checks if a bit is set
    - Requires a combination of operations from set and unset
    - Getting bit to check is the same as before
        - `1 << (value % INTSIZE)`
    - Use AND to check whether it is set in the original value
        - `1 << (value % INTSIZE) & b->field[index]`
    - If the bit was in the set, then the resulting value will not be 0 because one bit will be a one.
    - But if the bit was not in the set the resulting value will be 0 because `1 & 0` is 0
- `main`
    - Make some quick tests for our functions
    - % There is not a straight forward way to print a number in binary, so the tests print the values in the field array in hexadecimal format
