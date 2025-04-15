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
  - "10"
  - "11"
Chapter: 
Slides/Notes: 
Date: 2025-02-11
Date created: Tue., Feb. 11, 2025, 2:27:20 am
Date modified: Tue., Feb. 11, 2025, 3:41:38 am
---

# The C Preprocessor

- **Preprocessor directives**
    - Start with `#`
    - Modify source code before compilation
- Common directives:
    - `#define`
        - Defines macros
    - `#include`
        - Includes header files
    - `#if`, `#elif`, `#else`, `#endif`
        - Conditional compilation
    - `#ifdef`, `#ifndef`
        - Check if a macro is defined

> [!info]+ Preprocessor Workflow
> - Preprocessor runs before the compiler
> - Replaces macros, includes header files, and processes conditionals
> - Outputs a single file for the compiler

## Simple Macros and Header Files

See [[Useful C Features - Typedef, Macros|Macros]].

```c title:ex1.c
#define MY_INT (42)

int x = MY_INT;  // Assigning defined value MY_INT
```

- `ccp ex1.c`
    - Command that executes the preprocessor on file `ex1.c`
- Preprocessor
    - Executes all the directives
    - Expands all the macros
    - → Creates a stream of tokens passed to compiler’s parser

<!-- break -->
- ! Preprocessor replaces all instances of `MY_INT` with *string* `42`
    - Replaces text with text
    - Replaces even text in comment!
- Patterns found within quotes are not replaced
    - e.g., `// Assigning defined value "MY_INT"` does not replace `MY_INT` with *string* 42

### Predefined Macros

- **Predefined macros**
    - & System-defined macros provide that compilation details
    - Provide general information about when or how a program is compiled
        - e.g., `__LINE__`, `__FILE__`, `__DATE__`, `__TIME__`
- Convention:
    - Surrounded by double underscores (`__`)
- **System-specific macros**
    - e.g., `__APPLE__`, `__gnu_linux__`
    - Useful if you need code to behave differently based on system that is running code
    - ! Cannot use these macros to print informational messages
        - Macros are only defined when they are true
        - → Need *conditional statements*

> [!example]+ Using Predefined Macros
>
> ```c
> #include <stdio.h>
> int main() {
>   printf("Line %d: %s compiled on %s at %s.\n", __LINE__, __FILE__, __DATE__, __TIME__);
>   return 0;
> }
> ```

### Conditional Compilation

- Use `#if`, `#elif`, `#else`, `#endif` for conditional logic
    - Example: Check system type
    - Alternative: `#ifdef` and `#ifndef` for checking if a macro is defined

> [!example]+ Conditional Compilation
>
> ```c
> #include <stdio.h>
> #if __APPLE__
> const char OS_STR[] = "OS/X";
> #elif __gnu_linux__
> const char OS_STR[] = "gnu/linux";
> #else
> const char OS_STR[] = "unknown";
> #endif
> int main() {
>   printf("Compiled on %s\n", OS_STR);
>   return 0;
> }
> ```
> - Macro language does not use curly braces
> - Each block in sequence of conditionals needs to be terminated by another directive

> [!example]+ Same result using `ifdef` and `defined` operator
>
> ```c
> #include <stdio.h>
> 
> #ifdef __APPLE__
> const char OS_STR[] = "OS/X";
> #elif defined(__gnu_linux__)
> const char OS_STR[] = "gnu/linux";
> #else
> const char OS_STR[] = "unknown";
> #endif
> 
> int main() {
>     printf("Compiled on %s\n", OS_STR);
>     return 0;
> }
> ```

- If `__APPLE__` is not defined, it has value `0` (false)
    - Otherwise, `1`
- In second example using `ifdef`:
    - Value associated with it is irrelevant
    - If `__APPLE__` is defined, then the if statement is true
        - Regardless of its value

### Debugging with Macros

- Use `#ifdef DEBUG` to include debug-specific code
    - Example: Print debug messages
    - & Define `DEBUG` using `-D` flag during compilation
        - `gcc -D DEBUG program.c`

