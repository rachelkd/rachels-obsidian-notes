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
Date created: Mon., Jan. 13, 2025, 3:08:18 am
Date modified: Sat., Jan. 25, 2025, 2:46:40 am
---

# Pointers

## Introduction

```c title:pointers_intro_int.c
#include <stdio.h>

int main() {
    int i;
    i = 5;
    printf("Value of i: %d\n", i);
    printf("Address of i: %p\n", &i);
    
    // declare a pointer pt that will point to an int
    int *pt;
    // set pt to hold the address of i
    pt = &i;
    
    // print the value of the pointer (which is the address of i)
    printf("Value of pt: %p\n", pt);
    // the pointer itself has an address, print that
    printf("Address of pt: %p\n", &pt);

    // print the value in the address that is itself stored in pt
    printf("Value pointed to by pt: %d\n", *pt);
    return 0;
}
```

```c title:pointers_intro_char.c
#include <stdio.h>

int main() {

    char ch = 'Y';

    // ch_pt is declared as a pointer to a char
    char *ch_pt;

    // store the address of ch in the variable ch_pt
    ch_pt = &ch;

    // dereference ch_pt to print the value pointed at
    // by ch_pt 
    printf("ch_pt points to %c\n", *ch_pt);
    
    return 0;
}
```

- **Dereferencing** a pointer
    - Get value in the address that is stored in pointer variable
    - Using `*`

## Assigning to Dereferenced Pointers

```c title:assign.c
#include <stdio.h>
int main() {

    // Run this code in the memory visualizer and 
    // look at the addresses and values carefully.

    // variables i and j are integers
    int i = 7;
    int j = i;  
    // variable pt is declared as type pointer to int
    int *pt;
    // the address of i is assigned as the value of pt
    pt = &i;
    // 9 is assigned to the variable pointed-at by pt
    *pt = 9;
    printf("Value of i: %d", i);

    // First the expression on the right is evaluated to give 10.
    // Then, 10 is assigned to the variable pointed-at by pt.
    *pt = *pt + 1;
    printf("pt points to %d\n", *pt);
    return 0;
}
```

## Pointers as Parameters to Functions

```c title:late_penalty.c
#include <stdio.h>

/* This function is supposed to lower the grade by one letter-grade. 
 * It doesn't work correctly. 
 */
void apply_late_penalty(char grade) {
   if (grade != 'F') {
       grade++;
   }
}

/* This version works correctly */
void properly_apply_late_penalty(char *grade_pt) {
   if (*grade_pt != 'F') {
       (*grade_pt)++;
   }
}

int main() {
   
    char grade_Michelle = 'B';
    printf("Michelle earned a %c\n", grade_Michelle);

    // We can add 1 to the char 'B' and get the next char in sequence.
    // Since the ASCII codes come in alphabetical order, this would be a 'C'
    grade_Michelle++;   
    printf("Michelle was late, so instead she gets a %c\n",grade_Michelle);

    // Felipe was also late on his assignment but he started with an A
    char grade_Felipe = 'A';
    printf("Felipe earned a %c\n", grade_Felipe);

    //  Let's call a function to lower Felipe's mark.
    apply_late_penalty(grade_Felipe);

    // Felipe's grade didn't change
    printf("Felipe was also late and earns a %c\n",grade_Felipe);


    // Let's call our improved function to lower Felipe's mark
    properly_apply_late_penalty(&grade_Felipe);
    printf("Felipe was also late and earns a %c\n",grade_Felipe);

    return 0;
}
```

## Passing Arrays as Parameters

```c title:array_sum.c
#include <stdio.h>

int sum(int *A, int size) {

    int total = 0;
    for (int i = 0; i < size; i++) {
        total += A[i];
    }
    return total;
}


int main() {

    int scores[4] = {4, 5, -1, 12};
    printf("total is %d\n", sum(scores, 4));

    int ages[3] = {10, 12, 19};
    printf("total is %d\n", sum(ages, 3));

    return 0;
}
```

```c title:array_change.c
#include <stdio.h>

int change(int *A) {
    // We explicitly change element 0 to 50 
    // in order to demonstrate that this change
    // lasts in the array that was passed to the function
    A[0] = 50;
}


int main() {

    int scores[4];
    // set the first element to 4
    scores[0] = 4;

    // call the function which is supposed to change it to 50
    change(scores);

    // What will be printed?
    // Will the function have changed the array scores or just a local
    // copy of that array?
    printf("First element in array has value %d\n", scores[0]);
    return 0;
}
```

## Pointer Arithmetic

```c title:pointer_arithmetic.c
#include <stdio.h>

int main() {

    // Try running this code in the visualizer and then try playing with the types
    // using chars instead of ints.
    
    int A[3] = {13, 55, 20};
    int *p = A;

    // dereference p
    printf("%d\n", *p);

    // When we add an integer to a pointer, the result is a pointer.
    int *s;
    s = p + 1;

    // dereference s to see where this new pointer points
    printf("%d\n", *s);

    // first add 1 to p and dereference the result
    printf("%d\n", *(p+1));

    // we can use array syntax on p
    printf("%d\n", p[0]);
    printf("%d\n", p[1]);

    p = p + 1;    
    printf("%d\n", *p);
    printf("%d\n", p[0]);
    printf("%d\n", p[1]);
    printf("%d\n", *(p - 1));

    return 0;

}
```
