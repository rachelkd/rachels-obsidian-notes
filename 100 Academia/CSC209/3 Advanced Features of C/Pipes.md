---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: [dup2, Pipes]
Week: 8
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "14"
  - "15"
Chapter: 
Slides/Notes: 
Date: 2025-03-04
Date created: Fri., Feb. 14, 2025, 2:33:31 pm
Date modified: Sun., Mar. 9, 2025, 8:37:46 pm
---

# Pipes

## Unbuffered I/O

> [!summary]+ In these modules so far, the only I/O operations we have been using are those that operate on *streams* using the `FILE` objects.
> - Functions include:
>     - `fopen`
>     - `fclose`
>     - `printf`
>     - `fgets`
>     - `fwrite`
>     - and so on

- These functions are great to use with files because they hide some of the complexity of the actual IO system calls.
    - Particularly: they *buffer* data
    - i.e., May read or write larger blocks of data than the user has specified
- Collecting data into larger blocks:
    - & Allows file system to decide exactly when to send data to the disk, network, or screen
    - & Amortize the cost of data transfer
        - By reducing number of system calls made
- Delegating these tasks allows programmer, in most cases, to ignore the performance details of data transfer

```c title:low_level.c
#include <stdio.h>
#include <stdlib.h>

int main() {

    FILE *outfp = fopen("tmpfile", "w");

    if(outfp == NULL) {
        perror("fopen");
        exit(1);
    }

    fprintf(outfp, "This is ");
    fprintf(outfp, "one of several ");
    fprintf(outfp, "calls to fprintf.\n");
    fprintf(outfp, "How many write ");
    fprintf(outfp, "system calls are generated?\n");
    fclose(outfp);
    return 0;
}
```

- Simple program that opens a file and makes a few `fprintf` calls to write some text to the file
- `fopen` returns a `FILE` struct
- `FILE` struct holds all the data that file system code needs to implement buffering
- Program contains five separate `fprintf` calls

> [!question] How many `write` system calls are made?

- In order for data to be written to disk:
    - System call like `write` must happen somewhere in the library code
    - Linux:
        - Use `strace` program to run our program and see which system calls are made
        - Allows us to determine how many `write` calls are generated

```
gcc -Wall -o low_level low_level.c
strace low_level
```