> [!example]+ Debugging Macro
>
> ```c
> #include <stdio.h>
> int main() {
>     #ifdef DEBUG
>     printf("Running in debug mode at level %d\n", DEBUG);
>     #endif
>     return 0;
> }
> ```

- By default, `DEBUG` macro is defined with value `1`
    - Can set `DEBUG` to take another value
    - e.g., `gcc -D DEBUG=3 ex6.c`

### Header Files and Includes

See [[Compiling#Header Guards Preventing Multiple Inclusion|Compiling: Header Guards]].

> [!danger]+ `#include` copies the contents of a header file into the source file
> - Preprocessor ensures single inclusion of header files

> [!tip]+ Avoid multiple definitions using include **guards**
> - e.g., `#ifndef HEADER_H`, `#define HEADER_H`, `#endif`

## Function-like Macros

- & **Function-like macros**
    - Resemble functions but are expanded during pre-processing
- Defined using `#define` with parameters
    - e.g., `#define SET(var, flag) ((var) |= 1 << (flag))`
- Macros are text replacements, not functions
    - ! No type checking or evaluation of arguments before expansion

> [!warning]+ Avoid Function-like Macros
> - Macros are error-prone and difficult to debug
> - Prefer inline functions or constants in modern C code
> - Legacy code may still use macros, so understanding them is important

### Example: Bit Flag Macros

> [!example]+ Bit Flag Macros
>
> ```c
> #define PAGE_PRESENT  0
> #define PAGE_PROT     1
> #define PAGE_RW       2
> #define PAGE_USER     3
> #define PAGE_DIRTY    4
> #define PAGE_ACCESSED 5
>
> #define SET(var, flag)   ((var) |= 1 << (flag))
> #define ISSET(var, flag) ((var) & (1 << (flag)))
> ```

### Macro Expansion

- Macros are expanded textually
    - e.g., `SET(page_flag, PAGE_USER)` becomes `((page_flag) |= 1 << (3))`
- Nested macros are fully expanded
    - e.g., `ISSET(page_flag, PAGE_USER)` becomes `((page_flag) & (1 << (3)))`

> [!example]+ Macro Expansion
>
> ```c
> int page_flag = 0;
> SET(page_flag, PAGE_USER); // Expands to: ((page_flag) |= 1 << (3))
> printf("PAGE_USER: %d\n", page_flag); // Output: 8
> ```

### Common Issues with Macros

> [!warning]+ Side Effects in Macros
> - Arguments may be evaluated multiple times
> - Example: `GREATER(x, ++y)` expands to `((x) > (++y) ? (x) : (++y))`
>     - `++y` is evaluated twice, leading to unexpected behavior

> [!example]+ Side Effects
>
> ```c
> #define GREATER(a, b) ((a) > (b) ? (a) : (b))
> int x = 2, y = 1;
> printf("%d\n", GREATER(x, ++y)); // Output: 3 (y is incremented twice)
> ```

> [!warning]+ Nested Macros
> - Nesting macros can lead to invalid expressions
> - Example: `SET(SET(page_flag, PAGE_USER), PAGE_DIRTY)` expands to an invalid assignment

### Best Practices for Macros

> [!important]+ Always Parenthesize
> - Parenthesize arguments and entire macro definition
>     - Example: `#define GREATER(a, b) ((a) > (b) ? (a) : (b))`
> - Prevents order of operations errors

> [!important]+ Use `do-while` for Multi-statement Macros
> - Ensures macros behave like single statements
> - Example:
> ```c
> #define WARN(cond) \
>     do { \
>         if (cond) fprintf(stderr, "Warning: %s\n", #cond); \
>     } while (0)
> ```

### Alternatives to Macros

> [!info]+ Use Inline Functions
> - Inline functions provide type safety and avoid macro pitfalls
>
> ```c
> static inline int GREATER(int a, int b) {
>     return a > b ? a : b;
> }
> ```
