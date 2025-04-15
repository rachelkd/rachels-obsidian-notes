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
  - "6"
Chapter: 
Slides/Notes:
  - https://share.goodnotes.com/s/CFtwyigrwQ0uSVb3Q3ON9G
  - https://share.goodnotes.com/s/vMj22qN5W04vQEMqVrF2fB
Date: 2025-01-20
Date created: Mon., Jan. 20, 2025, 11:46:40 pm
Date modified: Fri., Jan. 24, 2025, 11:22:59 pm
---

# Dynamic Memory

## Memory Allocation: Stack Vs Heap

- **Stack**
    - Memory used for function-specific variables
    - Variables are deallocated once the function returns
- **Heap**
    - Memory used for dynamically allocated variables
    - Memory remains allocated until explicitly freed by the programmer

### Stack Memory Example

- Variables declared inside a function are stored on the stack.

```c
  int i = 5; // ‘i’ is allocated on the stack
  int *pt = &i; // ‘pt’ points to ‘i’
```

> [!danger] If a function returns a pointer to a **stack-allocated variable**, the memory may be reused by other functions, leading to unexpected behaviour.

### Problem with Returning Stack Memory

```c
int* set_i() {
    int i = 5; // ‘i’ is allocated on the stack
    return &i; // Warning: Returning address of a stack variable
}
```

- **Problem**: After `set_i` returns, the memory for `i` is deallocated and can be reused by other functions, causing undefined behavior.

### Heap Memory Allocation

- Use `malloc` to allocate memory on the heap.

```c
void* malloc(size_t size); // Allocates ‘size’ bytes on the heap
```

- **Return Type**: `void*` (generic pointer)
    - Must be cast to the appropriate type

```c
int* set_i() {
    int* i_pt = (int*) malloc(sizeof(int)); // Allocate memory for an integer on the heap
    *i_pt = 5; // Assign value to the allocated memory
    return i_pt; // Return pointer to heap memory
}
```

> [!info] Heap memory ==persists== even after the function returns.

### Pointer Arithmetic

- Pointers store type information to ensure proper address calculation.

```c
int* ptr = malloc(4 * sizeof(int)); // Allocate memory for 4 integers
ptr++; // Moves pointer to the next integer (address increases by 4 bytes)
```

```c title:"Motivate Malloc"
#include <stdio.h>
#include <stdlib.h>


int *set_i() {
    int *i_pt = malloc(sizeof(int));
    *i_pt = 5;
    return i_pt;

    // Try commenting out the lines above and uncommenting the ones below
    // to see the problem if you don't use malloc.
    //int i = 5;
    //return &i;
}

// a function that appears to have nothing to do with i and pt
int some_other_function() {
    int junk = 999;
    return junk;
}

int main () {
    int *pt = set_i(); 

    // try this program with and without this function call 
    some_other_function();

    printf("but if I try to access i now via *pt I get %d\n", *pt);
    return 0;
}
```

- **Stack**:
    - Short-lived memory for function-specific variables
- **Heap**:
    - Long-lived memory for dynamically allocated variables
- `malloc`:
    - Allocates memory on the heap
    - Must be explicitly freed using `free`
- `void*`:
    - Generic pointer type returned by `malloc`
    - Must be cast to the appropriate type

## Allocating Memory on the Heap

> [!summary]+
> - `malloc` allocates memory on the *heap*, which persists beyond the function’s scope.
> - Memory for pointers is allocated on the *stack*, but the memory they point to (allocated by `malloc`) is on the heap.
> - `sizeof(int) * max_val` ensures the correct amount of memory is allocated, regardless of the machine’s integer size.

- Create an array storing squares of integers from 1 to `max_val`
- Use a helper function `squares` to build the array

```c title:"Malloc Array"
#include <stdio.h>
#include <stdlib.h>

/* 
 * Return an array of the squares from 1 to max_val.
 */
int *squares(int max_val) {
    int *result = malloc(sizeof(int) * max_val);
    int i;
    for (i = 1; i <= max_val; i++) {
        result[i - 1] = i * i ;
    }
    return result;
}

int main() {
    int *squares_to_10 = squares(10);
    
    // Print the squares
    int i;
    for (i = 0; i < 10; i++) {
        printf("%d\t", squares_to_10[i]);
    }
    printf("\n");

    return 0;
}
```

> [!def]+ `malloc` Usage
> - `malloc` allocates memory on the heap
> - Syntax:
>     - `void *malloc(size_t size)`
> - Returns a `void` pointer
>     - Must be cast to the appropriate type (e.g., `int *`)

> [!important]+ Memory Allocation
> - `int *result = malloc(sizeof(int) * max_val);`
>     - Allocates space for `max_val` integers on the heap
>     - `sizeof(int)` ensures portability across machines with different integer sizes

