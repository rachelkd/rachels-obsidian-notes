---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 3
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "5"
Chapter: 
Slides/Notes:
  - https://pcrs.teach.cs.toronto.edu/csc209-2025-01/content/challenges/110/1#video-140
Date: 2025-01-20
Date created: Tue., Jan. 21, 2025, 2:15:58 am
Date modified: Fri., Jan. 24, 2025, 9:15:56 pm
---

# C Memory Model

## Code and Stack Segments

> [!info]+ Key Concepts
> - Memory in C is logically divided into segments, each storing a specific type of data.
> - The **code segment** stores compiled machine instructions.
> - The **stack segment** stores local variables and function call information in a last-in-first-out (LIFO) manner.
> - Each function call creates a **stack frame** to store its local variables and arguments.

![|258](https://i.imgur.com/vaDLsou.png)

```c title:"sum.c"
#include <stdio.h>

int sum(int a, int b){
    int i;
    i = a + b;

    return i;
}

int z;

int main() {
    z = sum(1, 3);
    
    return 0;
}
```

- A `main` function calls a `sum` function, which adds two integers and returns the result
- A global variable `z` stores the result of the `sum` function

> [!important]+ Code Segment
> - Stores the compiled machine instructions of the program
> - Located near the top of the memory array (logical address space)
> - Contains the instructions for `main` and `sum`

> [!important]+ Stack Segment
> - Stores local variables and function call information
> - Operates in a ==LIFO== manner:
>     - Most recent function call is at the top of the stack
> - Each function call creates a **stack frame** to store:
>     - Local variables
>     - Function arguments
>     - Return address (where to resume execution after the function call)

> [!example]+ Stack Frame for `sum`
> - When `sum` is called:
>     - A stack frame is created for `sum` above `main`‘s stack frame
>     - The stack frame stores:
>         - Arguments `a` and `b`
>         - Local variable `i`
> - When `sum` returns:
>     - The stack frame is popped, freeing the memory.
>     - The return value is passed back to `main`

> [!warning]+ Variable Scope
> - Local variables are only accessible while the function that defines them is active
> - Once a function returns, its stack frame is deallocated, and its variables are no longer valid

### Visualizing Memory Layout

#### Code Segment

- Contains instructions for `main` and `sum`
- Located at the top of the memory array

#### Stack Segment

- Grows downward in memory
- Each function call adds a stack frame to the top of the stack
- Example:
  - `main`’s stack frame:
    - Local variable `z`
  - `sum`‘s stack frame (while `sum` is executing):
    - Arguments `a` and `b`
    - Local variable `i`

## Heap and Global Segments

> [!info]+ Key Concepts
> - **Global variables** are stored in the **global data segment**, which is separate from the stack and heap.
> - **String literals** (e.g., `"Hi!"`) are also stored in the global data segment.
> - **Dynamic memory** allocated with `malloc` is stored in the **heap**.
> - The **heap** grows downward, while the **stack** grows upward.
> - Accessing invalid memory (e.g., OS-reserved memory) results in a **segmentation fault (segfault)**.

```c title:mem.c
#include <stdio.h>
#include <stdlib.h>

int sum(int a, int b){
   int *p = malloc(sizeof(int) * 500);

   return a + b;
}

int z;

int main() {
    z = sum(1, 3);

    char *c_ptr = "Hi!";
    printf("Hi!");

    // This will give you a segmentation fault:
    // int *p = NULL;
    // p[0] = 'a';

    return 0;
}
```

- A global variable `z` is accessible throughout the program
- A string literal `"Hi!"` is stored in the global data segment
- Dynamic memory is allocated using `malloc` in the `sum` function

> [!important]+ Global Data Segment
> - Stores **global variables** (e.g., `z`) and **string literals** (e.g., `"Hi!"`)
> - Global variables are accessible throughout the program
> - String literals are constants stored in the global data segment, even if not assigned to a variable

> [!important]+ Heap Segment
> - Stores **dynamic memory** allocated with `malloc`
> - Memory allocated on the heap persists until explicitly freed with `free`
> ```c
> int *p = malloc(sizeof(int) * 500); // Allocate space for 500 integers on the heap
> ```

> [!warning]+ Segmentation Fault (Segfault)
> - Occurs when accessing invalid memory (e.g., OS-reserved memory or `NULL` pointers).
> - Common causes:
>     - Uninitialized pointers
>     - Dereferencing `NULL` pointers
>     - Accessing memory beyond allocated bounds

> [!example]+ Memory Layout
> - **Code Segment**:
>     - Stores compiled machine instructions
> - **Global Data Segment**:
>     - Stores global variables and string literals
> - **Heap**:
>     - Grows downward; stores dynamic memory
> - **Stack**:
>     - Grows upward; stores local variables and function call information
> - **OS-Reserved Memory**:
>     - Located at the highest addresses; inaccessible to programs
