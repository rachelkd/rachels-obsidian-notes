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
  - "6"
Chapter: 
Slides/Notes: 
Date: 2025-01-23
Date created: Thu., Jan. 23, 2025, 3:17:19 pm
Date modified: Fri., Jan. 24, 2025, 9:16:03 pm
---

# Debugger in C

- Compile C program with `-g` flag
    - Allows compiler to collect debugging information

    ```bash
    gcc -Wall -g -o hello hello.c
    ```

- Launch C debugger (gdb)

    ```bash
    gdb ./hello
    ```

- Set up break point

    ```bash
    break line_number
    break func_name
    ```

- Execute C program in gdb debugger

    ```bash
    run [args]
    ```

    - C will execute until the first break point
    - Can use various gdb commands to debug the C program

| Command        | Description                                                   |
| -------------- | ------------------------------------------------------------- |
| `run` or `r`   | Executes the program from start to end.                       |
| `break` or `b` | Sets a breakpoint on a particular line.                       |
| `disable`      | Disables a breakpoint                                         |
| `enable`       | Enables a disabled breakpoint.                                |
| `next` or `n`  | Executes the next line of code without diving into functions. |
| `step`         | Goes to the next instruction, diving into the function.       |
| `list` or `l`  | Displays the code.                                            |
| `print` or `p` | Displays the value of a variable.                             |
| `quit` or `q`  | Exits out of GDB.                                             |
| `clear`        | Clears all breakpoints.                                       |
| `continue`     | Continues normal execution                                    |