> [!warning]+ Common Mistakes
> - Allocating insufficient memory
>     - `malloc(max_val)` allocates `max_val` bytes, not `max_val` integers
> - Forgetting to free memory allocated with `malloc`, leading to **memory leaks**

> [!example]+ Array Notation vs. Pointer Notation
> - Array notation (`result[i - 1]`) is preferred for readability
> - Pointer notation (`*(result + i - 1)`) is functionally equivalent but less intuitive

1. What is the purpose of `malloc`?
    - Allocates memory on the heap, persisting beyond the function’s scope.
2. Why is `sizeof(int) * max_val` used in `malloc`?
    - Ensures the correct amount of memory is allocated for `max_val` integers, regardless of machine-specific integer sizes.
3. What is the difference between stack and heap memory?
    - Stack: Local variables, limited size, automatically deallocated.
    - Heap: Dynamically allocated memory, persists until explicitly freed.
4. What does the `squares` function return?
    - The address of the first element of the array on the heap.
5. What happens if you use `malloc(max_val)` instead of `malloc(sizeof(int) * max_val)`?
    - Allocates insufficient memory (`max_val` bytes instead of `max_val` integers).

## Freeing Dynamically Allocated Memory

> [!summary]+
> - Heap memory must be explicitly deallocated using `free`
> - Memory leaks occur when allocated memory is not freed, making it inaccessible
> - Dangling pointers are pointers that reference freed memory, leading to unsafe behaviour

```c title:"free.c"
#include <stdio.h>
#include <stdlib.h>

int play_with_memory() {
    int i;
    int *pt = malloc(sizeof(int));

    i = 15;
    *pt = 49;
    
    // free(pt);

    // What happens if you uncomment these statements?
    // printf("%d\n", *pt);
    // *pt = 7;

    // printf("%d\n", *pt);

    return 0;
}

int main() {
    play_with_memory();
    // play_with_memory();
    // play_with_memory();
    return 0;
}
```

- Declares an integer `i` on the stack
- Allocates space for an integer on the heap using `malloc`
- `pt` (a pointer on the stack) holds the address of the heap-allocated memory

- Before the function returns:
    - `i` (on the stack) holds the value `15`
    - `pt` (on the stack) holds the ==address of the heap== memory, which stores `49`
- After the function returns:
    - The stack frame for `play_with_memory` is removed
    - `i` and `pt` are deallocated
    - The heap memory (holding `49`) remains allocated but inaccessible

> [!warning]+ Memory Leak
> - The heap memory is not freed, causing a memory leak
> - In long-running programs (e.g., web browsers), repeated leaks can lead to **out-of-memory errors** (`ENOMEM`)

### Fixing Memory Leaks with `free`

`void free(void *ptr)`

- Takes a pointer to the beginning of a block of memory previously allocated by `malloc`
- Returns the memory to the system for reallocation
- Does not require specifying the size of the block; the memory management system tracks it

```c
#include <stdlib.h>

void play_with_memory() {
    int i = 15;
    int *pt = malloc(sizeof(int));
    *pt = 49;
    free(pt); // Deallocate memory to prevent leak
}
```

- After calling `free(pt)`:
    - The heap memory is deallocated
        - i.e., Tells memory management system that this block of memory is now available for other purposes
        - ! Does not actually reset the value of `pt`
        - ! Does not itself change the values in the block being freed
    - No memory remains allocated on the heap when `main` returns

### Dangling Pointers

> [!def]+ Dangling Pointer
> - A pointer that references memory that has already been freed
> - Dereferencing a dangling pointer is unsafe and can lead to undefined behaviour

```c
int *pt = malloc(sizeof(int));
*pt = 49;
free(pt);
// pt is now a dangling pointer
// Accessing *pt here is unsafe
printf("%d\n", *pt);
```

> [!warning]+ Risks of Dangling Pointers
> - The memory might still contain the original value, creating a false sense of correctness.
> - If the memory is reallocated, accessing it can corrupt data or crash the program.

## Returning an Address with a Pointer

> [!summary]+
> - Instead of returning the address of dynamically allocated memory, we can use a parameter to store it
> - This approach allows the return value of the function to be used for other purposes (e.g., error signaling)
> - A common mistake is using `int *` as the parameter type instead of `int **`

```c
#include <stdio.h>
#include <stdlib.h>

void helper(int **arr_matey) {
    // Allocate space for 3 integers on the heap
    *arr_matey = malloc(sizeof(int) * 3);

    // Use a local variable for convenience
    int *arr = *arr_matey;
    arr[0] = 18;
    arr[1] = 21;
    arr[2] = 23;
}

int main() {
    int *data;  // This exist on the STACK frame for main
    helper(&data); // Pass the address of data

    // Access one of the values
    printf("the middle value: %d\n", data[1]);

    // Free the allocated memory
    free(data);
    return 0;
}
```

