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
Date created: Tue., Feb. 11, 2025, 3:48:01 am
Date modified: Mon., Mar. 10, 2025, 4:00:05 pm
---

# Errors and Errno

## When System Calls Fail

- Since system calls interact with resources outside of running program, it is possible for them to fail
- Reasons for fail may not be something program itself has any control over

```c title:show_fopen.c
#include <stdio.h>
#include <stdlib.h>

#define MAX_LINE_LENGTH 80

int main(int argc, char **argv) {
    FILE *fp;
    char s[MAX_LINE_LENGTH + 1];

    fp = fopen(argv[1], "r");
    
    fgets(s, MAX_LINE_LENGTH, fp);

    printf("One line from the file: %s", s);

    return 0;
}
```

- `fopen` to open a file, where file name is given as command-line argument
    - File may not exist, or has wrong permissions
    - & It is a programmer’s responsibility to check if a file exists before using its results
        - e.g., If you open a file that does not exist, it might lead to a **segmentation fault**
            - Does not help find what the issue is!
- & This is particularly true for system and library calls
    - If you do not check, strange things can happen later in program without much indication why

### Examples of System Calls

```c
FILE *fopen(const char *path, const char *mode);
```

- `fopen`
    - Returns a FILE pointer used for later operations on the file
    - If it cannot open the file, it returns `NULL`

```c
void *malloc(size_t size);
```

- `malloc`
    - Returns a pointer to a region in memory it has allocated
    - If it cannot allocate enough memory, it returns `NULL`
- Technically not a system call; it is a system library call
    - But uses system calls to allocate memory, and checks those calls and returns appropriate error

```c
int stat(const char *path, struct stat *buf)
```

- `stat`
    - Finds information about a file, pointed to by a path
    - Stores information in `buf`
    - Returns:
        - `0` if it succeeds
        - `-1` if it encounters an error

> [!obs]+ The main way to indicate an error occurred is to return a special value.
> Nearly all system calls follow the same pattern.
>
> - System calls that return `int` return `-1` to indicate an error occurred
> - System calls that return a pointer return `NULL` to indicate an error occurred

- We have explicitly checked for errors when we had system calls
- ! However, we also want to know *why* the error occurred to print a sensible message to the user
    - Return value itself cannot be used to indicate an error type

### `errno`

> [!def]+ `errno`
> - Global variable of type `int`
> - Used to store type of the error
> - Header file included with system defines different numeric code for different types of errors

