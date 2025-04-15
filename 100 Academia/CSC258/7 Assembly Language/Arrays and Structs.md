---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 11
Module:
  - 7 - Assembly Language
Lecture:
  - "32"
Chapter: 
Slides/Notes:
  - "[[assembly_slides.pdf]]"
Date: 2025-03-26
Date created: Sat., Apr. 12, 2025, 7:36:08 pm
Date modified: Sat., Apr. 12, 2025, 8:09:25 pm
---

# Array and Structs

## Arrays

- **Arrays** in assembly:
    - Stored in *consecutive locations* in memory
    - *Address* of array is the address of the array’s *first element*
- To access *element `i`* of an array:
    - Use `i` to calculate an *offset distance*
    - Add offset to address of first element to get address of `i`-th element
    - `offset = i * size of a single element`
- To operate on array elements:
    - *Load* array values into registers
    - Operate on them
    - Store them back into memory

### Translating Arrays

![](https://i.imgur.com/VO9ji1l.png)

## Example: a Struct Program

![](https://i.imgur.com/AyFE0f4.png)

- Struct
    - Something like an object in Java or Python without all the functions
    - Just a contiguous block of data
    - Useful for storing things like a date
        - Year, month, day
        - Instead of having three variables, use struct for a single date object
        - This is where the offset tends to come in
- `sw` lines:
    - Indicate that values in `$t1` are being stored at `$t0`, `$t0 + 4`, `$t0 + 8`
    - Each previous line sets the value of `$t1` to store
- $ Code stores the values `5`, `13`, `-7` into struct at location `a1`