- A function `helper` allocates an array of 3 integers on the heap
- The address of the array is stored in a parameter of type `int **`
- The calling function (`main`) passes the address of a pointer to `helper`
- Which variables exist on the stack and heap?
    - `data` variable exists on the ==stack== frame for `main`
    - `arr_matey` exists on the ==stack== frame for `helper`
        - `helper` makes the address that `arr_matey` points to (which is `data`) point to the address that `malloc` returns
        - New address that `*arr_matey == data` points to exists on the **heap**
        - i.e., The address value of the pointer that `arr_matey` points to is assigned an address in ==heap== memory

> [!important]+ Why Use `int **`?
> - `int *` would only modify a local copy of the pointer, not the original pointer in the calling function
> - `int **` allows the function to modify the original pointer by dereferencing it

> [!warning]+ Common Mistakes
> 1. Using `int *` as the parameter type:
>    - Modifies a local copy of the pointer, leaving the original pointer unchanged
> ```c
> void helper(int *arr) {
>     arr = malloc(sizeof(int) * 3); // Local copy is modified
> }
> ```
> 2. Placing the local variable declaration above `malloc`:
>    - Creates a local variable that holds the address of the allocated memory, but does not update the original pointer

> [!success]+ Correct Approach
> - Use `int **` as the parameter type.
> - Dereference the parameter to modify the original pointer.
> - Example:
> ```c
> void helper(int **arr_matey) {
>     *arr_matey = malloc(sizeof(int) * 3); // Modifies the original pointer
> }
> ```

### Memory Management

> [!def]+ Freeing Memory
> - Memory allocated with `malloc` must be explicitly freed using `free`.
> - Free memory as soon as it is no longer needed to avoid memory leaks.
> ```c
> free(data); // Free memory after last use
> ```

> [!warning]+ Dangling Pointers
> - Avoid accessing memory after it has been freed.
> ```c
> free(data);
> printf(“%d\n”, data[1]); // Unsafe: data is a dangling pointer
> ```

## Nested Data Structures

> [!def]+ Nested array
> - Array of pointers
> - Each element may itself point to an array

![|452](https://i.imgur.com/SEseoQY.png)

```c
#include <stdio.h>
#include <stdlib.h>

int main() {

    // this allocates space for the 2 pointers
    int **pointers = malloc(sizeof(int *) * 2); 
    // the first pointer points to a single integer
    pointers[0] = malloc(sizeof(int));
    // the second pointer pointes to an array of 3 integers
    pointers[1] = malloc(sizeof(int) * 3);

    // let's set their values
    *pointers[0] = 55;

    pointers[1][0] = 100;
    pointers[1][1] = 200;
    pointers[1][2] = 300;

    
    // do other stuff with this memory

    // now time to free the memory as we are finished with the data-structure
    // first we need to free the inner pieces
    free(pointers[0]);
    free(pointers[1]);
    // now we can free the space to hold the array of pointers themselves
    free(pointers);
    
    return 0;
}
```

- Allocate memory for an array of 2 pointers (`int **`)
- Allocate memory for a single integer and assign its address to the first pointer
- Allocate memory for an array of 3 integers and assign its address to the second pointer
- Free memory in the correct order to avoid memory leaks and dangling pointers

> [!important]+ Memory Allocation Order
> 1. Allocate memory for the top-level array of pointers (`int **`)
> 2. Allocate memory for each sub-array or individual element
> 3. Assign the addresses of the allocated memory to the top-level array

> [!warning]+ Common Mistakes
> - Freeing the top-level array before freeing the sub-arrays:
>     - Leads to dangling pointers and memory leaks
> - Forgetting to free memory:
>     - Results in memory leaks, especially in long-running programs

> [!success]+ Correct Memory Deallocation
> - Free memory in the reverse order of allocation i.e., *bottom-up*
>     1. Free the sub-arrays or individual elements
>     2. Free the top-level array

### Memory Management

> [!def]+ Freeing Memory
> - Each `malloc` call must have a corresponding `free` call
> - Free memory as soon as it is no longer needed to avoid memory leaks
> - Allocated memory will be freed when program terminates

```c title:"Dynamic Memory Practice 5"
#include <stdlib.h>
#include <stdio.h>
int main() {
    char **last_names;
    // Assign a dynamically allocated char * array of length 4 to last_names.
    // Then, allocate a character array of length 20 for each element of the array pointed to by last_names.
    last_names = malloc(sizeof(char *) * 4);
    
    for(int i = 0; i < 4; i++) {
        last_names[i] = malloc(sizeof(char) * 20);
    }
    
    // Typically, would free allocated memory in order of last_name elements first, then last name, but
    // that does not pass the test cases for this question
    
    return 0;
}
```
