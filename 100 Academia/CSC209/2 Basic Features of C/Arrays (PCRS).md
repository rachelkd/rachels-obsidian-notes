---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 2
Module:
  - "[[2 - Basic Features of C]]"
Lecture:
  - "3"
Chapter: 
Slides/Notes: 
Date: 2025-01-13
Date created: Mon., Jan. 13, 2025, 2:30:18 am
Date modified: Sat., Jan. 25, 2025, 2:46:14 am
---

# Arrays

## Introduction

```c
#include <stdio.h>

int main() {

    // we declare an array of 4 float values
    float daytime_high[4];

    // we initialize these values one at a time
    // later we will learn other ways to do this
    daytime_high[0] = 16.0;
    daytime_high[1] = 12.8;
    daytime_high[2] = 14.6;
    daytime_high[3] = 19.1;
    
    
    float average_temp = (daytime_high[0] + daytime_high[1] + daytime_high[2] + daytime_high[3]) / 4;

    // the variable index is used to access one of the array elements
    int index = 1;
    printf("On day %d, the high was %f.\n",index, daytime_high[index]);

    return 0;
}
```

## Accessing Array Elements

```c
int main() {
    int A[3];
    A[0] = 13;
    A[1] = 55;
    A[2] = 20;
    
    return 0;
}
```

- When line 2 is executed, an array of 3 elements is declared on the stack
    - ![|394](https://i.imgur.com/lHBqDOC.png)
    - Initial values are ==junk== — whatever was previously in those memory addresses
- After lines 3-5 execute:
    - ![|395](https://i.imgur.com/3zzUCMz.png)

> [!tip]+ We can accomplish the same rult in fewer lines of code using new syntax.
>
> ```c
> int main() {
>     int A[3] = {13, 55, 20};
>     
>     return 0;
> }
> ```

![](https://i.imgur.com/FhvcLgZ.png)

- Addresses are ==4 bytes== apart
    - Integer takes 4 bytes
    - When `A` is declared, compiler sets aside 12 contiguous bytes
    - 12 bytes in one chunk

> [!info]+ Arrays in Stack Memory
> - Space for the entire array is allocated when the array is declared
> - All elements are allocated in one place with no gaps between them
>
> > [!warning]+ Consequences
> > 1. Arrays cannot change in size
> >     - Can make larger array by declaring a larger array and copying elements to that new array
> > 2. Once we know the address where the array starts and size of each element, we can calculate the address of each element
> >     - Rely on this fact when we use indices to access elements of the array

- Address of array `A`
    - The address of element 0 of the array
- Address of the first element `A[1]`
    - `address of A + 4`
- Address of `A[i]`
    - `address of A + (i * size of one element of A)`

```c
int main() {
    // This code doesn't print anything.
    // To see what it is doing, run it in the memory visualizer.
    
    int A[3] = {13, 55, 20};
    int B[3] = {1, 2, 3};

    
    int in_bounds = A[1];

    // This line accesses memory that is not part of array A.
    // It might cause an error and stop the execution (crash) 
    // or it might appear to work and assign a random value to out_of_bounds.
    // Try changing it to int out_of_bounds = A[3000];
    int out_of_bounds = A[3];
    

    // We can assign to something out of bounds as well. It is wrong
    // but may or may not cause a visible error.
    A[5] = 999;

    return 0;
}
```

- C does not generally check that array access is within bounds of array
    - Does not typically store array bounds anywhere that it can be checked

> [!question]+ If you have an array of X elements, and you write to A[Y] where Y >= X, what happens?
> - Risk writing over another variable in program
> - Might appear to work perfectly fine
> - Might get an error

## Iterating Over Arrays

```c
#include <stdio.h>
#define DAYS 4

int main() {

    // this does the same thing as the first five lines of
    // daily_highs.c
    float daytime_high[DAYS] = {16.0, 12.8, 14.6, 19.1};
    
    // we initially use this variable to hold the running total
    float average_temp = 0;

    // We loop here over the array and add each element to the running total
    int i;
    for (i = 0; i < DAYS; i++) {
        printf("adding element %d with value %f\n", i, daytime_high[i]);
        average_temp += daytime_high[i];
    }

    // We divide by the number of items. The running total becomes the average.
    average_temp = average_temp / DAYS;
    printf("average %f\n", average_temp);
    return 0;
}
```
