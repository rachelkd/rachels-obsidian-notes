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
Date created: Tue., Feb. 11, 2025, 3:41:37 am
Date modified: Tue., Feb. 11, 2025, 3:48:02 am
---

# System Calls

> [!def]+ System call
> - A function that requests a service from the operating system

- % `void exit(int status)`
    - Request to OS to terminate program
    - When called:
        - OS executes instructions to clean up data structures that represent the running process
        - Terminate the process
- % Low-level calls `read` and `write` are system calls
    - `scanf` and `printf` use `read` and `write` in their implementation
        - Higher-level

```c
#include <stdio.h>
#include <strings.h>

int main() {
    char *str = "Hello World";

    // This should print "The length of Hello World is 11":
    printf("The length of %s is %lu\n", str, strlen(str));
    
    return 0;
}
```

- `printf`
    - Library function that executes several helper functions before calling the right system call
    - ![|490x299](https://i.imgur.com/RIBnM1o.png)
