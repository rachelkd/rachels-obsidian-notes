---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [Header Files, Make]
Week: 5
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "10"
  - "9"
Chapter: 
Slides/Notes: 
Date: 2025-02-06
Date created: Thu., Feb. 6, 2025, 12:05:52 am
Date modified: Mon., Mar. 10, 2025, 10:56:50 pm
---

# Compiling

## The Compiler Toolchain

- **Compiler toolchain**
    - Set of applications that let you transform source code into a running program

![](https://i.imgur.com/lGofMoi.png)

- We use `gcc` to compile **source code**
    - `gcc` creates an executable `a.out`
    - When we execute `a.out`, the instructions in the program execute
- Compiler is not a single program
    - `gcc` has to work on many different systems
        - e.g., Linux machines, PCs, Macs
        - Different processors built by different computers that speak different languages

![](https://i.imgur.com/IOSDtPn.png)

- **Compiler**
    - Any program that translates code in one language to a different language
- Typically, we think of compilers as accepting input from some *high-level* language e.g., C, and producing output in a *lower-level* language
    - e.g., Assembly
- Can run just this part of `gcc` using `-S` flag
    - `gcc -S hello_world.c`
    - Result `hello_world.s` is in Assembly
- **Assembly** code
    - Human-readable language
    - Represents the instructions that a computer actually runs

> [!summary]+ The compiler runs in three phases
>
> 1. **Front end**
>     - ![|399](https://i.imgur.com/smYuJiz.png)
>     - Translates source code to a largely language independent intermediate representation
>         - e.g., `gcc` transforms all of its input languages into two languages called **Gimple** and **Generic**
>         - Think of them as **abstract syntax trees**
>             - ![|350](https://i.imgur.com/aO34LWX.png)
>             - Graph structures where each node is an element of the program
> 2. **Middle end**
>     - Semantic analysis
>     - Compiler optimizes code
>     - Looks for ways to make it faster
> 3. **Back end**
>     - Translates intermediate language into Assembly language of the computer that will run the program

> [!note]+ The toolchain presented is idealized.
>
> - In reality:
>     - Some distinctions in-between components are blurred
>     - e.g., Optimizations also occur in front and back end of a compiler

- What we have just discussed is the **compiler**
- Back end produces Assembly code, not an `.out` file you can execute
- **Assembler**
    - Assembles Assembly code into object code

![](https://i.imgur.com/non3RWy.png)

- Invoke assembler directly using command `as`
    - e.g., `as hello_world.s -o hello_world.o`
    - Output is no longer human-readable
    - Output is object file with machine-code instructions and data
- ! We cannot run this file yet
    - e.g., `./hello_world.o` → `-bash: ./hello_world.o: Permission denied`

```bash
> gcc hello_world.s
> file hello_world.o
hello_world.o: Mach-O 640bit object x86_64
> file a.out
a.out: Mach-O 64-bit executable x86_64
```

- Running `gcc` on `hello_world.s` produces the file `a.out`
    - Assembles our code and invokes the final step to producing an executable file
- `file` command
    - Shows the format of the file
    - Confirms that `gcc` is invoking more than the assembler
        - $\because$ File it generates is an *executable* rather than an object

![](https://i.imgur.com/I3vQhSn.png)

- **Linking**
    - Final step in compilation process
- **Linker**
    - Takes one or more compiled and assembled object files
    - Combines them to create a file that is an ==executable== format
    - Final executable file is a **package**
        - Contains all of the instructions in program in addition to a data section containing items e.g., constant strings
        - Links to dynamic libraries
- **Libraries**
    - Contain object code that implements functions such as `printf`

> [!warning]+ The executable is not *portable*.
>
> - Cannot copy it to another machine and know that it will run
> - Everything from assembly code to object code and executable is produced for a specific type of machine
> - Executable is specific to the *type* of machine and *operating system*
>     - Maybe even system configurations

- & Executable needs to be loaded into memory when executed
- **Loader**
    - Loads executable into memory upon execution
    - Operating-system specific
- & Before we even start the compiler, we need to **pre-process** it
    - Prepare the source code for front end
        - e.g., `cpp hello_world.c`
    - Adds function prototypes for library and system calls required by the program
    - Interprets pre-processor directives
        - e.g., Macros
    - i.e., Every line that starts with a `#` needs to be cleaned up by the pre-processor

## Header Files

> [!question] What happens when you compile a program with multiple source files?

```c title:"Initial compare_sorts.c"
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
```

- `sorts.h`:
    - Compares the runtime of various sorting functions
    - Contains:
        - Includes at top of file
        - User-defined types
        - Various **function prototypes**
        - Array of the sorting algorithms that are implemented
- Sorting functions are implemented in another file `sorts.c`
- ? What happens if we try to compile by running `gcc compare_sorts.c`?
    - Linking errors
        - ![|400](https://i.imgur.com/eyfftkZ.jpeg)
    - Functions we prototyped are not implemented in the file
    - & Compile like `gcc compare_sorts.c sorts.c`
        - Have to list all the files that contain code to get `gcc` to compile

![](https://i.imgur.com/w5kg61B.png)

- Each file is assembled separately
- Object files are combined during **linking** stage
    - → Produces an executable

- **Separate compilation**
    - Another way to compile

      ```bash
        > gcc -c compare_sorts.c
        > gcc -c sorts.c
        > ls *.o
        compare_sorts.o  sorts.o
        > gcc compare_sorts.o sorts.o
        ```

    - Separately generate object files for two source files
    - Linked them in the `gcc` command
    - & If we make one change to one source code file, we only need to create the single object file that is changed
        - Instead of recompiling every file in a huge project
- ! If you change the types of variables — e.g., changing `int` to `long` in `sorts.c` — and only re-compile the single file, the compiler does not throw an error
    - Despite using inconsistent types!
    - Running executable fails
        - Values are mis-sorted and segmentation faults may arise

> [!abstract] Separate compilation is valuable enough that we work around this issue by improving the organization in our project using **header files**.

- **Header files**
    - Example of an **interface**
    - Should declare *what* functions do and what *types* they require
    - Without defining *how* they are actually implemented

> [!idea]+ Can think of a header file as declaring a *design*
>
> - The `.c` files *implement* the design

```c title:sorts.h
typedef void (*SortFunc_t)(int *, int);

void bubble_sort(int *, int);
void selection_sort(int *, int);
void insertion_sort(int *, int);
```

- Include header file using quotes `"`
    - Tells compiler it should use header file in current directory
    - Rather than header file in system library

```c title:sorts.c
#include "sorts.h"

void swap(int *arr, int index1, int index2) {
    int temp = arr[index1];
    arr[index1] = arr[index2];
    arr[index2] = temp;
}


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

- ? What happens if we run `gcc compare_sorts.c`?

```c title:"compare_sorts.c"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#include "sorts.h"


typedef struct {
    char *name;
    SortFunc_t sort_func;
} sort_info;

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

## Header File Variables

- Header files define the design to be implemented in source files
    - They include type and function declarations, allowing type mismatch detection between different source files
- Function prototypes are placed in one location, reducing redundancy

> [!danger]+ Problem
>
> - Defining variables in a header file causes multiple definitions across object files
> - When each source file includes the header and compiles separately, a copy of the variable is created in each object file
> - Results in linker errors due to duplicate symbols

> [!goal]+ Separate the declaration of the variable’s name and type from the definition that creates it
>
> ```c
> const int NUM_SORTS = 3;
> sort_info SORTS[] = {
>     {.name = "bubble", .sort_func = bubble_sort},
>     {.name = "selection", .sort_func = selection_sort},
>     {.name = "insertion", .sort_func = insertion_sort}
> };
> ```
>
> - ? Where should these variables actually be created?
>     - Actual definition should probably be in `sorts.c`
>     - Where functions are also defined
>     - ! `compare_sorts.c` needs to know the variables exist
>         - → Must be declared in the header file
> - ~ Remove assignment of values to variables and declare them to be `extern` in header file

- `extern`
    - “Externally defined”

### Use `extern` for Global Variables

- **Declaration** (in the header file):
    - Specifies the name and type ==without allocating memory==
    - Uses `extern` to indicate the variable is defined elsewhere

  ```c title:sorts.h
  extern const int NUM_SORTS;
  extern sort_info SORTS[];
  ```

- **Definition** (in `sorts.c`):
    - Allocates memory and assigns values to the variables

  ```c title:sorts.c
  const int NUM_SORTS = 3;
  sort_info SORTS[] = {
      {.name = "bubble", .sort_func = bubble_sort},
      {.name = "selection", .sort_func = selection_sort},
      {.name = "insertion", .sort_func = insertion_sort}
  };
  ```

### Static Variables: Limiting Scope

- By default, symbol names in C are externally visible
    - i.e., Available globally everywhere in program
- `static` limits the visibility of a variable to the file it is defined in
    - i.e., Makes variable local to the file
- Example:

  ```c
  static int x = 3;
  ```

    - Prevents `x` from being accessible in other files

> [!tip] Use `static` when a variable should be unique to each source file

### Header Guards: Preventing Multiple Inclusion

> [!goal]+ Prevent header file from being included more than once in any compilation
>
> - Multiply-included header will generate errors
>     - e.g., Duplicate symbols
> - Prevents errors from including the same header multiple times

> [!tip]+ Solution
>
> - Add a **guard condition**

- Implemented using preprocessor directives:

  ```c
  #ifndef SORTS_H
  #define SORTS_H

  typedef void (*SortFunc_t)(int *, int);
  void bubble_sort(int *, int);
  void selection_sort(int *, int);
  void insertion_sort(int *, int);

  typedef struct {
      char *name;
      SortFunc_t sort_func;
  } sort_info;

  extern const int NUM_SORTS;
  extern sort_info SORTS[];

  #endif // SORTS_H
  ```

- `#ifndef SORTS_H`
    - “If not defined, `SORTS_H`”
    - Checks if `SORTS_H` is defined
- `#define SORTS_H`
    - Defines it if not already defined
    - First time header file is encountered:
        - `SORTS_H` does not exist
        - Definition is executed
    - If header file is included again, `if` condition is true
        - → Contents of header file will not be processed
- `#endif`
    - Marks the end of the conditional inclusion
    - `// SORTS_H`
        - Comment for the human reader stating which if statement is being terminated

### Dependencies and Compilation

- **Dependencies**
    - ! Changing a header file requires recompiling all source files that include it
- Dependencies can be managed using **makefiles**

### Static Variables Inside Functions

> [!attention] `static` keyword has multiple meanings.

- `static` variables inside functions retain their values across function calls
    - i.e., `static` used in a definition of a local variable rather than global variable

```c
#include <stdio.h>

void function_example() {
    static int x = 3;
    printf("%d\n", x);
    x = x + 2;
}

int main() {
    function_example();
    function_example();
    function_example();
    return 0;
}
```

- The value of `count` persists between function calls instead of resetting each time

## Makefiles

> [!abstract]+ Makefiles help manage the compilation (or build) process by tracking dependencies and automating the recompilation of files that have been modified.
>
> - Especially useful for projects with multiple source files and header files

Recall the files we have been working on:

```bash
> ls
compare_sorts.c sorts.c    sorts.h
> gcc -c compare_sorts.c
> gcc -c sorts.c
> gcc sorts.o compare_sorts.o
```

- `compare_sorts.c` and `sorts.c`
    - Source files that need to be *compiled* and *linked*
    - Include header file `sorts.h`

> [!goal] Create a makefile to build the parts of the project that need it

- **Makefiles**
    - Composed of a sequence of rules

Each rule consists of:

- **Target**
    - The file to be built
- **Dependencies** (prerequisites)
    - Files that the target depends on
- **Recipe**
    - Commands that build the target if dependencies are newer than the target
    - If there are no dependencies:
        - Commands are only executed if target does not exist
    - ! The recipe must be indented with a ==*tab*==, not spaces

```makefile title:Makefile
compare_sorts: compare_sorts.c sorts.c
 gcc compare_sorts.c sorts.c -o compare_sorts
```

- Target:
    - `compare_sorts`
- Dependencies:
    - `compare_sorts.c` and `sorts.c`
- Recipe:
    - Compiles `compare_sorts` using `gcc`

- When you run command `make` with no arguments:
    - Looks for a file named `Makefile` in local directory
    - Checks rules it contains
- ! The rule in the example does not account for the header file!
    - Make does not detect that it needs to rebuild executable if header file is updated
- ! Also not taking advantage of separate compilation

<!-- break -->
- & Our executable actually depends on two ==object== files
    - → Need to add rules to build object files

```makefile title:Makefile
compare_sorts.o: compare_sorts.c sorts.h
    gcc -c compare_sorts.c -o compare_sorts.o

sorts.o: sorts.c sorts.h
    gcc -c sorts.c -o sorts.o

compare_sorts: compare_sorts.o sorts.o
    gcc compare_sorts.o sorts.o -o compare_sorts
```

- $ Only the first rule gets built when `make` is run without arguments because make evaluates only the first rule in the Makefile by default
- If you use a command-line argument to specify a particular target, it will evaluate that rule

### Running Make

- Running `make` without arguments executes the first rule in the Makefile.
- Running `make <target>` compiles only the specified target.

> [!tip] Each time `make` evaluates a rule, it will first check all dependencies.
>
> - If dependency is also a target in the makefile, it will evaluate that rule first before checking the dependencies

#### Example Workflow

1. Modify `compare_sorts.c`:

    ```
    touch compare_sorts.c
    ```

2. Run `make compare_sorts`:

    ```
    make compare_sorts
    ```

    - `make` detects that `compare_sorts.c` has changed
    - It recompiles `compare_sorts.o`
    - Then, it relinks `compare_sorts`

### Wildcards, Cleaning Up, and Variables

> [!missing]+ It is tedious to create a separate rule for each source file and tell `make` what to build each time
>
> - Makefile supports **wildcards** of various sorts

```makefile title:Makefile
OBJFILES = compare_sorts.o sorts.o

%.o: %.c sorts.h
 gcc -c $< -o $@

compare_sorts: $(OBJFILES)
 echo making this target
 gcc $(OBJFILES) -o compare_sorts

.PHONY: clean
clean:
 rm compare_sorts $(OBJFILES)
```

- `%` is a wildcard representing filenames
    - Each object file that needs to be built depends on a source file of the same name
- `$<` refers to the first dependency (source file)
- `$@` refers to the target (object file)

<!-- break -->
- A **phony** target
    - Allows defining commands that don’t generate actual files
- `.PHONY` indicates that `clean` is not actually a file
    - Running `make clean` removes compiled files to force a full rebuild next time

<!-- break -->
- **Variables**
    - Simplify maintenance by avoiding repetition
- If new source files are added:
    - Update `OBJFILES` instead of modifying multiple rules

## Lecture 10: Header Files, Make

### Header Files

```bash
gcc -Wall -c main.c
```

- `-c` flag:
    - Creates an **object file**
    - Contains executable (machine) code for file `main.c`
    - When all object files are linked together, it will resolve their pointers and produce a single executable

```bash
gcc -Wall -c ll.c
gcc -Wall -c main.c
gcc -Wall -c stack.c
gcc -Wall -g -o main main.o ll.o stack.o
```

- `gcc -Wall -g -o main main.o ll.o stack.o`
    - Links the object files together

<!-- break -->
- **Separate compilation**
    - [p] If we only change `main.c`, the only thing we need to re-compile is `main.c` to make `main.o`
    - Minimize the amount of work the compiler needs to do

> [!important]+ You do not ever use memory in your header files!
> - e.g., Do not declare local variables in header files
> - If you want to use a local variable, use `extern`

### Makefiles

```make
test_print: test_print.o ptree.o
    gcc -Wall -g -o test_print test_print.o ptree.o
```

- ? What is the target?
    - `test_print`
- ? What are the pre-requisites or dependencies?
    - `test_print.o`, `ptree.o`
- ? How many actions does this rule have?
    - 1
