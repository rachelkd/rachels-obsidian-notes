---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 9
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "16"
Chapter: 
Slides/Notes: 
Date: 2025-03-13
Date created: Wed., Mar. 12, 2025, 1:57:53 pm
Date modified: Thu., Mar. 13, 2025, 12:40:00 am
---

# Signals

## Introduction to Signals

### Example: Dots

```c title:dots.c
#include <stdio.h>

/* Prints dots to standard error. */
int main() {
    int i = 0;

    for (;;) {
        if (( i++ % 50000000) == 0) {
            fprintf(stderr, ".");
        }
    }

    return 0;
}
```

When we run `dots.c`:

- Runs in the foreground
    - → Do not get shell prompt back in terminal window its attached to
- ? How to send a **termination signal** to process?
    - Ctrl+C
- ? How to suspend the process?
    - Ctrl+Z
- ? How to get process to start again?
    - Run `fg` program
    - Sends another signal to process to wake it up

![](https://i.imgur.com/PY0T2o1.png)

### Example: Segs

```c title:segs.c
#include <stdio.h>

/* This program deliberately dereferences a NULL pointer in order
   to cause a segmentation fault. */
int main() {
    int *ptr = NULL;
    *ptr = 3;
    printf("Value at ptr is %d\n", *ptr);
    
    return 0;
}
```

`segs` deliberately deferences a null pointer.

- When you run the program:
    - Dereferences `NULL`
    - Attempts to access an invalid memory location
    - When OS discovers this:
        - & Sends a *signal* to the process to report it
        - Signal: **segmentation fault**
        - → Process terminates

> [!obs]+ There are different types of *signals*.
> - Different actions are taken when signal is delivered to a process

### Definition

> [!def]+ Signal
> - A mechanism that allows a process or OS to interrupt a currently running process
> - Notifies it that an event has occurred

> [!goal]+ Write code so that you can determine what action to take when a particular signal arrives at your process
> - First:
>     - Need to know how signals work

### How Do Signals Work?

![](https://i.imgur.com/m7HhUYX.jpeg)

- Each signal is identified by a number between ==1 and 31==
- **Defined constants**
    - Used to give them names
- $ Each signal is associated with a *default action*
    - Process receiving the signal will normally perform when it receives the signal

<!-- break -->
- ? What signal does Ctrl+C send?
    - Terminal sends the `SIGINT` signal to the process
    - Default action: Process terminates
- ? What signal does Ctrl+Z send?
    - Terminal sends `SIGSTOP` signal to the process
    - Default action: Process suspends execution

> [!question]+ How do we send arbitrary signals to a process?
> - Library function `kill`
> - Command line using program called `kill`

- Before we can send a signal to a process:
    - & Need to know its process id
    - Run `ps`
        - e.g., `ps aux | grep dots`

![](https://i.imgur.com/E6v1AkC.png)

- Number in second column is the process id
    - e.g., `3819`
- Use `kill` to send `SIGSTOP`:
    - `kill -STOP 3819`
- % `SIGSTOP` is signal *17*
    - Could have also directed `kill` to send signal 17 directly

<!-- break -->
- Behaviour:
    - Dots stop printing
    - Like typing Ctrl+Z
    - % Dots process is now suspended

![|300](https://i.imgur.com/smSbWfr.png)

- ? How to get process to run again?
    - Send `SIGCONT` signal
    - `kill -CONT 3819`

![|300](https://i.imgur.com/nOkanca.png)

- ? How to make process terminate?
    - Send `SIGINT` signal
    - `kill -INT 3819`

> [!tip]+ The library function `kill` provides the same functionality ass the `kill` program.
> - Library function is a **C function**
> - & Can send a signal to another process inside C code

> [!note]+ You need to know the PID of the process that you are signalling
> - Most cases:
>     - Use signals to send messages to our children
>     - → Can get PID from return value of `fork`
> - Child process can get parent PID with `getppid`

## Handling Signals

- Signals can be used to notify a process of an event
- Each signal has a default action associated with it
- & Sometimes, we would like to be able to change the default behaviour of the signal
    - e.g., to print a message, save some state, or possibly ignore the signal

> [!info]+ We can write a function to be executed when the signal is received by the process.
> - How does this function get called?
>     - → Have to talk about what happens when a signal is delivered to a process

- **Process control block**
    - Contains a *signal table*
        - Similar to the open file table

![|300](https://i.imgur.com/40JR6wo.png)

- **Signal table**
    - Each entry in the signal table contains a pointer to *code* that will be executed when OS delivers signal to process
    - Called the **signal handling function**

### `sigaction`

> [!tip]+ We can change the behaviour of a signal by installing a new signal handling function.

> [!def]+ `sigaction`
> - System call
> - Modifies the signal table so that our function is called
>     - Instead of the default action
>
> ![](https://i.imgur.com/P33QVYS.png)

- `signum`
    - Number of the signal that we are going to modify
- `act`
    - Pointer to a struct
    - Need to initialize struct before calling `sigaction`
- `oldact`
    - Also pointer to a struct
    - System call fills in the values of the struct with the current state of the signal handler before you change it
    - % Will not be using this argument in our examples, but is sometimes useful to save previous state of signal

#### `sigaction` Struct

```c
struct sigaction {
    void    (*sa_handler)(int);
    void    (*sa_sigaction)(int, siginfo_t *, void *);
    sigset_t  sa_mask;
    int       sa_flags;
    void    (*sa_restorer)(void);
};
```

- `sa_handler`
    - *Function pointer* for the signal handler function that we want to install


We will get to the remaining arguments when we talk about blocking signals.

### Example: Dots

We will add the code to `dots.c` to install the signal handler.

- Need to write a function that will execute when process is signalled
    - i.e., write the **signal handler**

![|400](https://i.imgur.com/aRPdppo.png)

- Can select the name of the function
    - ! But needs to take a single *integer*
- In example:
    - Signal handler will just print a message to `stderr`

Now we need to add code to install our new function in the signal table.

![](https://i.imgur.com/9t98lQy.png)

- Create a struct `sigaction`
    - Set its `sa_handler` member to be the `handler` function we just wrote
    - Use default flags
    - Set `sa_mask` to empty
        - → No signals are blocked during handler
- `newact` `sigaction` struct is all set
    - & Have to call the `sigaction` system call to install the handler for the `SIGINT` signal
        - `sigaction(SIGINT, &newact, NULL);`

We will compile and run our new program with the signal handler installed in this window.

![](https://i.imgur.com/OcHQe4a.png)

- Find the PID of the `dots2` process
    - Then send it a signal
- When we send `INT` signal to process:
    - $ New message is printed to `stderr`
- ! Process does not terminate
    - We did not call `exit` in handler
- When signal handling function finishes:
    - & Control returns back to the process at the point where it was interrupted

> [!question]+ If `SIGINT` is not going to work to stop our process, how are we going to kill it?
> - Send a different termination signal

![](https://i.imgur.com/IODVNX3.png)

- Could go install the same signal handling function for `SIGQUIT` by adding one more call to `sigaction`

![](https://i.imgur.com/Usncmdr.png)

> [!question]- Can we install a function for all the signals that cause the program to terminate, and create an unkillable process?
> - No

> [!note]- `dots2.c` example code
>
> ```c title:dots2.c
> #include <stdio.h>
> #include <signal.h>
> #include <stdlib.h>
> #include <unistd.h>
> 
> /* A signal handling function that simply prints
>    a message to standard error. */
> void handler(int code) {
>     fprintf(stderr, "Signal %d caught\n", code);
> }
> 
> int main() {
>     // Declare a struct to be used by the sigaction function:
>     struct sigaction newact;
> 
>     // Specify that we want the handler function to handle the
>     // signal:
>     newact.sa_handler = handler;
> 
>     // Use default flags:
>     newact.sa_flags = 0;
> 
>     // Specify that we don't want any signals to be blocked during
>     // execution of handler:
>     sigemptyset(&newact.sa_mask);
> 
>     // Modify the signal table so that handler is called when
>     // signal SIGINT is received:
>     sigaction(SIGINT, &newact, NULL);
> 
>     // Keep the program executing long enough for users to send
>     // a signal:
>     int i = 0;
>     
>     for (;;) {
>         if ((i++ % 50000000) == 0) {
>             fprintf(stderr, ".");
>         }
>     }
> 
>     return 0;
> }
> ```

### You Cannot Change `SIGKILL` and `SIGSTOP` Signals

> [!important]+ Two signals that you cannot change
> - `SIGKILL`
> - `SIGSTOP`
>
> ![](https://i.imgur.com/HNCKRTT.png)
