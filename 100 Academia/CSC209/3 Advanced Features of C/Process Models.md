---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [exec, fork, wait]
Week: 7
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "12"
Chapter: 
Slides/Notes: 
Date: 2025-02-25
Date created: Tue., Feb. 25, 2025, 3:14:30 pm
Date modified: Mon., Mar. 10, 2025, 10:35:36 pm
---

# Process Models

## Introduction

- **Program**
    - The executable instructions of a program
    - e.g., source code, compiled machine code
- **Process**
    - Running instance of a program
    - Includes machine code and information of the current state of the process (e.g., what instruction to execute next, current values of variables in memory)

![|200](https://i.imgur.com/dIbBU3I.png)

- When a program is loaded into memory in preparation for executing the program:
    - Program code, global variables, stack, and heap all have their own regions of memory
    - Memory layout represents the state of the program
- Stack
    - Tells us which function we are executing and holds current values for any local variables
- Heap and global variable regions
    - Hold current values for other variables in program
- Operating system
    - Keeps track of some additional state for a process

![|400](https://i.imgur.com/J3RFv7m.png)

- Each process:
    - Has a unique identifier or process ID (PID)
    - Associated with a data structure called a **process control block** (PCB)
- **Process control block** ^1c4a37
    - Stores the current values of important registers,
    - Any open file descriptors, and
    - Other state that the OS managers
- Registers
    - Include **stack pointer**, which
        - Identifies the top of the stack
    - Include **program counter**, which
        - Identifies the ==next instruction== to be executed

- Computer is currently running a lot of processes
    - e.g., A process displaying a YouTube video, a process running mail reading, or message program, at least one process for web browser, and many others
- Linux: Run the program `top` to see active processes on machine

![|400](https://i.imgur.com/TGh6RQI.png)

*Running `top` on the server.*

- Can see *user level* processes running
    - Including `top` itself
- Can see *operating systems* processes running owned by ==root==
- `top`
    - Gives information about some of the state information the OS has about a process
- A process:
    - Has a process identifier (PID)
    - Is executing a command or program
        - In far right column
    - $ Process with PID 1 and command `init` is a special OS process

![](https://i.imgur.com/kWT5JHp.png)

- Number of active processes is much ==larger== than number of processes executing instructions at a particular instant of time
- Number of processors — labelled as **CPU**(s) — on the computer
    - Determines how many processes can be executing an instruction at the same time

![|880x478](https://i.imgur.com/rwTYO1c.png)

- If computer has 4 CPUs, then 4 processes can be running simultaneously
    - Processes are in the **running state**
- ? What processes are in the **ready state**?
    - Processes that could be executing if a CPU were available
- ? What processes are in the **blocked** or sleeping state?
    - Processes waiting for an event to occur
    - e.g., Might have made a real call and are waiting for the data to arrive, or
        - Might have explicitly called sleep and are waiting for a timer to expire

> [!info]+ A single process will move from one state to another throughout its lifetime
> - `gcc` may have been running, but then needed to access a file
>     - → `gcc` is moved to the blocked state while it waits for data from file to arrive
> - One data arrives, `gcc` is moved to the ready state
>     - Processor is not available yet
>     - When it is, `gcc` could be executed
> - Once a processor becomes available, `gcc` is executed
>     - → Back in the running state

![](https://i.imgur.com/keFGnJr.png)

> [!note]+ OS gives us the illusion of running dozens or hundreds of processes simultaneously by switching between Ready and Running processes very quickly.

## Creating Processes with Fork

> [!info]+ Creating a new process requires a **system call**
> - OS must set up data structures needed by the new process
>     - Such as the [[#^1c4a37|process control block]]

### `fork`

- ? What happens when a process calls `fork`?
    - Process passes ==control to the OS==
- ? What does the OS do?
    - Makes a copy of the existing process

![|877x392](https://i.imgur.com/u4hM2ro.png)

- ? What happens when OS duplicates a process?
    - OS copies original process’ address space i.e., its data
    - Copies the PCB that keeps track of current state of the process
    - $\therefore$ Newly created process:
        - & Is running the same code
        - & Has the same values for all variables in memory
        - & Has the same value for program counter and stack pointer
        - *Almost* identical
- ! Two important differences between process and a forked process:
    - Newly created process gets a different process id (PID)
    - Return value of `fork` is different in the original process and the newly created process
- **Parent process**
    - Original process
- **Child process**
    - Newly created process as a result of calling `fork` in the original process
    - Has the same value for program counter as original/parent process
    - & When child process runs, it starts executing just after `fork` returns

> [!attention]+ We do not know whether the parent process or child process will execute first
> - When OS is finished creating process, control can be returned to either parent or child

#### Example

Throughout the example, we assign to and print the variable `i` to get a sense of the *flow* through the program.

![|300](https://i.imgur.com/9W0QBg9.png)

- Process with PID 677 starts executing as you would expect
    - Sets `i = 5`
    - Prints `i = 5`
- When it calls `fork`:
    - Is as if time stops while OS does a bunch of work behind the scenes to create the child process

![|300](https://i.imgur.com/t1YqMgc.png)

- Child process is give a *new* process ID 680
- Begins executing at the point when fork returns
- Variable `i` still has the value 5
    - $\because$ New process is a copy of the old one
- Neither the assignment statement nor the `printf` statement are executed by child process

- ? What is the return value of `fork` in the parent process?
    - Process ID of the child
    - e.g., `result == 680`
- ? What is the return value of `fork` in the child process?
    - `0`
    - e.g., `result == 0`
- ? What is the return value of `fork` in the parent process if `fork` *fails*?
    - `-1` in the parent
    - New process is not created
- ? Why might `fork` fail?
    - If there are already too many running processes for the user or across the whole system

- We have two processes ready to execute exactly the same code
- Return value of `fork` allows us to distinguish between the two processes
    - Parent process can follow a different path through code than the child process

In our example:

- Parent process sets `i = i + 2 = 7`
- Child process sets `i = i - 2 = 3`

- ? What is the output if we run the program?
    - `5 7 3` or `5 3 7`
    - Since we do not know whether the parent process or child process will get run on the CPU
        - & OS controls the order that processes run

> [!important]+ The parent and child processes are now completely *separate* processes and *do not share* memory
> In our example:
> - When parent sets `i` to `i + 2`:
>     - Value of `i` in child process does *not* change
>     - Child has its own memory and its own variable `i`

## Process Relationships and Termination

- Programmer cannot control whether parent or child process executes first after parent returns from `fork`
    - & If both child and parent are producing output, the *order* of output may be different ==each time we run== the program

```c title:nowait.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

int main() {
    int result;
    int i, j;

    printf("[%d] Original process (my parent is %d)\n", 
            getpid(), getppid());

    for (i = 0; i < 5; i++) {
        result = fork();

        if (result == -1) {
            perror("fork:");
            exit(1);
        } else if (result == 0) {   //child process
            for (j = 0; j < 5; j++) {
                printf("[%d] Child %d %d\n", getpid(), i, j);
                usleep(100);
            }
            exit(i)
        }
    }

    printf("[%d] Parent about to terminate\n", getpid());
    return 0;
}
```

In this process:

- Original process prints a message that includes its PID and its parents PID
- Creates 5 child processes and then prints a message and terminates

- Each child process executes 5 iterations of a loop
- In loop body:
    - Prints a message, then
    - Sleeps for a short amount of time
- **`usleep`**
    - **System call**
    - Slows processes down enough to see what can happen when they run concurrently
    - Introduces more variability in how processes run
- Every time `usleep` is called:
    - Control is passed to the OS
    - Gives the OS the opportunity to make a *scheduling decision*
        - Decide which process will run next

![|400](https://i.imgur.com/dIn7F9r.png)

- First line of output is “`Original process`”
    - Printed before program calls `fork`
- $ Before all children have printed their first message, the parent prints the “`about to terminate`” message
    - Then more child processes print lines
- $ Can confirm that each child process does print the 5 lines of output that we expect each process to print
    - Number in `[…]` is the process ID
    - All messages printed by one child have the same process ID
- $ Order of output for child processes alone is what you would expect
    - Printing in the correct order of the loop
- $ Print statements for child 2 and child 0 are not interweaved in a predictable way
- ? Why does the parent output come before so much of the child output?

Looking at what the parent process does in the code:

- After printing first message, parent calls `fork` 5 times
- Does not have anything else to do in the body of the outer for loop
- After it completes its 5 iterations:
    - Prints the final message and process terminates

From the operating system’s point of view:

- Parent process is no different than any of the child processes
- OS schedules the parent process to run the same as it does for the child processes
    - e.g., There is no reason for parent to wait for its children to execute

> [!question]+ Why do we see output after the shell prompt appears? Why do we not see a shell prompt when all output has finished?
> - Normally:
>     - When you run a program, shell waits until process has finished, then prints a prompt for next command
>     - That is also happening here
>     - Shell is waiting for original parent process to finish, and then it will print a shell prompt
> - & Shell is just another process that the OS has to schedule
>     - → A few child processes get to print some output before the shell prints its prompt
> - Once shell runs, it prints its message
>     - But some child processes have not finished yet
>     - → Child processes print more output afterwards

- $ We do not see the shell prompt at the end of the output
    - Looks like program has not finished yet, or is “hanging”
- If you hit enter, you will see the prompt again
    - Program has finished
    - Shell was *waiting* our input

### Waiting

> [!question]+ How does the shell know to *wait* for the parent process?
> - The difference is explained by the `wait` system call
> - Program we run from the shell is a *child* of the shell process
> - The shell uses the `wait` system call to suspend itself until its child terminates

- **`wait`**
    - System call
    - Use `wait` to force the parent process to wait until its children have terminated
    - & “Suspends execution of the calling process until one of its children terminates”
        - From `man` page
- % Information about how the child terminated — cleanly or with a particular — is stored in the `int` passed in using the `stat_loc` argument.


![|500](https://i.imgur.com/msJvVC8.png)

- Modified the example code from `nowait.c` that spawned 5 child processes
    - Now: Parent calls `wait`
        - → Will **block** until all 5 child processes have terminated

```c
for (i = 0; i < 5; i++) {
    pid_t pid;
    int status;
    if( (pid = wait(&status)) == -1) {
        perror("wait");
    } else {
        printf("Child %d terminated with %d\n", pid, status);
    }
}
```

- Must call wait once for each child that was created to wait for all of the child processes
- Information in `status` argument is only useful if system call succeeded
    - & Must check `wait`‘s return value before using it

- ? What does `wait` return if successful?
    - Process ID of the child that terminated
- ? What does `wait` return if it fails?
    - `-1`

- Code stores the return value of `wait` into the variable `pid` and checks if value is a real PID or `-1`

![](https://i.imgur.com/lpeRFsG.jpeg)

When we compile and run the program:

- $ Parent does not terminate until all child processes have terminated
- $ Children *may* or *may not* terminate in the same order as they were created
    - In this case, they do

- When a program calls `exit`, as we see in the error checking for `fork`, or calls `return` from main function:
    - We provide an argument like `-1` or `0`
    - Value makes up part of the `status` value in `wait`
- ! This `exit` value enables limit communication from child back to parent
    - Conventionally:
        - Status of 0 represents a *successful* run of the process
        - Non-zero values represent various abnormal terminations
    - ! `exit` value is only part of the value of the status returned by `wait`

Modify our example program so that each child process exits with a different value:

![](https://i.imgur.com/bV28fCL.png)

- Prints the value of `status` after we call `wait`

![|300](https://i.imgur.com/2IUAfyi.jpeg)

> [!obs] `status` is not just the argument to the `exit` function

- & Various bits in the `status` argument are divided up and used for different purposes
- ? What do the lowest 8 bits indicate?
    - Indicates whether the child process terminated normally, or terminated because it received a signal
        - e.g., Ctrl + C
    - If child terminated because of a signal, lowest 8 bits tells us *which* signal
- ? Where is the exit value of the child process located?
    - In the next 8 bits
    - Process that exited with a 1 has `status == 256`
        - $2^{8}$
    - Process that exited with a 2 has `status == 512`
        - $2^{8} \times 2 = 2^{9} = 512$

> [!tip]+ `man` page for `wait` tells us how to *extract* the values we want from the `stat_loc` argument
> - Several macros are provided to help extract data you want

- & Need to add an `#include` so macros are available for use in program
    - `#include <sys/wait.h>`
- `WIFEXITED`
    - Macro to check if process terminated normally
- `WEXITSTATUS`
    - Macro to extract exit value of the process if process terminated normally
- `WIFSIGNALED` and `WTERMSIG`
    - Find out the number of the signal that cause process to terminate if process exits as a result of a *signal*

```c title:wait.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <signal.h>

int main() {
    int result;
    int i, j;

    printf("[%d] Original process (my parent is %d)\n",
            getpid(), getppid());

    for (i = 0; i < 5; i++) {
        result = fork();

        if (result == -1) {
            perror("fork:");
            exit(1);
        } else if (result == 0) { //child process
            for(j = 0; j < 5; j++) {
                printf("[%d] Child %d %d\n", getpid(), i, j);
                usleep(100);
            }

            if(i == 2) {
                abort();
            }
            exit(i);
        }
    }
	sleep(10);
    for (i = 0; i < 5; i++) {
        pid_t pid;
        int status;
        if( (pid = wait(&status)) == -1) {
            perror("wait");
        } else {
            if (WIFEXITED(status)) {
                printf("Child %d terminated with %d\n",
                    pid, WEXITSTATUS(status));
            } else if(WIFSIGNALED(status)){
                printf("Child %d terminated with signal %d\n",
                    pid, WTERMSIG(status));
            } else {
                printf("Shouldn't get here\n");
            }
        }
    }
    printf("[%d] Parent about to terminate\n", getpid());
    return 0;

}
```

- Force child 2 to exit abnormally by calling abort instead of exit
    - So that we can see what happens when we wait for a process that terminates abnormally

![|400x551](https://i.imgur.com/x6GGkd9.jpeg)

- $ Child 2 terminates with a signal
- & Signal for `abort` is 6

> [!question]+ What system call do we use if we want to specify which child process to wait for?
> - **`waitpid`**
>     - System call that lets you specify which child process to wait for
>     - Pass in the process ID of a child process

- `WNOHANG`
    - Can pass in this macro to `waitpid` if parent process just wants to check if a child has terminated, but does not want to block

> [!caution]+ `wait` and `waitpid` can only wait for *child* processes
> - Cannot wait for an unrelated process or even a child of a child process

- `wait` and `waitpid` only give us a *limited* form of synchronization between processes
    - For more power communication between processes, you can use a **pipe** or **signal**

## Zombies and Orphans

The parent-child relationship between processes leads to some interesting issues.  We discussed how a parent process can wait for a child to terminate and noted that a process can only wait for its direct children.

> [!question] What happens when a child process terminates before the parent calls wait?

![|600](https://i.imgur.com/1JQgBUc.png)

In this program:

- Original process calls fork once
- Parent process sleeps for 20 seconds before it calls wait
- Child process prints only one line and then exits
- If we are quick, we can use `top` to see the state of the child process before the parent process calls wait.

![](https://i.imgur.com/EnGWJ74.jpeg)

- State of the child process is given as *Z* for **zombie**
- Process is shown as *defunct*
- Child process has called exit
    - → Child process is dead
    - But not quite
- OS needs to keep exit information of the process somewhere
    - In case parent calls `wait` to get this value
    - → OS cannot delete the process control block of the terminated process until it knows it is safe to clean it up
- **Zombie process**
    - Process that is dead
    - But still hanging around for parent to collect its termination status

> [!question]+ What happens if the parent never calls wait?
> - ? Why do we not have dozens or hundreds of zombie processes using up the process table?
> - ? What happens to a child process when its parent terminates?

- **Orphan**
    - Child process whose parent process terminates first

> [!question] Who is the parent of an orphan process? Does it even have a parent?

Here’s a variant of the program we used in the example about fork.

```c title:orphans.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    int result;
    int i, j;

    printf("[%d] Original process (my parent is %d)\n",
            getpid(), getppid());

    for(i = 0; i < 5; i++) {
        result = fork();

        if(result == -1) {
            perror("fork:");
            exit(1);
        } else if (result == 0) { //child process
            for(j = 0; j < 5; j++) {
                printf("[%d] Child %d %d (parent pid = %d)\n",
                        getpid(), i, j, getppid());
                usleep(100);
            }
            exit(0);
        }
    }
    printf("[%d] Parent about to terminate\n", getpid());
    return 0;

}
```

- Added a call to `getppid` to child processes’ print statement
    - System call that gets the parent process’ ID
- Parent process is *not* waiting after the child processes are created by fork.
- `usleep`
    - Child processes will likely still be active after the parent terminates
    - → Can see what `getppid` reports when child processes are orphans

![|400](https://i.imgur.com/49lJ4n9.jpeg)

When we run the program, we see:

- First statements printed by children have the correct parent process ID
- Shortly after:
    - Parent process prints message that it is about to terminate
    - $ Parent process IDs of all the running children become the value 1
- ? What process has process ID 1?
    - The **init** process
    - First process that the OS launches
    - & When a process becomes an orphan, it is “adopted” by the init process


Now we can come back to the question of when zombie processes go away.

- ? When is a zombie **exorcised** i.e., put to rest?
    - When its termination status has been collected
    - Main task of the init process:
        - Call wait in a loop, which collects the termination status of any process that it has adopted
- After init has collected the termination status of an orphaned process:
    - All of the process’ data structures can be deleted
    - Zombie disappears

## Running Different Programs

> [!goal] Load and execute a different program

### `exec`

- Linux provides the “`exec`” family of functions to replace the currently running process with a different executable.
- While there are several variants on the `exec` function:
    - All perform the same task
    - Differ in how the arguments to the function are interpreted

![](https://i.imgur.com/D0CoGRB.png)

- `do_exec` program:
    - Prints a message then calls `execl`
- ? What are the arguments to `execl`?
    - First argument is a ==path== to an executable
    - Remaining arguments are ==command line arguments== to the executable named in first argument
- ! `execl` requires that we include NULL as the final argument
    - Even though we have no command line arguments in the example

![|647x467](https://i.imgur.com/sQo2WPr.png)

- When process calls `execl`:
    - Control is passed to the OS
    - Code is loaded into the **code region** of the address space
        - Loaded as **machine executable instructions**
        - Not source code, like in the picture
    - Program counter points to the `execl` function
    - Stack pointer points to the main function stack frame
        - Since that is the function we are currently executing

![|500](https://i.imgur.com/cKCc4GX.png)

- When performing `execl`:
    - OS finds the file containing the executable program
    - Loads it into memory where code segment is
    - Also initializes a new stack
    - Program counter and stack pointer are updated
        - → The next instruction to execute when this process returns to user level is the first instruction in the new program

> [!note]+ In our example, the executable that is passed into `execl` is a simple hello world program.
> - Note that we are passing in the name of a compiled program, not source code.

> [!important]+ When control returns to the user level process, the original code that called `exec` is *gone*.
> - It should never return
> ![|568x202](https://i.imgur.com/55Itov2.png)
> - These lines should never execute
> - ! `execl` will return if an error occurs and it was not able to load the program
>     - Still worth checking for errors

- & The OS does not create a new process
    - That is a job for `fork`
- & `exec` asks the OS to *modify* the calling process
    - The process has the *same* process ID after `exec`
    - Retains some state from the original process such as open file descriptors
        - As we will see when we talk about **[[pipe]]**

![](https://i.imgur.com/HC1AZKy.png)

- `exec` is a family of functions
- Used in similar ways
    - But have to pick the one that you need

> [!tip]+ What do these letters stand for?
> - `l`
>     - List
>     - Command line arguments to the program are passed as a list of arguments to the `exec` function
> - `v`
>     - Vector
>     - Command line arguments are passed in as an array of strings
>     - Like with `argv` parameter of main
> - `p`
>     - Path
>     - PATH environment variable is searched to find the executable
>     - ! Without `p`: `execl` and `execv` expect that the first argument is a ==full path to the executable==
> - `e`
>     - Environment
>     - Pass in an array of environment variables and their values
>         - So that the program executes within a specific environment

> [!note] You will find yourself using `execvp` or `execlp` most often.

> [!info]+ Now we can fully explain how the *shell* executes the commands we enter.
> - The shell is just a **process**
>     - Uses `fork` and `exec`
> - When you type a command at a shell prompt:
>     - Shell first calls `fork` to create a new process
>     - Child process calls `exec` to load a different program into the memory of the child process
>     - Typically, shell process then calls `wait`
>         - Blocks until child process finishes executing
> - When `wait` call returns:
>     - Prints a prompt indicating it is ready to receive the next command