![](https://i.imgur.com/T35K65l.jpeg)

- `strace` spits out a lot of output
    - Even a simple program makes a number of system calls
- $ Only a single `write` call is generated
    - Call writes 84 bytes
    - Length of the string produced by all five of our `printf` calls
- ? What is the value `3` in the system call?
    - **File descriptor**
        - Integer that represents an open file or open communication channel
        - Type `int`
            - Because OS uses them as indexes into a table of open files
- ? Why `3` in particular?
    - Three files are automatically opened for you
        - `stdin`, `stdout`, `stderr`

### Running `low_level` in Debugger

![](https://i.imgur.com/i1dQfHb.jpeg)


- & These global variables are actually of type `FILE *`
- If we display the contents the `FILE` struct, you can see the file descriptor
    - `stdin` has file descriptor 0
    - `stdout` has file descriptor 1
    - `stderr` has file descriptor 2

> [!tip]+ Knowing the value of a file descriptor is sometimes useful for debugging
> - In general:
>     - Should not have to look at value of the file descriptor

```c
// Open a file using a FILE object
FILE *fp = fopen("file1.txt", "r");

// Open a file using a file descriptor
int filedes = open("file1", O_RDONLY);
```

- Think of value returned by `fopen` (or the low-level open system call) as a name that is required when performing I/O

- **Pipes**
    - A form of interprocess communication that also uses *file descriptors*

```c
// Create a pipe
int fd[2];  // Array of two file descriptors
pipe(fd);

// Read 256 bytes from file
read(filedes, buf, 256);

// Read 256 bytes from pipe
read(fd[0], buf, 256);
```

- Although `open` is used to open a file, and pipe is used to set up a communication channel:
    - Both operate on file descriptors
    - Convenient
        - Same `read` and `write` system calls can be used both for files and pipe

> [!question]- Follow-up Question 1
> - A file descriptor is one field in a `FILE` struct
> - The global variable `stdin` is of type `FILE *`
> - The file descriptor for standard output has the value 1

## Introduction to Pipes

- The [[Process Models|fork]] system call gives us the ability to create multiple processes
- If we can divide up a problem so that multiple processes can work on it simultaneously:
    - Should be able to solve these problems more quickly
    - Can take advantage of machines with multiple processors
- But to cooperate to solve a problem:
    - Processes need to *communicate*

- **Pipes**
    - One type of communication mechanism
    - Can be used to ==send data between related processes==
- Pipe is specified by an array of two file descriptors
    - One for *reading* data from the pipe
    - One for *writing* data to the pipe


When the program calls the pipe system call:

![|400](https://i.imgur.com/Gp1n3sS.png)

- & OS creates the **pipe data structures**
    - Opens two file descriptors on the pipe
        - One for reading
        - One for writing
- Both file descriptors are stored in `fd`
    - Array of two `int`s that we pass into `pipe`

<!-- break -->
- ? What is the 0-th element of `fd`?
    - File descriptor used for *reading* from the pipe
- ? What is the `int` at index 1?
    - File descriptor used for *writing* to the pipe

<!-- break -->
- After `pipe` system call returns:
    - Process can now read and write on the pipe
    - Not very useful!
        - ! Process does not need a pipe to communicate with itself

> [!tip]+ However, we can now take advantage of a very useful aspect of `fork`.
> - When fork makes a copy of the existing process:
>     - Also makes a copy of all open file descriptors
> - & Child process inherits all open files descriptors

After the `fork` call, the picture looks like this:

![](https://i.imgur.com/fJKFYTg.png)

- $ Both processes have the read and write file descriptors on the pipe open

- Pipes are **uni-directional**
    - One process will write to the pipe
    - Another process will read from the pipe
    - Parent can write to the pipe and child can read from it, OR
    - Child can write to the pipe and parent can read from it

> [!important]+ Once you have decided which direction the data should flow, you need to close the file descriptors that you won’t be using.
> - e.g., If the parent will read data from the child:
>     - Close the write file descriptor `fd[1]` in the parent
>     - Close read file descriptor `fd[0]` in the child

![](https://i.imgur.com/0wnoJpT.png)

> [!tip]+ We can use `read` and `write` system calls to send and receive data on the pipe.

```c title:show_pipe.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

#define MAXSIZE 4096

/* A program to illustrate how pipe works.  The parent process reads
 * from stdin in a loop and writes each line it reads to the pipe.  The
 * child process reads from the pipe and prints each line is reads to
 * standard output.  Extra print statements are included so you can see
 * what each process is doing.
 *
 * Notes:
 *  - to close standard input from the shell type ctrl-D
 *  - In this example, we are writing the same number of bytes each time
 *    and reading the same number of bytes each time from the client.
 *    This simplifies the client side.
*/

int main() {
    char line[MAXSIZE];
    int fd[2];
    // create a pipe
    if (pipe(fd) == -1) {
        perror("pipe");
    }

    int r = fork();

    if (r > 0) {
        // Parent reads from stdin, and writes to child
        // close the read file descriptor since parent will write to pipe
        close(fd[0]);

        printf("Enter a line > ");
        while (fgets(line, MAXSIZE, stdin) != NULL) {

            printf("[%d] writing to pipe\n", getpid());
            if (write(fd[1], line, MAXSIZE) == -1) {
                perror("write to pipe");
            }

            printf("[%d] finished writing\n", getpid());
            printf("Enter a line > ");
        }

        close(fd[1]);
        printf("[%d] stdin has been closed, waiting for child\n", getpid());

        int status;
        if (wait(&status) != -1)  {
            if (WIFEXITED(status)) {
                printf("[%d] Child exited with %d\n", getpid(),
                        WEXITSTATUS(status));
            } else {
                printf("[%d] Child exited abnormally\n", getpid());
            }
        }

    } else if (r == 0) {
        close(fd[1]);
        printf("[%d] child\n", getpid());
        // Child will read from parent
        char other[MAXSIZE];

        while (read(fd[0], other, MAXSIZE) > 0) {
            printf("[%d] child received %s", getpid(), other);
        }
        printf("[%d] child finished reading", getpid());
        close(fd[0]);
        exit(0);

    } else {
        perror("fork");
        exit(1);
    }

    return 0;
}
```

- Simple example that demonstrates that we can send text from the parent process to the child
    - Includes *most* of the error checking required
        - Should really be checking `close`, too

<!-- break -->
- Parent uses `fgets` to read one line at a time from standard input and places each line in the pipe for its child to read
- Parent closes the read end of the pipe
    - `close(fd[0])`
- Child closes the write end of the pip
    - `close(fd[1])`

Now we’re set to send data between the two processes.

- `if (write(fd[1], line, MAXSIZE) == -1)`
    - Parent writes the full character array of 256 bytes to the pipe
    - Somewhat wasteful because we are writing more data than we really need to
    - On the other hand:
        - Simplifies the child process
        - Because it knows exactly how many bytes to expect
        - Doesn’t have to worry about whether the strings are null terminated

- `write` command is wrapped in a loop
    - → One line is written at a time
- `fgets` will return `NULL` if file is closed
    - → Loop will exit when `stdin` is closed
    - Can close `stdin` from shell by typing Ctrl+D

- After the loop terminates:
    - & Close the write file descriptor for the pipe
    - This is important:
        - When all the write file descriptors on the pipe are closed, a read on the pipe will return 0
        - Indicating that there is no more data to read from the pipe
- `while (read(fd[0], other, MAXSIZE) > 0)`
    - & Child process uses this to detect when the parent is finished writing lines to the pipe
    - Loop terminates when the parent closes the write end of its file descriptor
    - & While loop will also terminate if an error occurs and read returns -1

Inside the loop, the child reads data from the pipe and displays it on the screen.

- % Child process uses a different character array
    - Same size as the one used in parent process
    - `char other[MAXSIZE]`
    - Did not reuse the `line` variable to remind you that memory is *not shared* between processes

When the child has finished reading all the data from the pipe:

- Close its read file descriptor for the pipe, and
- Exit

> [!note]+ When a process exits, all of its open file descriptors are closed and all memory is freed, so it isn’t strictly speaking necessary to close file descriptors before exiting.
> - But good habit to get into
>     - Ensures you are thinking about which file descriptors are open
> - In a long running program:
>     - Important to close descriptors that are no longer needed
>         - Because number of open file descriptors is limited
>     - Possible to run out.

> [!question]- Follow-up Question 2
> - The `pipe` call must be made before the fork call because the child inherits open file descriptors from the parent.
> - A read call returns 0 when the write ends of the pipe are closed and all the data has been read from the pipe.

## Concurrency and Pipes

- Pipes are used to communicate between two independent running processes
- The operating system decides when these processes run
    - → Possible — even likely — that they won’t run in lock step with each other.
    - Example of the more general Producer/consumer problem

- **Producer/consumer problem**
    - One process is producing or writing date
    - Other process is consuming or reading data
    - & Connected by a queue

![](https://i.imgur.com/5ZjD48V.png)

- & Producer and consumer may not work at the same rate

<!-- break -->
- If producer is running and consumer is not (or if consumer is not running enough to keep up):
    - & Data can pile up

![](https://i.imgur.com/L7pQZzv.png)

- ! Problem if we have limited buffer space in the queue
    - Do not want producer to try to put data into a full queue

On the other hand:

- If the consumer is running but the producer is not:
    - & Consumer will not have data to operate on
    - Need to make sure that the consumer doesn’t try to remove data from an empty queue
        - Or from a queue where the producer is just starting to add data

> [!summary]+ Three cases we are worried about
> 1. Producer adds to the queue when it is full
> 2. Consumer removes from the queue when it is empty
> 3. Producer and consumer operate on queue simultaneously

### Case 3: Producer and Consumer Operate on Queue Simultaneously

- Pipe is a queue data structure in the OS
- Process *writing* to the pipe is the *producer*
- Process *reading* from the pipe is a *consumer*
- Since OS manages the pipe data structure:
    - & OS ensures that only one process is modifying it at a time
    - Eliminates the case where a consumer removes data as a producer is writing it

### Case 2: Producer Takes Longer to Write Data to the Pipe than the Consumer Can Read it

Consider what happens if the producer takes longer to write data to the pipe than the consumer can read it

- Consumer process will call `read` on pipe when it contains no data
- OS helps us out in this case:
    - & `read` call will *block* if pipe is empty
- Eliminates case where consumer removes from queue when it is empty

#### Example from Previous Video

- Parent process reads from `stdin` and write to the pipe
- Child process has very little work to do
    - Just reads from pipe and writes message to `stdout`
- Parent process has to *wait* for user to type something
    - → Child process spends quite a bit of its time blocked
        - Waiting for parent to write data to pipe

### Case 1: When the Producer Operates More Quickly than the Consumer

- Pipe will eventually fill up
- To prevent data from being lost:
    - OS will cause a `write` to the pipe to block until there is space in the pipe

- Can watch this in action by putting a `sleep` statement in the child to slow down its reading
    - → Parent process can write multiple times before the child reads

![](https://i.imgur.com/9DsIjGd.jpeg)

- $ Parent process eventually has to wait until the child reads each chunk of data from the pipe before it can write its next piece of data

> [!question]- Follow-up Question 3
> - ? Who ensures that one process cannot be reading from the pipe at the same time as another process is writing to the pipe?
>     - Operating system
> - A `write` call will block if the pipe is full

```c title:show_pipe2.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

#define MAXSIZE 8192

/* A program to illustrate how pipe works.  The parent process reads
 * from stdin in a loop and writes each line it reads to the pipe.  The
 * child process reads from the pipe and prints each line is reads to
 * standard output.  Extra print statements are included so you can see
 * what each process is doing.
 *
 * Notes:
 *  - to close standard input from the shell type ctrl-D
 *  - You will save yourself some grief if you read the same number of bytes
 *  from the pipe as were written to the pipe. This is especially important
 *  if you are working with binary data.
*/

int main() {
    char line[MAXSIZE];
    int fd[2];
    // create a pipe
    if (pipe(fd) == -1) {
        perror("pipe");
    }

    int r = fork();

    if (r > 0) {
        // Parent reads from stdin, and writes to child
        // close the read file descriptor since parent will write to pipe
        close(fd[0]);

        printf("Enter a line > ");
        while (fgets(line, MAXSIZE, stdin) != NULL) {

            printf("[%d] writing to pipe\n", getpid());
            if (write(fd[1], line, MAXSIZE) == -1) {
                perror("write to pipe");
            }

            printf("[%d] finished writing\n", getpid());
            printf("Enter a line > ");
        }

        close(fd[1]);
        printf("[%d] stdin has been closed, waiting for child\n", getpid());

        int status;
        if (wait(&status) != -1)  {
            if (WIFEXITED(status)) {
                printf("[%d] Child exited with %d\n", getpid(),
                        WEXITSTATUS(status));
            } else {
                printf("[%d] Child exited abnormally\n", getpid());
            }
        }

    } else if (r == 0) {
        close(fd[1]);
        printf("[%d] child\n", getpid());
        // Child will read from parent
        char other[MAXSIZE];

        while (read(fd[0], other, MAXSIZE) > 0) {
            printf("[%d] child received %s", getpid(), other);
            sleep(2);
        }
        printf("[%d] child finished reading", getpid());
        close(fd[0]);
        exit(0);

    } else {
        perror("fork");
        exit(1);
    }

    return 0;
}
```

## Redirecting Input and Output with Dup2

> [!goal]+ Redirect input or output from one file descriptor to another using the `dup2` system call

Consider the shell’s redirection operators:

- Many useful shell programs read their input from standard input and write their output to standard output
    - e.g., Use `grep` to search for lines in a file that contain the phrase `L0101` and print the output to standard output
        - `grep L0101 student_list.txt`
- ! But printing output to the screen only lets us read it
- One of the things that makes the shell so useful as a command-line interface is:
    - & Ability to use *redirection* operators to send the output to different places without modifying a program

### Output Redirection Operator

- **Output redirection** operator
    - Lets us save the output to a file
    - e.g., `grep L0101 student_list.txt > day.txt`

> [!tip] Redirection also lets us combine programs to perform complex tasks.

### Pipe Operator

- **Pipe** operator
    - Use the output of a program into the input to another program
    - e.g., `grep L0101 student_list.txt | wc`
        - Use output of `grep` as input to `wc` program

```bash
> grep L0101 student_list.txt | wc
    11    44    418
>
```

### How Does Output Redirection Work?

Think about the shell as a process:

```bash
> grep L0101 student_list.txt > day.txt
```

- `grep` is going to send its output to `stdout`
- But output redirection operator indicates we want output to go to a file
    - Rather than screen
- Do not want to modify `grep` to understand this operator
- Instead:
    - @ Need a way to change what standard output means
        - So when `grep` program writes to standard output, it actually goes to the file
    - System call that we need is `dup2`

#### `dup2`

- **`dup2`**
    - System call
    - Makes a copy of an open file descriptor
    - Can use it to reset the `stdout` file descriptor
        - → Writes to `stdout` will go to our output file

![](https://i.imgur.com/BsjeMx8.jpeg)

#### Example: `grep student_list.txt > day.txt`

![](https://i.imgur.com/i5jGTme.png)

- A file descriptor is really an index into a table
- Each process has its own set of file descriptors
    - & → Each process has its own file descriptor table
- ? Where is the file descriptor table stored?
    - **Process control block**
- **File descriptor table**
    - Contains pointers to data structures that contain information about open files
    - e.g., 0 index into the file descriptor table usually contains a link to the console

![](https://i.imgur.com/MkKLgDQ.png)

- For the shell to execute a program:
    - First needs to call `fork` to create a new process
- When a child process is created:
    - Obtains a copy of the file descriptor table from its parent
- $ Pointers in both file descriptor tables point to the *same* object
    - Even though file descriptor tables are separate
- & File objects are shared
    - Like the console in this figure
    - & Changes to the console will be observed by both processes

<!-- break -->
- When the child runs:
    - Opens the file that will receive the output of the program

![](https://i.imgur.com/7nqdQHO.png)

- We have opened the file with *write* permissions
    - Child can receive output

Now we can redirect standard output using `dup2`.

![|986x527](https://i.imgur.com/7zL8Exf.png)

- Extract the file descriptor for `stdout` using `fileno` function
    - ? What type is `stdout`?
        - A global variable of type `FILE *`

After the `dup2` call:

- File descriptor 2 points to the newly opened file
    - Rather than the console
    - & → When the process writes to `stdout` the output will be written to the file, rather than the console

> [!tip]+ It is a good idea to close the file descriptors that we are not using
>
> ```c
> close(filefd);
> ```
> - Close `filefd` because we will not be writing to the file except as `stdout`

- & All these steps happen after the fork but before calling `exec`.
- Now, when the child process calls `exec`:
    - All the file descriptors that were open before the `exec` call are retained
    - → The shell program is running `grep`, but writes to `stdout` go to a *file*, rather than the console

```c title:redirect.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>

/* Demonstrate how file redirection is implemented using dup2.  */

int main() {
    int result;

    result = fork();
    
	/* The child process will call exec  */
	if (result == 0) {
        //int filefd = open("day.txt", O_RDWR | S_IRWXU | O_TRUNC);
        int filefd = open("day.txt", O_RDWR | O_CREAT | O_TRUNC, S_IRWXU);
		if (filefd == -1) {
			perror("open");
		}
        if (dup2(filefd, fileno(stdout)) == -1) {
			perror("dup2");
		}
		close(filefd);
		execlp("grep", "grep", "L0101", "student_list.txt", NULL);
		perror("exec");
		exit(1);

	} else if (result > 0) {
	    int status;
		printf("HERE\n");
		if (wait(&status) != -1) {
		    if (WIFEXITED(status)) {
			    fprintf(stderr, "Process exited with %d\n", 
				        WEXITSTATUS(status));
			} else {
			    fprintf(stderr, "Process teminated\n");
			}
		}
	   
	} else {
	    perror("fork");
		exit(1);
	}
   return 0;
}
```

```txt title:"student_list.txt (Sample input)"
g5hopper  Hopper  Grace         L0101
g4bartik  Bartik  Jean          L5101
g5perlma  Perlman  Radia        L0101
g5allenf  Allen  Frances        L0101
g5liskov  Liskov  Barbara       L5101
g4tardos  Tardos Eva            L0101
g5goldwa  Goldwasser  Shafi     L5101
g4klawem  Klawe  Maria          L5101
g5wingje  Wing  Jeanette        L0101
g4borgan  Borg  Anita           L0101
g4worsel  Worsley  Beatrice     L5101
g4mcnult  McNulty  Kay          L0101
g5snyder  Snyder  Betty         L5101
g5wescof  Wescoff  Marlyn       L0101
g5lichte  Lichterman  Ruth      L5101
g4jennin  Jennings  Betty       L0101
g5bilasf  Bilas  Fran           L0101
g5schnei  Schneider  Erna       L5101
g4sammet  Sammet  Jean          L5101
g5estrin  Estrin  Themla        L0101
g4little  Little  Joyce Currie  L1010
g5keller  Keller  Mary Kenneth  L5101
```

> [!question]- Follow-up Question 4
> - `dup2` is used when you want to reset standard input for a process to read from a file.
> - `dup2` takes two indices in the file descriptor table and resets one to refer to the same file object as the other.

## Implementing the Shell Pipe Operator

> [!goal] Walk through an example that uses pipe and dup2 to implement the shell’s pipe operator

- Shell **pipe** operator
    - Allows us to connect two processes
    - → Can make standard output from one process the standard input for another process

```bash
> cat file1.txt
Pass
Fail T1
Fail T2
Pass
Pass
Fail T5
```

The example that we will use for the remainder of the video uses the `sort` and `uniq` programs.

- **`sort`**
    - Sorts its input and writes it to standard output

```bash
> sort < file1.txt
Fail T1
Fail T2
Fail T5
Pass
Pass
Pass
> sort file1.txt
Fail T1
Fail T2
Fail T5
Pass
Pass
Pass
```

- $ `sort` can operate on either standard input or files passed in as command-line arguments
    - We’ll focus on the use of standard input and output

- **`uniq`**
    - Only emits lines that are different from the line that precedes them

```bash
> uniq < file1.txt
Pass
Fail T1
Fail T2
Pass
Fail T5
```

- $ “Pass” is displayed *once* between T2 and T5
    - Second one was cut

> [!goal] Chain these programs so that only unique messages are printed — regardless of their order in original file

```c title:shpipe.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <sys/wait.h>

/* equivalent to sort < file1 | uniq */

int main() {
    int fd[2], r;

    /* Create the pipe */
    if ((pipe(fd)) == -1) {
        perror("pipe");
        exit(1);
    }

    if ((r = fork()) > 0) { // parent will run sort
        // Set up the redirection from file1 to stdin
        int filedes = open("file1", O_RDONLY);

        // Reset stdin so that when we read from stdin it comes from the file
        if ((dup2(filedes, fileno(stdin))) == -1) {
            perror("dup2");
            exit(1);
        }
        // Reset stdout so that when we write to stdout it goes to the pipe
        if ((dup2(fd[1], fileno(stdout))) == -1) {
            perror("dup2");
            exit(1);
        }

        // Parent won't be reading from pipe
        if ((close(fd[0])) == -1) {
            perror("close");
        }

        // Because writes go to stdout, noone should write to fd[1]
        if ((close(fd[1])) == -1) {
            perror("close");
        }

        // We won't be using filedes directly, so close it.
        if ((close(filedes)) == -1) {
            perror("close");
        }

        execl("/usr/bin/sort", "sort", (char *) 0);
        fprintf(stderr, "ERROR: exec should not return \n");

    } else if (r == 0) { // child will run uniq

        // Reset stdi so that it reads from the pipe
        if ((dup2(fd[0], fileno(stdin))) == -1) {
            perror("dup2");
            exit(1);
        }

        // This process will never write to the pipe.
        if ((close(fd[1])) == -1) {
            perror("close");
        }

        // SInce we rest stdin to read from the pipe, we don't need fd[0]
        if ((close(fd[0])) == -1) {
            perror("close");
        }

        execl("/usr/bin/uniq", "uniq", (char *) 0);
        fprintf(stderr, "ERROR: exec should not return \n");

    } else {
        perror("fork");
        exit(1);
    }
    return 0;
}
```

*`shpipe.c` includes all of the error checking needed.*

![|300x296](https://i.imgur.com/rXx8eRF.png)

*File descriptors used by the program and their initial state.*

- When the program starts running:
    - Only the standard descriptors — to the console — are available

> [!note]+ Should be using the `fileno` function on `stdin`, or use the defined constant provided by the library
> - `STDIN = STDIN_FILENO` or `STDIN = fileno(stdin)`
> - `STDOUT = STDOUT_FILENO` or `STDOUT = fileno(stdout)`
>
> <!-- break -->
> - Could not fit either of these into my picture
>     - → Made up variables that would fit

![|300x288](https://i.imgur.com/69I8vi5.png)

- First step:
    - Call the *pipe* system call
    - Create the pipe that will connect `sort` and `uniq`

![|986x543](https://i.imgur.com/ATcQrnQ.png)

- Next step:
    - Call `fork`
    - $ Child process inherits all the same *open* file descriptors
- @ Our job is to set the file descriptors correctly, and close the ones that we do not need

### Consider the Parent Process (to Execute `sort`)

Consider the parent process first:

![|300x299](https://i.imgur.com/2CCc7Vf.png)

- Will be executing `sort`
- Want standard input to come from the file `file1.txt`
    - → Open the file for reading

![|300x295](https://i.imgur.com/0iQxGKa.png)

- Then use `dup2` to reset standard input
    - → Data will come from file
    - `stdin` is set correctly
        - Link is coloured blue

![|868x466](https://i.imgur.com/qbrjhtQ.png)

- Close `f`
    - Because will not be using that file descriptor directly
- Close `fd[0]`
    - Parent process will not be reading from pipe
- Connect `stdout` to the pipe
    - → Can send output from `sort` to input for `uniq`

![](https://i.imgur.com/rx0F9xU.png)

- Close `fd[1]`
    - Will not be writing to the pipe using this file descriptor
    - Using `stdout` instead!

> [!important]+ Why is it important to *close* all write descriptors eventually?
> - If we do not close all write descriptors:
>     - Read end of pipe will not know when there is nothing more to read from pipe

The communication channels for the parent are now set up correctly.

### Consider the Child Process (to Execute `uniq`)

Consider the child process:

![](https://i.imgur.com/gRjlDqb.png)

- Connect `stdin` to the to the read end of pipe
    - → When process reads from `stdin`, the data comes from the pipe

- Close `fd[0]`
    - Will not be using that file descriptor to read from the pipe
- Close `fd[1]`
    - Process will not be writing to pipe

![](https://i.imgur.com/DtQjqwB.png)

Now all of the file descriptors are correctly set up in the parent and the child processes.

### Load the Executables for `sort` and `uniq` into the Appropriate Processes

![](https://i.imgur.com/d53hUx6.png)

- `exec` call:
    - & Preserves all open file descriptors

> [!question]- It is important to close all file descriptors for writing on a pipe except the one that we plan to use because…
> - the read end of the pipe won’t know when all the data has been sent until all write descriptors are closed.

> [!question]- Follow-up Question 5B
> - Open file descriptors are inherited across the fork call.
> - We have to call `fork` after pipe for the pipe to have the right file descriptors open in both processes.

## Lecture

> [!important]+ Key points about pipes
> - Pipes are **uni-directional**
>     - Data can only flow in one direction
>     - Close ends of the pipes you are not going to use
> - Read returns 0 when other end(s) of the pipe are closed
>     - How the reader knows there will be no more data
> - Writes and reads move bytes
>     - So the writer and reader need to agree on how to communicate
>     - Writer should write *exactly* the *number of bytes* the reader expects to read
