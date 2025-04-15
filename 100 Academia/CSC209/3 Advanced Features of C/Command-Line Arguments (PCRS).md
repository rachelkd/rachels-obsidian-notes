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
  - https://pcrs.teach.cs.toronto.edu/csc209-2025-01/content/challenges/118/1#video-48
Date: 2025-01-20
Date created: Tue., Jan. 21, 2025, 2:29:40 am
Date modified: Tue., Feb. 4, 2025, 12:56:57 am
---

# Command-Line Arguments

## Converting Strings to Integers

> [!summary]+
> - Use `strtol` to convert strings to integers
> - `strtol` handles leading spaces, signs, and trailing non-numeric characters
> - The `base` parameter allows conversion in different number systems (e.g., base 10, base 2)

### Using `strtol` for String-to-Integer Conversion

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    //char *s = "17";
    char *s = "  -17";
    int i = strtol(s, NULL, 10);

    printf("i has the value %d\n", i);


    s = "  -17 other junk.";
    char *leftover;
    i = strtol(s, &leftover, 10);

    printf("i has the value %d\n", i);
    printf("leftover has the value %s\n", leftover);
    return 0;
}
```

- Convert a string (e.g., `"17"`) to an integer using `strtol`
- Handle strings with leading spaces, signs, and trailing non-numeric characters

> [!def]+ `strtol` Function
> - Converts a string to a `long` integer
> - Syntax: `long strtol(const char *str, char **endptr, int base)`
> - Parameters:
>   - `str`: The string to convert
>   - `endptr`: Pointer to store the address of the first invalid character (can be `NULL`)
>   - `base`: Number system to use for conversion (e.g., 10 for decimal, 2 for binary)

> [!example]+ Handling Different Cases
> - Leading spaces: `strtol` skips them
> - Signs: Handles `+` or `-` at the beginning of the string
> - Trailing non-numeric characters: Stores the address of the first invalid character in `endptr`

> [!warning]+ Common Mistakes
> - Forgetting to include `<stdlib.h>` for `strtol`
> - Not checking for errors (e.g., invalid input or overflow)

## Command-Line Arguments

> [!summary]+
> - Use `argc` and `argv` to handle command-line arguments
> - `argc` (argument count) stores the number of arguments
> - `argv` (argument vector) is an array of strings containing the arguments
> - `argv[0]` is the name of the executable, and the rest are user-provided arguments

```c title:args_example.c
#include <stdio.h>
// Try running this code from the commandline as ./a.out fee fi fo fum 

int main(int argc, char **argv) {
    // argc is the number (or count) of arguments that were provided
    printf("%d\n", argc);
 
    // Notice the relationship of argv[0] to the executable.
    int i;	
    for (i = 0; i < argc; i++ ) {
        printf("argv[%d] has the value %s\n", i , argv[i]);
    }
    return 0;
}
```

- Print the number of command-line arguments (`argc`)
- Print each argument in `argv`, including the executable name

> [!important]+ `argc` and `argv`
> - `argc`: Number of command-line arguments (including the executable name)
> - `argv`: Array of strings containing the arguments
>     - `argv[0]`: Name of the executable
>     - `argv[1]` to `argv[argc - 1]`: User-provided arguments

> [!example]+ Running the Program
> - Command: `./a.out fee fi fo fum`
> - Output:
>     ```c
>     5
>     argv[0] has the value ./a.out
>     argv[1] has the value fee
>     argv[2] has the value fi
>     argv[3] has the value fo
>     argv[4] has the value fum
>     ```

## A Worked Example

> [!summary]+
> - Use command-line arguments to pass data into a program at runtime
> - Validate user input to avoid errors (e.g., segmentation faults)
> - Convert strings to integers using `strtol`
> - Use type casting to ensure correct division for averages

```c title:sum_or_avg.c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv) {

    // Ensure that the program was called correctly.
    if (argc < 3) {
        printf("Usage: sum_or_avg <operation s/a> <args ...>");
        return 1;
    }

    int total = 0;
    
    int i;
    for (i = 2; i < argc; i++) {
        total += strtol(argv[i], NULL, 10);
    }

    if (argv[1][0] == 'a') {
        // We need to cast total to float before the division.
        // Try removing the cast and see what happens.
        double average = (float) total / (argc - 2);
        printf("average: %f \n", average);
    } else {
        printf("sum: %d\n", total);
    }

    return 0;
}
```

- The program `sum_or_avg` takes a flag (`s` for sum, `a` for average) and one or more integers as arguments
- It calculates either the sum or average of the integers based on the flag

> [!important]+ Input Validation
> - Check `argc` to ensure the program has enough arguments
> - Print a usage message and exit with a non-zero code if arguments are missing

> [!important]+ Calculating the Average
> - Cast the sum to `double` before division to avoid integer division
>     ```c
>     double average = (float) total / (argc - 2);
>     ```

> [!warning]+ Common Mistakes
> - Forgetting to check `argc`, leading to segmentation faults
> - Not casting to `double` before division, resulting in incorrect averages
> - Assuming user input is valid without validation
