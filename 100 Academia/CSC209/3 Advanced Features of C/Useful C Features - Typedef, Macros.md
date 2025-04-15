---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [Macros, Typedef]
Week: 6
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "10"
  - "11"
Chapter: 
Slides/Notes: 
Date: 2025-02-11
Date created: Mon., Feb. 10, 2025, 6:01:31 pm
Date modified: Tue., Feb. 25, 2025, 3:12:13 pm
---

# Useful C Features

> [!def]+ Typedef
> - Allows you to create aliases for types
> - Evaluated by ==compiler== at compile-time

> [!def]+ Macros
> - Allows you to define keywords that are replaced by specified strings
> - Occurs when program is processed
>     - *Before* being compiled

## Typedef

```c
#include <stdio.h>

typedef unsigned int size_t;
```

- Type `size_t`
    - Type of `malloc`‘s parameter and return type of `sizeof`
    - Found in C standard library file `stddef.h`
    - Declares `size_t` to be an unsigned int
- `typedef`
    - Keyword
    - Allows you to define a name
        - e.g., `size_t`
    - Refers to an existing type
        - e.g., `unsigned int`

> [!tip] In the future, if you rather `size_t` to be type unsigned long, you can just change the `typedef`
> - Every piece of code that uses `size_t` would use `unsigned long`
>     - Rather than `unsigned int`

> [!danger]+ You are not creating a new type like `struct`s
> - Instead, you are ==providing a new name== for an existing type

- There are arguments for using `typedef` to make your code easier to read
    - Arguments for and against
- & Most agree that `typedef` makes structs easier to read
    - e.g., We do not want to say `struct student` every time
- `typedef struct student Student;`
    - Aliasing `Student` to be the type `struct student`

```c title:typedef_example.c
#include <stdio.h>
#include <stdlib.h>

// some programmers argue that this is a good idea
typedef unsigned int age_t; 
typedef unsigned int shoe_size_t;

void print_boot_size(shoe_size_t shoe_size) { 
    printf("Boot size:%d\n", shoe_size + 2);
}

struct student {
    char first_name[20];
    char last_name[20];
    int year;
    float gpa;
};
typedef struct student Student;

/* We could also have done it this way
typedef struct {
    char first_name[20];
    char last_name[20];
    int year;
    float gpa;
} Student;
*/

int main() {

    print_boot_size(7);
    // but the compiler won't object if we do this
    // so other programmers argue that using typedef here is not helpful
    age_t driving_age = 16;
    print_boot_size(driving_age);


    // Most agree that using typedef with structs is helpful
    Student s;
    Student *p;

    s.year = 2;
    p = malloc(sizeof(Student));

    return 0;
}
```

## Macros

> [!question] What if you wanted an alias for something other than a type?

- Macros are evaluated during **pre-processing**
    - Similar to `#include` statements
- Defined using **`#define` directive**
    - Replaces occurrences of the macro with its definition ==before compilation==

> [!tip]+ Macros are useful for **constants** and **simple expressions**
> - Avoids “magic numbers” in code
> - Example: `#define TAX_RATE 0.08`

> [!info]+ Macro Syntax
> - No equals sign or semicolon needed
> - e.g., `#define MAX_NAME_LENGTH 40`
>     - Preprocessor replaces `MAX_NAME_LENGTH` with `40`

> [!attention] All macros are ==capitalized==.

### Macro Parameters

- & Macros can take ==parameters==
    - Example: `#define WITH_TAX(x) ((x) * 1.08)`
    - Acts like a function but evaluated during pre-processing
    - More efficient than function calls

> [!warning]+ Common Mistakes
> - & No space between macro name and opening parenthesis
>     - [c] Incorrect: `#define WITH_TAX (x) ((x) * 1.08)`
>         - Pre-processor thinks macro has no parameters
>         - → Replaces `WITH_TAX` with the rest of the line, including parenthesized `x`
>     - [p] Correct: `#define WITH_TAX(x) ((x) * 1.08)`
> - & Always parenthesize parameters and entire definition
>     - Prevents order of operations errors
>     - [p] e.g., `#define WITH_TAX(x) ((x) * 1.08)`

### Examples

> [!example]+ Example
>
> ```c
> #define TAX_RATE 0.08
> #define WITH_TAX(x) ((x) * (1 + TAX_RATE))
>
> double purchase = 9.99;
> printf("Total with tax: %f", WITH_TAX(purchase));
> ```

> [!example]+ Incorrect Macro Definition
>
> ```c
> #define WITH_TAX2(x) (x * 1.08) // Missing parentheses around x
> double purchase = 9.99, purchase2 = 12.49;
> printf("Incorrect total: %f", WITH_TAX2(purchase + purchase2)); // Error: tax only applied to purchase2
> ```
> - `purchase + purchase2` is substituted for `x`
> - → `printf("Incorrect total: %f", (purchase + purchase2 * 1.08);`

```c title:macro_example.c
#include <stdio.h>

#define CLASS_SIZE 80
#define TAX_RATE 0.08

#define WITH_TAX(x) ((x) * 1.08)

// in this incorrect version we forgot to parenthesize the parameter
#define WITH_TAX2(x) (x * 1.08)

int main() {
    double midterm_grades[CLASS_SIZE];
    midterm_grades[1] = 54.3;

    double purchase = 9.99;
    printf("with tax that is %f\n", WITH_TAX(purchase));


    double purchase2 = 12.49;
    printf("with tax that is %f\n", WITH_TAX(purchase + purchase2));
    printf("with tax that is %f\n", WITH_TAX2(purchase + purchase2));

    return 0;
}
```
