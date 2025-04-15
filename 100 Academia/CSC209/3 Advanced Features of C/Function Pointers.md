---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 6
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "11"
Chapter: 
Slides/Notes: 
Date: 2025-02-13
Date created: Fri., Feb. 14, 2025, 2:33:31 pm
Date modified: Sun., Mar. 9, 2025, 6:19:47 pm
---

# Function Pointers

## Introducing Function Pointers

> [!abstract]+ We have always differentiated between *data* — values — and *functions* — the C code being executed, and we don’t need to.
> - Functions can be data, too
> - In particular:
>     - May want to pass functions into arguments or store functions in structures
>     - & To do this, we’ll need **function pointers**

Example code we can use to explore the runtime of various sorting functions:

```c title:"sorts.c (Code needed to compile examples)"
void swap(int *arr, int index1, int index2) {
    int temp = arr[index1];
    arr[index1] = arr[index2];
    arr[index2] = temp;
}

/* Notice that all of these sorts have the same signature. They 
 * all take an integer pointer and an integer and have void return type.
 */
void bubble_sort(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        for (int j = 1; j < size - i; j++) {
            if (arr[j - 1] > arr[j]) {
                swap(arr, j - 1, j);
            }
        }
    }
}


void selection_sort(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        int current = i;
        for (int j = i; j < size; j++) {
            if (arr[current] > arr[j]) {
                current = j;
            }
        }
        swap(arr, i, current);
    }
}

void insertion_sort(int *arr, int size) {
    for (int i = 1; i < size; i++) {
        for (int j = i; j > 0 && arr[j - 1] > arr[j]; j--) {
            swap(arr, j - 1, j);
        }
    }
}
```

```c title:"compare_sorts.c (initial version)"
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

// compile this code with gcc -Wall compare_sorts.c sorts.c

// 3 sorts that we might like to try
void bubble_sort(int *, int);
void selection_sort(int *, int);
void insertion_sort(int *, int);

// confirm that our sort works correctly
void check_sort(int *arr, int size) {
    for (int i = 1; i < size; i++) {
        if (arr[i - 1] > arr[i]) {
            printf("Mis-sorted at index %d\n", i);
            return;
        }
    }
}


// fill arr with random values
void random_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = rand();
    }
}

void max_to_min_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = size - i;
    }
}


/* Time a particular sort on an array of size integers */
double time_sort(int size) {
    int arr[size];
    random_init(arr, size);

    clock_t begin = clock();
    // To change the sort, we have to hardcode the new name and recompile
    // insertion_sort(arr, size);
    bubble_sort(arr, size);
    clock_t end = clock();

    check_sort(arr, size);

    return (double)(end - begin) / CLOCKS_PER_SEC;
}


int main() {
    srand(time(NULL));

    for (int size = 1; size <= 4096; size *= 2) {
        double time_spent = time_sort(size);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

- These three sort functions are defined in another file
    - Each of them takes an integer pointer (the array to be sorted) and an integer (the size of the array being sorted)

- % We have two helper functions `check_sort` and `random_init`
    - Verify that the sort worked correctly and initialize an array to be sorted
- % Have a function that times the sort
    - `time_sort`
    - Code relies on calls to **`clock`**
        - *System call* that returns the amount of CPU time used so far
    - Get the amount of CPU time used by the code between the `clock` calls by subtracting the start time from the end time

- $ We hard-code the name of the sort that we want to test
    - Line 38

> [!danger]+ Problem
> - Have to change the code every time I want to explore a different sort or different initialization of the array to be sorted

- $ Both initialization functions have the same *signature*
    - i.e., Have same return type and take the same number and type of arguments
    - & Both initialization functions have the *same type*

> [!question]+ What is the type of a function?
> `void … (int *, int)`
>
> - Function returns a void and takes an integer pointer and an int
> - Name does not matter
>     - Like any other variable
>     - Just a name we use to refer to a value
>     - In this case, the value is a function

> [!question]+ How do we declare a variable of type pointer to a function?
> - Give it a *name*
> - Type *pointer* to a function
>     - & Name is always preceded by `*`

```c title:compare_sorts.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>


void bubble_sort(int *, int);
void selection_sort(int *, int);
void insertion_sort(int *, int);


void check_sort(int *arr, int size) {
    for (int i = 1; i < size; i++) {
        if (arr[i - 1] > arr[i]) {
            printf("Mis-sorted at index %d\n", i);
            return;
        }
    }
}


void random_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = rand();
    }
}

// Initialize the values in arr to be in decreasing order.
// This is the reverse of what our sorting function will provide.
void max_to_min_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = size - i;
    }
}