![](https://i.imgur.com/N7mUBqJ.jpeg)

- [p] You do not need to remember the numeric codes for error types or named constants
    - Library provides a few functions that will map error code to a string that explains the error

### `perror`

```c
void perror(const char *s);
```

> [!def]+ `perror`
> - Prints an error to `stderr`
> - Message includes argument `s`, followed by `:` and then the error message that corresponds to the current value of `errno`

- Useful for when a system call has failed
- ! Do not use `perror` as a generic error message reporting function
    - Real purpose is to display an error message for the current value of `errno`
- For other error messages, you should still use `fprintf` to `stderr`

### Example: `perror` vs. `fprintf`

```c title:show_fopen.c
#include <stdio.h>
#include <stdlib.h>

#define MAX_LINE_LENGTH 80

int main(int argc, char **argv) {
    FILE *fp;
    char s[MAX_LINE_LENGTH + 1];

    if ((fp = fopen(argv[1], "r")) == NULL) {
        perror("fopen");
        exit(1);
    }

    if (fgets(s, MAX_LINE_LENGTH, fp) == NULL) {
        fprintf(stderr, "no line could be read from the file\n");
        exit(1);
    }

    printf("One line from the file: %s", s);

    return 0;
}
```

- Add error checking to `fopen`
    - Use `perror` since `fopen` will set `errno` if it fails
    - Add call to `exit` (instead of `return`)
        - Since we want program to terminate
- Good habit to use `exit`
    - Errors you detect in other functions will not terminate if you simply `return` rather than invoking `exit`
- Use `fprintf` for `fgets`
    - If it cannot read any text from file, it does not set `errno`
    - Simply signals `EOF` (end of file)

```c title:show_malloc.c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

int main(int argc, char **argv) {
    // Call malloc with a size that's too large to allocate
    // memory for:
    char *name = malloc(LONG_MAX);

    if (name == NULL) {
    	perror("malloc");  // prints an error message to stderr
    	exit(1);
    }

    return 0;
}
```

```c title:get_size.c
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>

int main(int argc, char **argv) {
    struct stat sbuf;

    if (stat(argv[1], &sbuf) == -1) {
	    perror("stat");
        exit(1);
    } else {
	    printf("Size of %s is %lld\n", argv[1], sbuf.st_size);
    }

    return 0;
}
```

- If you run `get_size.c` with an invalid file, then you will see the output:
    - `stat: No such file or directory`

```c title:"Bad example of get_size.c"
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/types.h>

int main(int argc, char **argv) {
    struct stat sbuf;

    stat(argv[1], &sbuf);
    printf("Size of %s is %lld\n", argv[1], sbuf.st_size);

    return 0;
}
```

- If `stat` fails, then the values in `sbuf` are unknown
    - → Cannot trust result

## Checking for Errors

- Error checking is essential for:
    - Providing useful feedback to users
    - Discovering bugs quickly
    - Preventing undefined behavior (e.g., segmentation faults)
- Common error checks:
    - Validate command-line arguments
    - Check return values of system calls (e.g., `fopen`, `fgets`)
    - Handle edge cases (e.g., invalid input, file errors)

> [!info]+ Why Error Checking Matters
> - Prevents crashes and undefined behavior
> - Improves user experience with meaningful error messages
> - Makes debugging easier

### Example: A Simple `head` Program

- Prints the first `N` lines of a file
- Takes two command-line arguments:
    - `NUM`: Number of lines to print
    - `FILE`: Name of the file to read

> [!example]+ Example: Basic Program (No Error Checking)
>
> ```c
> #include <stdio.h>
> #include <stdlib.h>
>
> #define BUFSIZE 256
>
> int main(int argc, char **argv) {
>     FILE *fp;
>     char buf[BUFSIZE];
>     int i;
>     
>     long numlines = strtol(argv[1], NULL, 0); // Convert string to long
>     fp = fopen(argv[2], "r"); // Open file
>     
>     for (i = 0; i < numlines; i++) {
>         fgets(buf, BUFSIZE, fp); // Read line
>         printf("%s", buf); // Print line
>     }
>     
>     return 0;
> }
> ```

> [!example]+ Basic Program (With Error Checking)
>
> ```c
> #include <stdio.h>
> #include <string.h>
> #include <sys/types.h>
> #include <stdlib.h>
> #include <limits.h>
> 
> #define BUFSIZE 1024
> 
> /* This program takes two command line arguments: a number NUM and a 
>  * filename FILE. It prints NUM lines from the file FILE.
>  * This version has error checking.
> */
> 
> int main(int argc, char **argv) {
>     FILE *fp;
>     char buf[BUFSIZE];
>     int i;
>     
>     if (argc != 3) {
>         fprintf(stderr, "Usage: %s NUM FILE\n", argv[0]);
>         exit(1);
>     }
>     
>     long numlines = strtol(argv[1], NULL, 0);
>     
>     if (numlines <= 0) {
>         fprintf(stderr, "ERROR: number of lines should be positive.");
>         exit(1);
>     }
>     
>     if ((fp = fopen(argv[2], "r")) == NULL) {
>         perror("fopen");
>         exit(1);
>     }
>     
>     for (i = 0; i < numlines; i++)  {
>         if ((fgets(buf, BUFSIZE, fp)) == NULL) {
>             fprintf(stderr, "ERROR: not enough lines in the file\n");
>             exit(1);
>         }
> 
>     	while (strchr(buf, '\n') == NULL) {
>             if ((fgets(buf, BUFSIZE, fp)) == NULL) {
>                 fprintf(stderr, "ERROR: not enough lines in the file\n");
>                 exit(1);
>             }
>             printf("%s", buf);
>     	}
> 
>         printf("%s", buf);
>     }
>     
>     return 0;
> }
> ```

### Common Errors and Fixes

> [!warning]+ Missing Arguments
> - No arguments: Causes segmentation fault
> - Fix: Check `argc` before accessing `argv`
> ```c
> if (argc != 3) {
>     fprintf(stderr, "Usage: %s NUM FILE\n", argv[0]);
>     exit(1);
> }
> ```

> [!warning]+ Invalid File Name
> - `fopen` fails if the file doesn’t exist or can’t be opened
> - Fix: Check `fopen` return value and use `perror`
> ```c
> if ((fp = fopen(argv[2], "r")) == NULL) {
>     perror("fopen");
>     exit(1);
> }
> ```

> [!warning]+ Invalid Number of Lines
> - Non-numeric or negative values for `NUM`
> - Fix: Validate `strtol` result
> ```c
> long numlines = strtol(argv[1], NULL, 0);
> if (numlines <= 0) {
>     fprintf(stderr, "ERROR: number of lines should be positive.\n");
>     exit(1);
> }
> ```

> [!warning]+ End of File (EOF) Handling
> - `fgets` returns `NULL` if EOF is reached before reading `NUM` lines
> - Fix: Check `fgets` return value
> ```c
> if (fgets(buf, BUFSIZE, fp) == NULL) {
>     fprintf(stderr, "ERROR: not enough lines in the file\n");
>     exit(1);
> }
> ```

> [!warning]+ Handling Long Lines
> - Lines longer than `BUFSIZE` are truncated
> - Fix: Use a loop to read until a newline is found
> ```c
> while (strchr(buf, '\n') == NULL) {
>     if (fgets(buf, BUFSIZE, fp) == NULL) {
>         fprintf(stderr, "ERROR: not enough lines in the file\n");
>         exit(1);
>     }
>     printf("%s", buf);
> }
> ```