// Now our time_sort takes both the sorting function and the initializer
// function as parameters. This function does not need to be recompiled
// to change these values.
double time_sort(int size, void (*sort_func)(int *, int), void (*initializer)(int *, int)) {
    int arr[size];
    // We are calling the function received as a parameter.
    initializer(arr, size);

    clock_t begin = clock();
    // We are calling the function received as a parameter.
    sort_func(arr, size);
    clock_t end = clock();

    check_sort(arr, size);

    return (double)(end - begin) / CLOCKS_PER_SEC;
}


int main() {
    srand(time(NULL));

    for (int size = 1; size <= 4096; size *= 2) {
        // insertion_sort and random_init are functions. In the line below
        // they are being passed as parameters to time_sort
        double time_spent = time_sort(size, insertion_sort, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

- Change `time_sort` function to add two function pointer as additional arguments
    - Function pointers are called `sort_func` and `initializer`
- Call `initializer` and `sort_func` as if it is a function
    - Do *not* need to reference the pointer
- When calling `time_sort`:
    - Pass in the name of the function you want to use as a sort function and initialization function
    - ! Do *not* call the function
        - Just pass in the name as if it is a normal variable
        - Which actually is happening
- & A function name is treated as a pointer to that function
    - Just as how an array name is treated as a pointer to its first element

> [!summary]+ Inside `time_sort`, we call the sort function that was passed to us, instead of a hard-coded sort function.
> - Function pointers are no more difficult than any other type you’ve seen
> - Just have the address of a function as data, instead of an integer or string

## Extending the Worked Example

> [!danger]+ Problem
> - We still have to change the line of code that called our timing function and recompile our program to have it time a different sort

> [!goal]+ Extend our code to allow the user to select the sort to run from the command line

To do this, we need to accept data from the command line:

```c
int main(int argc, char **argv) {
    srand(time(NULL));

    for (int size = 1; size <= 4096; size *= 2) {
        // insertion_sort and random_init are functions. In the line below
        // they are being passed as parameters to time_sort
        double time_spent = time_sort(size, insertion_sort, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

- Add `argc` and `argv` arguments

We need to use that command line argument to select which sort function to execute:

```c
int main(int argc, char **argv) {
    srand(time(NULL));
    
    int sort;
    for (sort = 0; sort < NUM_SORTS && strcmp(argv[1], SORTS[sort].name) != 0; sort++);
    
    for (int size = 1; size <= 4096; size *= 2) {
        double time_spent = time_sort(size, SORTS[sort].sort_func, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

> [!note] Could use another command line argument to determine how we initialize the array

- First loop:
    - Iterates through an array called `SORTS`
    - Checks if the name associated with the sort is the name our user typed in
- ? What is `SORTS`?
    - Array of structs
    - Each element has a string `name` and function pointer `sort_func`

```c title:"sort_info struct"
typedef struct {
    char *name;
    void (*sort_func)(int *, int);
} sort_info;
```

- `sort_info` struct:
    - Has string `name`
        - Which we will compare to the string the user provides on the command line
    - Has function pointer `sort_func`
        - Right type to store a sorting function

```c title:"SORTS"
const int NUM_SORTS = 3;

sort_info SORTS[] = {
    {.name = "bubble", .sort_func = bubble_sort},
    {.name = "selection", .sort_func = selection_sort},
    {.name = "insertion", .sort_func = insertion_sort}
};
```

- Initialize a couple of constants
    - → Can call any of the three sort functions available

> [!note]+ Syntax
>
> ```c
> SORTS[]
> ```
> - Have an array whose name is `SORTS`
> ```c
> sort_info SORTS[]
> ```
> - Each element is of type `sort_info`
>     - Struct we just defined
> ```c
> {
>     {.name = "bubble", .sort_func = bubble_sort},
>     {.name = "selection", .sort_func = selection_sort},
>     {.name = "insertion", .sort_func = insertion_sort}
> };
> ```
> - Each of the three elements of `SORTS` (which are all structs) are assigned initial values

![|304x565](https://i.imgur.com/SrWI5pM.png)

- Do not need to recompile for our code to execute a different sorting function!
- Two elements that we added:
    - An *array* that contained the functions
    - Code to *identify* which element of the array contains the function user has asked to execute
- $ Identification code is in `main`
    - Seems like that code should be improved
        - No error-checking

> [!question]- What happens if we run program without command line argument?
> - Segmentation fault

Move the argument parsing to its own function:

```c
int main(int argc, char *argv[]) {
    srand(time(NULL));

    parse_command_line(argc, argv);

    for (int size = 1; size <= 4096; size *= 2) {
        double time_spent = time_sort(size, sort_func, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

> [!question]- What should the return type of this function be?
> - Potentially `int`
>     - Could return index of the `SORTS` array that contains the function we want to execute
> - But, why not just return the function pointer itself?
>     - Type `void (*sort_func)(int *, int)`
>     - Since sorting functions return `void` and take arguments of type `int *` and `int`

```c
int main(int argc, char *argv[]) {
    srand(time(NULL));

    void (*sort_func)(int *, int) = parse_command_line(argc, argv);

    for (int size = 1; size <= 4096; size *= 2) {
        double time_spent = time_sort(size, sort_func, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

Need to write our helper function `parse_command_line`:

```c
... parse_command_line(int argc, char *argv[]) {
    if (argc > 1) {
        for (int sort = 0; sort < NUM_SORTS; sort++) {
            if (strcmp(argv[1], SORTS[sort].name) == 0) {
                return SORTS[sort].sort_func;
            }
        }
    }
    fprintf(stderr, "Unrecognized sort name. Valid names are:\n");
    for (int i = 0; i < NUM_SORTS; i++) {
        fprintf(stderr, "  %s\n", SORTS[i].name);
    }
    exit(1);
}
```

- Two places where there could be errors:
    1. `argv[1]` could fail if not enough arguments
    2. If we exit the loop without finding the right function, then the user didn’t enter a valid sort

> [!question]- How do we declare a function pointer return value?
> - Syntax is ugly
> - ? How did we declare a function pointer variable?
>     - `void (*sort_func)(int *, int)`
>     - $ Variable name is in the *middle* of the type
>     - & Same thing happens when we declare a function that returns a function pointer
>         - `void (*parse_command_line(int argc, char **argv))(int *, int)`
>         - Name and arguments are in the middle of the type
>         - This is actually consistent

```c
void (*parse_command_line(int argc, char **argv))(int *, int) {
    if (argc > 1) {
        for (int sort = 0; sort < NUM_SORTS; sort++) {
            if (strcmp(argv[1], SORTS[sort].name) == 0) {
                return SORTS[sort].sort_func;
            }
        }
    }
    fprintf(stderr, "Unrecognized sort name. Valid names are:\n");
    for (int i = 0; i < NUM_SORTS; i++) {
        fprintf(stderr, "  %s\n", SORTS[i].name);
    }
    exit(1);
}
```

- Read return type as “the function `parse_command_line`, which takes an `int` and a pointer to pointer to `char`, returns a pointer to a function that takes an `int` pointer and an `int` and returns `void`”

> [!tip]+ Use `typedef` to create an *alias* for the function pointer type
>
> ```c
> typedef void (*SortFunc_t)(int *, int);
> ```

- Line of code defines a new type, `SortFunc_t`
    - Is a function pointer
    - → Change our code whenever we declared a variable — or a return value — that was a function pointer, to use new `SortFunc` type

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>


typedef void (*SortFunc_t)(int *, int);

typedef struct {
    char *name;
    SortFunc_t sort_func;
} sort_info;

void bubble_sort(int *, int);
void selection_sort(int *, int);
void insertion_sort(int *, int);

const int NUM_SORTS = 3;
sort_info SORTS[] = {
    {.name = "bubble", .sort_func = bubble_sort},
    {.name = "selection", .sort_func = selection_sort},
    {.name = "insertion", .sort_func = insertion_sort}
};


SortFunc_t parse_command_line(int argc, char *argv[]) {
    if (argc > 1) {
        for (int sort = 0; sort < NUM_SORTS; sort++) {
            if (strcmp(argv[1], SORTS[sort].name) == 0) {
                return SORTS[sort].sort_func;
            }
        }
    }
    fprintf(stderr, "Unrecognized sort name. Valid names are:\n");
    for (int i = 0; i < NUM_SORTS; i++) {
        fprintf(stderr, "  %s\n", SORTS[i].name);
    }
    exit(1);
}


void check_sort(int *arr, int size) {
    for (int i = 1; i < size; i++) {
        if (arr[i - 1] > arr[i]) {
            printf("Mis-sorted at index %d\n", i);
            return;
        }
    }
}


void random_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = rand();
    }
}


void max_to_min_init(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = size - i;
    }
}


double time_sort(int size, SortFunc_t sort_func, SortFunc_t initializer) {
    int arr[size];
    initializer(arr, size);

    clock_t begin = clock();
    sort_func(arr, size);
    clock_t end = clock();

    check_sort(arr, size);

    return (double)(end - begin) / CLOCKS_PER_SEC;
}


int main(int argc, char *argv[]) {
    srand(time(NULL));

    SortFunc_t sort_func = parse_command_line(argc, argv);

    for (int size = 1; size <= 4096; size *= 2) {
        double time_spent = time_sort(size, sort_func, random_init);
        printf("%d: %f\n", size, time_spent);
    }

    return 0;
}
```

![|300](https://i.imgur.com/qO3l59u.png)

- Timing function is a general function
    - Do not need a different function for each sort function
    - Do not need if statement at the point where the sort function is being called
- Can change the sort that is run based on the user’s input
    - Without a messy if statement

> [!info]+ We typically use this kind of functionality when we have a struct which may need to be handled in different ways depending on the value it stores.
> - Then, the struct stores both the value and the function used to handle it.
> - This is the beginning of the idea of an object in OOP

## Example with `qsort`

> [!impl]+ You might want to customize how sorting happens.
>
> - e.g., Compare elements using $<$ or $>$
> - → Determine whether array is sorted in ascending order or descending order
> - `qsort` lets you **pass in a function** to compare two elements in an array

- Alternative would be to write two `qsort` methods
    - One for ascending, one for descending
    - ! This is not great; just copying and pasting code

### `man qsort`

```bash
heapsort, heapsort_b, mergesort, mergesort_b, qsort, qsort_b, qsort_r – sort functions

SYNOPSIS
     #include <stdlib.h>

     void
     qsort(void *base, size_t nel, size_t width, int (*compar)(const void *, const void *));

     void
     qsort_b(void *base, size_t nel, size_t width, int (^compar)(const void *, const void *));

     void
     qsort_r(void *base, size_t nel, size_t width, void *thunk, int (*compar)(void *, const void *, const void *));

DESCRIPTION
     The qsort() function is a modified partition-exchange sort, or quicksort.  The heapsort() function is a modified selection sort.  The mergesort() function is a modified merge sort with exponential search, intended for sorting data with pre-existing order.

     The qsort() and heapsort() functions sort an array of nel objects, the initial member of which is pointed to by base.  The size of each object is specified by width.  The mergesort() function behaves similarly, but requires that width be greater than or equal to “sizeof(void *) / 2”.

     The contents of the array base are sorted in ascending order according to a comparison function pointed to by compar, which requires two arguments pointing to the objects being compared.

     The comparison function must return an integer less than, equal to, or greater than zero if the first argument is considered to be respectively less than, equal to, or greater than the second.

     The qsort_r() function behaves identically to qsort(), except that it takes an additional argument, thunk, which is passed unchanged as the first argument to function pointed to compar.  This allows the comparison function to access additional data without using global variables, and thus qsort_r() is suitable
     for use in functions which must be reentrant.

     The algorithms implemented by qsort(), qsort_r(), and heapsort() are not stable; that is, if two members compare as equal, their order in the sorted array is undefined.  The mergesort() algorithm is stable.

RETURN VALUES
     The qsort(), qsort_b() and qsort_r() functions return no value.

     The heapsort(), heapsort_b(), mergesort(), and mergesort_b() functions return the value 0 if successful; otherwise the value -1 is returned and the global variable errno is set to indicate the error.

COMPATIBILITY
     Previous versions of qsort() did not permit the comparison routine itself to call qsort(3).  This is no longer true.

ERRORS
     The heapsort(), heapsort_b(), mergesort(), and mergesort_b() functions succeed unless:

     [EINVAL]           The width argument is zero, or, the width argument to mergesort() or mergesort_b() is less than “sizeof(void *) / 2”.

     [ENOMEM]           The heapsort(), heapsort_b(), mergesort(), or mergesort_b() functions were unable to allocate memory.
```

- `base`:
    - Array to sort
- `nel`:
    - Size of each member in that array
    - Have to do this since `base` is a `void *`
    - Type `void *` lets it support arrays of any type
- `width`:
    - Length of array
- `*compar`:
    - Function pointer

### Function Pointer Syntax

```c
int (*compar)(const void *, const void *)
```

- `compar` is a function pointer to a function that returns `int`
    - Takes arguments to two void pointers to arrays
    - Points to some code segment in memory that stores all of our functions

> [!tip]+ You do not need to dereference function pointers
>
> ```c title:"qsort Pseudocode"
> void qsort(..., int (*compar)(const void *first, const void *second)) {
>     compr(…, …);  // Do not need to dereference with a *!
> }
> ```
>
> If we are calling a function that takes in a function pointer, we also do not need to get the address of the function.
>
> ```c title:main.c
> int main() {
>     int arr[] = {1, 3, 2, 4};
>
>     qsort(arr, 4, sizeof(int), comp);  // Do not need to do &comp
>
>     for (int i = 0; i < 4; ++i) {
>         printf("%d ", arr[i]);
>     }
> }
> ```
>
> > [!note]+ It still works if you pass in `&comp`, but you can also just use `comp`.

#### Demo

```c
#include <stdio.h>
#include <stdlib.h>

int comp(const void *first, const void *second) {
    if (*((int *) first) < *((int *) second)) {
        return -1;
    } else if (*((int *) first) > *((int *) second)) {
        return 1;
    } else {
        return 0;
    }
}

int main() {
    int arr[] = {1, 3, 2, 4};

    qsort(arr, 4, sizeof(int), comp);

    for (int i = 0; i < 4; ++i) {
        printf("%d ", arr[i]);
    }
}
```
