---
dg-publish: false
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "19"
  - "21"
  - "22"
Chapter: 
Slides/Notes: 
Date: 2025-03-25
Date created: Mon., Mar. 24, 2025, 11:50:25 pm
Date modified: Sat., Apr. 12, 2025, 11:45:45 pm
---

# Shell Programming

## Video 1

- **Shell**
    - Command interpreter
    - Program you type commands to that causes them to be executed

![|500](https://i.imgur.com/DovKXMB.png)

- If you log into the text console (via `ssh`)
    - First checks username and password
    - Then runs a *shell*
- If you log in on a graphical console:
    - Runs some sort of desktop environment
    - Have to run terminal program explicitly
    - Terminal program runs a *shell*

> [!info]+ The **shell** is a big loop.
>
> - Prints a prompt
>     - Traditionally `$`
> - → Reads a command
> - → Parses the command
> - → Executes the command
> - Loops
>     - Printing another prompt and so on
>
> ![](https://i.imgur.com/rFhNArX.png)

- *Two* main varieties of shell programming languages:
    - **`sh`**
        - Version 7 shell
        - `ksh`
        - `ash`
        - `bash`
        - `dash`
        - And others
        - All implementations of the basic `sh` language + additional features
            - Which author of particular program thought was a good idea
    - **`csh`**
        - `csh`
            - Inferior programming language
        - `tsch`
- When we write shell programs:
    - Always test on at least two different implementations of `sh`
    - → Be sure that we have not inadvertently used any extra features that might not be present when shell is run by somebody else
- Some people use `csh` or `tsch` for interactive use
- For programming:
    - & Use `sh`
    - Familiar with typing a command and hitting return
        - e.g., `cat file`

### Command-line Substitutions

> [!info]+ When shell parses command-line, it performs many **command-line substitutions**
>
> - e.g., File name wildcards are substituted for the matching list of file names by the shell
>     - Before executing command!

```bash
cat *.c
```

- `cat` program does not see that `*.c`
    - & Sees the expanded argument list based on the list of files in the current directory

![](https://i.imgur.com/TxVE8S0.png)

#### Experimenting with `echo`

- `echo` outputs all its command-line arguments
    - Separated by spaces

```bash
$ echo hello
hello
```

```bash
$ echo *.c
a.c b.c
```

- Shell interprets `*.c`
    - Expands it to the list of matching file names
    - Passes that to `echo` rather than `*.c`
- % Since `echo` outputs command-line arguments, we get to see how command-line substitution actually works
- & `sh` is a complete programming language
    - But a weird one
    - Can use `sh` in a usual software tools way to execute commands from a file instead of interactively

### Running a File of Commands

- `vi hello`

- Have to put `?` is single quotations
    - Otherwise would be interpreted file name wildcard

![](https://i.imgur.com/gJ4OYc7.png)

- `cat hello`
    - `cat` command reads all these lines
    - Outputs them
- Execute the list of commands:
    - Run `sh hello`

![](https://i.imgur.com/5oxuOEL.png)

> [!obs] Shell works the same as reading commands form a file on disk as when reading commands from terminal interactively

### Variables

- Any programming language needs *variables*
- In `sh`:
    - Command-line substitutions are involved in using variables

```sh
i=3
i = 3
```

- Assignment statement
    - First line above
    - Press return → Done
    - $ No spaces in an assignment statement
- `i = 3` means something different:
    - Execute a command named `i`
    - With first argument `=`, second argument `4`

![](https://i.imgur.com/5t0g7RY.png)

- ? How do we know this is doing something? How can we explore what the value of `i` is?
    - Write `$i`
        - Shell substitutes that `$i` with the value of the variable `i`

```sh
$ echo $i
4
```

- Same as typing `echo 4`
- % Using `echo` to see how the command-line substitution works
    - In practice:
        - Use `$i` in a command in some appropriate way

```sh
$ $i
4: Command not found
```

- If we do `$i`:
    - Tries to execute a command `4`
- Can use `echo` where we normally use a print statement

```sh
$ echo I would like to buy $i cupcakes
I would like to buy 4 cupcakes
```

- If variable `i` contains a suitable file name:

```sh
$ cat $i
cat: 4: No such file or directory
```

- `cat` gives us the error message
- `cat` does not see `$i`
    - & Sees *substituted* value
    - As shell does the line substitutions

![](https://i.imgur.com/GAvgipA.png)

- Without the `$`
    - `i` in `i+1` means the *string* `i`
        - Not the value `i`!

![](https://i.imgur.com/M8FPCGE.png)

- `$` does not help much
- @ Need some program that will do the arithmetic

### `expr`

- `expr`
    - Utility program
    - Particularly useful for doing arithmetic in shell programs

![](https://i.imgur.com/JFIYAK6.png)

> [!question]+ How do we apply this to `i`, to add `1` to `i` and put it back into `i`?
>
> - Need another category of command-line substitutions
>     - Invoked with the character backquote \`
>     - & What is inside backquotes is interpreted as a command from scratch
>         - Command is executed
>         - Output is captured
>         - Output is substituted in the command line

### Backquotes \`

```sh
i=`expr 4 + 1`
```

- Runs `expr` command
    - Gives it the arguments `4`, `+`, `1`
- `expr` outputs `5`
- Backquotes substitutes that into command line
- → Get the command `i=5`

![](https://i.imgur.com/rKI0vIA.png)

- ? How to increment `i` again?

```sh
i=`expr 5 + 1`
```

More generally,

```sh
i=`expr $i + 1`
```

- Shell executes `expr`
    - Which involves a command-line substitution
    - Puts in value `5`
    - `5 + 1` will be executed
    - Output will be 6
        - Will not appear in terminal
        - Will be captured and substituted in command line

![](https://i.imgur.com/PCd1ZIp.png)

> [!important]+
>
> - `expr $i + 1`
>     - Separate command
>     - Executes that commands
> - Substitutes otuput of that command into command line

- $ No *spaces* around the equals sign
    - If there were spaces, that would be a command named `i`
- $ There are spaces in `expr $i + 1`
    - Make it easier to write `expr`

![](https://i.imgur.com/XgxHHpZ.png)

- `?` is not interpreted as a wildcard character
    - Because `?` is quoted in `'?'`
- Now, it is executing the `read` command
    - **read** takes a list of variable names

![|400](https://i.imgur.com/O5ejjCN.png)

- Type something → gets assigned to the variable `name`

![](https://i.imgur.com/381W1hz.png)

- & `read` species more variable names
- e.g., Type three tokens → Each token goes into one corresponding variable
    - If there are more tokens than variables in the command:
        - & All the rest go into the last variable

```sh
$ echo $x
foo
$ echo $y
bar baz
```

- % If you had one variable, it reads the whole line into that variable
- If there are more variables than tokens:
    - Subsequent variables are assigned to be the *empty string*

> [!info]+ Your `sh` program can include any `sh` command — including any useful little utilities you write in C
>
> - Common for a complex package to include some C code and some shell scripts
>     - Shell scripts invoke C program as needed

> [!question]+ Where does the shell find these commands?
>
> - & Looks for a list of directories specified by `PATH`

### `PATH`

![](https://i.imgur.com/nGwIVAj.png)

- `PATH`
    - Special variable
    - When you type a command that does not contain any slashes in the command name:
        - & Looks through the list of directories specified by this variable
            - Separated by colons
        - & Looks for command in each one of these directories until it finds it

### Follow-up Questions

> [!question]+ Question 1a
> Assume the following code fragment has executed:
>
> ```
> d=31
> ```
>
> Write a `sh` command involving variable d to print the following to standard out:
>
> ```
> March has 31 days
> ```
>
> > [!success]- Solution
> > `echo March has $d days`

> [!question]+ Question 1b
> Variable x has been assigned a digit between 1 and 10. Write a shell command to calculate x minus 1 and display the result to standard output.
>
> > [!success]- Solution
> > `expr $x - 1`

> [!question]+ Question 1c
> Variables `width` and `height` have been initialized. Calculate the area of a rectangle using `width` and `height`, and assign the resulting value to a variable named `area`.
>
> > [!success]- Solution
> > area=\`expr $width '\*' $height\`

## Video 2: Control Constructs

- Conditional constructs in `sh`
    - Based on commands’ exit statuses

![](https://i.imgur.com/jgepkEy.png)

- A command in Unix *succeeds* or *fails*
    - e.g., `grep arn students` succeeds
        - `grep ern students` fails

> [!question]+ How do we tell when a command succeeds or fails?
>
> - For interactive explorations:
>     - & Special variable `$?`
>         - Tells us the exit status of the last command
>
> > [!tip]+ Use `echo $?` to see value of this variable
> > In Unix,
> >
> > - `0` is success exit status
> > - Anything non-zero indicates a failure

```sh title:s1
if grep Fred students
then
    echo end of list
else
    echo No students named Fred
fi
```

- $ `if`
    - `if` in `sh` is followed by a command

![](https://i.imgur.com/mpLEVYB.png)

- Command passed to `if` is *executed*
- If command succeeds:
    - Do `then` clause
- If command fails:
    - Do `else` clause

> [!note]+ Case: Fail
> ![](https://i.imgur.com/s1u24fS.png)

### `if` In `sh`

We have:

- `if` keyword
    - Then an arbitrary `sh` statement
- Next command is `then`
    - Have one or more statements for `then` clause
- Have one or more statements for the `else` clause
    - `else` keyword is *optional*
- `fi` keyword
    - `if` spelled backwards

<!-- break -->
- `if` command executes an arbitrary other command
- → `then` has to be on the next line
    - If we put `then` on same line as `if <command>`:
        - No way for shell to tell that it is not supposed to be an argument to `grep`
    - % To make it uniquely parsable, we have to put `then` on the next line

> [!note]+ Most normal modern programming languages are *free-format*
>
> - Any white space counts as the same
>     - Spaces, tabs, newlines
> - ! Python is not like that
>     - Newlines and indentation levels are significant syntactically

- `sh`
    - Comes closer to being free-format than Python does
        - But want to ‘return’ to separate commands interactively
            - So it also does in shell scripts
- `;`
    - Command separator

### `test`

- ? How do you write “if $x$ is less than 3?”
    - `test 2 -lt 3`
- **==`test`==**
    - General testing command
    - Exists for this purpose
    - Produces no output
    - But $2 < 3$ is true

![](https://i.imgur.com/1ItaiVn.png)

- One of those arguments might be a variable:
    - `x=5`
    - `test $x -lt 3`
    - This fails since $5 \not< 3$

If we want to use it in a shell script:

```sh
x=5
if test $x -lt 3
then
    echo This is very surprising!
fi
```

![](https://i.imgur.com/p2PyvP8.png)

Might see something like this:

```sh
if [ $x -lt 3 ]
```

- Can also write the `then` and whatever
- The way that this is implemented is a *hack*
    - $ There is a `/bin/[`
        - File in `/bin`
        - What we are actually running above
- & `if` is *always* followed by a command
    - `test` and `[` are the same program
        - % Use Unix tool `cmp` to compare the files
            - No output → Files are identical

> [!important]+ You can write test, or you can writ `[`
>
> - & But what follows `if` is a **command**
>     - Command is executed and has all the effects it has from executing
> - & Success or failure exit status determines whether we take then `then` or the `else` clause in the `if`

#### `test`: Numeric Comparison Operators

![](https://i.imgur.com/LKbVHK3.png)

#### `test`: String Comparisons

> [!info]+ `test` also does string comparisons.
> ![](https://i.imgur.com/pOuZJXY.png)

- Everything in `sh` is a string
    - But this still makes a difference
- `test 03 = 3`
    - String comparison
    - Unequal
- `test 03 -eq 3`
    - Numeric comparison
    - Equal

#### `test`: File-testing Operators

![](https://i.imgur.com/JlDe1k4.png)

- Many other file-testing operators
    - Other things such as boolean operators
- Check `man test`

### `while`

![](https://i.imgur.com/ggZC1Xq.png)

- `while` keyword
    - & Followed by an arbitrary command
    - e.g., `while test <something>`

### Boolean Constants

![](https://i.imgur.com/q3rAl9l.png)

- `true`
    - Exit 0
- `false`
    - Exit 1

### `read`

- `read` can be used in a `while`
    - Because it fails on end-of-file

```sh
while read x y
do
    echo x is $x and y is $y
done
```

- `while read x y`
    - Will do that read command
- If it succeeds:
    - Do body of the loop and continue
- If we hit end-of-file:
    - Fails
    - Loop is done

![](https://i.imgur.com/FVaMnFs.png)

- ? How to signal end-of-file from terminal?
    - Special control character `Ctrl+D`
- This makes more sense non-interactively:
    - Have data file `d1`
    - Run `s4` with that data file as input

### Remove First line of a File so long as the File Has a Capital Q in it

```sh
while grep Q file
do
    (echo 1d; echo w) | ed - file
done
```

- `while grep Q file`
    - “As long as there is a Q in file”
- Do this compound statement
    - Formulates input to `ed` using two shell commands
        - `echo 1d`
            - Delete first line
        - `echo w`
            - Write the file
    - % We don’t need a quit command because when `ed` gets end-of-file on the standard input, it will exit

![](https://i.imgur.com/v1vWWMy.png)

- % Script will work, but produces messy output
- Only want `grep` for its exit status
    - Not to *see* the lines with the Q’s
    - Just check if there is a Q in the file

![](https://i.imgur.com/dlYoJIZ.png)

- Could throw away `rep` output by redirecting it to some file name we do not care about, or
- & Use special file `/dev/null`
    - Device file with the simplest possible driver
    - Driver just says “return 0”
        - Does nothing
        - → Data is lost
            - Which is what we want

### `||` And `&&`

- `||` and `&&`
    - Operators
    - Combine boolean statuses just like in C or Java
        - Or like the `or` and `and` keywords in Python
    - & Have *short-circuit* behaviour
        - Right operand is only evaluated if necessary
        - → A command which is only executed if necessary

```sh
if test $x -gt 3 && test $x -lt 10
```

- `test $x -gt 3` is executed
    - If it fails → Right command is *not* executed
        - Exit status of the compound command is the exit status of the left command
    - If left command succeeds → Right command is also *executed*
        - Exit status of compound command is the exit status of the right command

### Nested If Statements

![](https://i.imgur.com/n77YKIk.png)

- Left, cascading if-then-else
    - Common
    - But pretty messy

`sh` is free-format, so we could write it like this:

```sh
if foo1
then
    bar1
else
if foo2
then
    bar2
else
    bar3
fi
fi
```

- Helps a little to show the structure
- Will get an increasing number of `fi`‘s at the end all in a row as it gets more complicated
- Also gets error-prone

> [!tip]+ Special combined else-if keyword
>
> - `elif`
> - Combined else-if keyword does *not* introduce an additional nesting level
>     - → Only have one `fi` on the end

Suppose we did not have anything to do in the `then` case.

```sh
if foo1
then

elif foo2
then
    bar2
else
    bar3
fi
```

- ! This is not syntactically valid!
    - `then` clause is *one or more* statements
        - Not “zero or more” statements
- & Need the `sh` **null statement**
    - Statement that does not do anything
    - `:`
    - Similar to `pass` statement in Python, or `;` in C or Java

![](https://i.imgur.com/oa9WOCU.png)

### Bad Practices

Do not do this:

```sh
foo
if test $? -eq 0
then
    something
fi
```

- Do not run a command → Use exist status as `$?` in `test` in an `if`
- & This is what `if` does
    - Checks whether a command succeeds or fails

```sh
if foo
then
    something
fi
```

## Video 3: Quoting in `sh` and More Control Constructs

### Quoting in `sh`

![](https://i.imgur.com/TcxAwKX.png)

- Normal programming languages:
    - String needs *quotes*
    - Commonly `"`
    - e.g., If we want to run `ls` command on two directories from C program:
        - `exec("ls", "dir1", "dir2")`
        - ! Pretty cumbersome compared to the command in `sh`
            - `$ ls dir1 dir2`
            - We do not want to write `$ "ls" "dir1" "dir2"` in the shell prompt
- In C, Java, Python:
    - `"ls"` is very difference from `ls`
        - String vs. reference to some existing variable or function or something named `ls`

> [!important]+ In `sh`, `"ls"` and `ls` are the *same*.

- Normal programming language:
    - When you write `"ls"`:
        - Programming language processor *interprets* it
        - Figures out: is this a keyword, variable, function name, etc.
- `sh`:
    - Shell will only interpret some things
    - `ls` is a string
        - Even without quotes
    - If we wanted it to be interpreted as a variable name:
        - Preface with `$` e.g., `$ls`

> [!abstract]+ Good compromise for something which is both a programming language and the way we type commands daily
>
> - But it has some odd consequences
> - ! Need a way to *suppress* the special interpretation of interesting characters

> [!question]+ How do we use `echo` to output an actual `>` character?

Python:

```python
print 'To forward your mail to user@host, type:  echo user@host >.forward'
```

> [!danger]+ In `sh`, we cannot just type `echo Fwd to user@host, type:  echo user@host >.forward`.
>
> - Output went into file `.forward`

![](https://i.imgur.com/AmiwnMJ.png)

- Three possible ways to quote this
    - Backslash
        - `echo To forward your mail to user@host, type:  echo user@host \>.forward`
    - Single quotes
        - `echo 'To forward your mail to user@host, type:  echo user@host >.forward'`
    - Double quotes
        - `echo "To forward your mail to user@host, type:  echo user@host >.forward"`
- ! Single and double quotes have different semantics

Suppose we customize the address for the user in a variable.

![](https://i.imgur.com/sV3ECQ4.png)

- Single quotes:
    - Too much quoting
    - Want special meaning of `$` to be onoured
        - But special meaning of `>` to be suppressed
    - & Double quotes suppress everything except for `$`, `\` \`, closing `"`

![](https://i.imgur.com/QvnR2FN.png)

![](https://i.imgur.com/9wP6AJm.png)

- $ Single and double quotes suppress the special meaning of a *space*
    - Backslash does not
        - We see a single space instead of double space
- Backslash:
    - First argument to `echo` is `Fwd`
    - Second argument is `to`
    - Third argument is `user@host`
    - And so on
- Single and double quotes:
    - Only one argument to `echo`
        - Which is the whole string
- & `echo` outputs all of its arguments, separated by spaces
    - Result is very similar

![](https://i.imgur.com/U39bRw4.png)

Compare these two commands:

- Second case:
    - File name is actually three characters `a b`
    - String is all *one* argument to `cat`
    - & Space is a special character to the shell
    - & Special meaning is also suppressed by quoting

Suppose we have a file named `hello world`, with a space in the file name.

- Might have a variable containing this file name
    - `$ filename='hello world'`

![](https://i.imgur.com/TcFe2hB.png)

- Cannot `cat` file normally (by doing `cat hello world`)
    - `cat` would get two arguments
        - Neither of which is the file name
    - & → Have to use quotes

```sh
cat 'hello world'
```

> [!warning]+ If we wanted to use the variable, we cannot write `cat $filename`
>
> - Shell interpolates the variable value
> - Takes the spaces as separating arguments

```sh
cat '$filename'
```

- Single quotes suppress too much
- & → Want to use double quotes

```sh
cat "$filename"
```

- Double quotes
    - Suppress interpretation of the space
    - Does not suppress interpretation of `$`

> [!tip]+ Almost any time we have a variable whose value might include a space, we put that variable interpolation in **double quotes**.

### More Control Constructs

#### `for`

![](https://i.imgur.com/ocIsNOO.png)

- `for`
    - Loops over any number of strings
- % We do not always specify the list of strings literally
- Second `for` script loops over all file names ending with `.c`

#### `seq`

![](https://i.imgur.com/MpDriaZ.png)

```sh
$ seq 1 4
1
2
3
4
```

- `seq`
    - Outputs a range of numbers based on its argument
        - Similar to `range` in Python
- & Useful for `for` statements
    - Using backquotes \`

```sh
for i in `seq 1 4`
```

- Runs `seq 1 4`
- `seq`’s output will be captured
- Output will be substituted in the command line
- Will be the same as writing `for i in 1 2 3 4`

#### Add All the Numbers from 1 to 100

![](https://i.imgur.com/2P5P7v5.png)

#### `for`: Loop Through All Command Line Arguments

![](https://i.imgur.com/4x7X9pi.png)

#### `case`

![](https://i.imgur.com/XnWJHeV.png)

- Syntax is pretty weird with the `)` in the case labels
    - Meant to look like numbered items in traditional text
- If `$i` expands to `hello`:
    - Execute the first case
- Else if it expands to `goodbye` :
    - Execute the second case
- & Each particular case is terminated with the double-semicolon `;;`

##### Default case

![](https://i.imgur.com/6iUtnD6.png)

- Each of the case labels is actually in the *glob* notation
    - Pattern language used for filename wildcards
- For a default case:
    - Use *asterisk* `*`
        - Which matches anything
- & Case labels are tested *in order*
    - Only going to check against the value of `$i` after `hello` and `goodbye` do not match

##### Glob Notation in case Labels

![](https://i.imgur.com/VNv5Oer.png)

- Change `hello` to `hello*`

## Video 4

### File Descriptors

> [!info]+ A running program in Unix has a certain set of *open files* at any one time.
>
> - Open files are identified by small *integers*
>     - **File descriptors**

- Normally. when program is started:
    - Already has *three* files open

> [!question]+ What file descriptor do most programs *read* from?
>
> - File descriptor 0
> - i.e., **standard input**

> [!question]+ What file descriptor do most programs *write* to?
>
> - File descriptor 1
> - i.e., **standard output**

- & Terminal in Unix is also a file
    - By default:
        - These two files are both the terminal
    - When program reads input from standard input:
        - Reads from terminal
    - When outputs on standard output:
        - Writes to terminal

### I/O Redirection

- & Input and output can be *redirected*

```bash
foo <file1 >file2
bar >>file2
```

- Shell opens file `file1` for *input* on file descriptor 0
    - Opens file `file2` for output on file descriptor 1
- % Happens all before shell executes program named `foo`

> [!idea]+ Think of the operators as arrows
>
> - `<`: **direct in**
> - `>`: **direct out**

> [!info]+ `>>`
>
> - Opens the file for **append**
>     - Instead of overwrite

- Command runs `bar` with its output redirected to `file2`, but appending to what is already there in `file2`

#### `<<`

![](https://i.imgur.com/LTLZpKD.png)

- **`<<`**
    - Takes input right from shell script text
- $ Command outputs `7`
    - 7 lines as counted by the `wc -l` command
- % `<<` is followed by an **arbitrary token**
    - When it appears later — alone on a line
    - → Terminates the input
    - Called the **here-is** or **here** test

> [!important]+ `<< EOF … EOF`
>
> - Dollar signs and backquotes inside the here-is text are **interpreted**
>     - Just like inside double quotes

- To get behaviour more like single quotes:
    - Quote the original mention of the end-of-file word like this:
        - `wc -l <<\EOF`
        - % Any dollar signs or backslashes or backquotes inside the here-is text are taken literally
            - Do not have their special meaning
- % Bash shell also has triple-character redirection operations
    - ! Should not be used because resulting program will then not work under other versions of sh

#### Standard Error

- Third file is open on file descriptor 2
    - Open for output
    - Also connected to terminal by default
        - Like file descriptor 1
    - Called **standard error**
        - & Used for error messages
- ? Why have two standard file descriptors for output?
    - Consider `cat file | grep blof`
        - If file exists, but does not contain string `blof`:
            - No output
        - If file does not exist or is unreadable:
            - `cat` will give an error message
        - & Do not want this error message to go down to the pipe
            - Where `grep` will read and discard it because it does not contain the string `blof`
        - ! Need to see this error message on the terminal
            - → Want this error message to bypass the pipe
    - & → Write error messages to file descriptor 2 — standard error channel
        - Instead of FD 1
        - Which is mixed in with normal output
- When you redirect standard output:
    - (with a pipe `|` or redirect into file with `>`)
    - & Messages to standard error will still be seen on screen
        - Rather than going into the pipe or into file on disk

### Managing File Descriptors in Shell

![](https://i.imgur.com/hxc6sh2.png)

- & Can manage file descriptors fully in the shell
- `cat >file`
    - Redirects file descriptor 1 into file
- ? How to redirect file descriptor 2?
    - `cat 2>file`

> [!important]+ Must be no space between `2` and `>`
>
> - → Still possible to do `cat 2 >file`
>     - Cats a file named 2 and redirects standard output to file
> - Whereas `cat 2>file` runs cat with no arguments but with file descriptor 2 redirected into the file
>     - File descriptor 1 left as is

- Can also do both for same command
    - e.g., `cmd >file1 2>file2`
- Can also redirect file descriptors to each other
- To specify which file descriptor to redirect:
    - Put number on left of symbol
- To redirect *to* a file descriptor:
    - Put number on the right of the symbol, prefaced by `&`
    - e.g., `foo >&2`
        - Redirect standard output to standard error

### Command-line Arguments

![](https://i.imgur.com/p8xozvO.png)

- Consider `cat file`:
    - `file` is an argument to `cat`
    - `cat` program has to be able to get that string to be able to open the appropriate file
- ? How does shell script get the two command-line arguments?
    - `$` followed by a single character for special variable name
        - e.g., `$1`, `$2`
    - Can only go up to `$9`
        - % If we are going that high, we would want to be doing this in a loop anyway

<!-- break -->
- @ Check if user supplied enough arguments before you start accessing them

![](https://i.imgur.com/hkHYDEq.png)

> [!tip]+ Access name of program by using argument zero `$0`
> ![](https://i.imgur.com/xfBKMqL.png)
>
> - % Can also do this in C

> [!question]+ How do we access all arguments?
>
> - Special variable `$*`
>
> ![](https://i.imgur.com/u7kaEcO.png)

> [!warning]+ When argument is interpolated with `$*`, there is *no* quoting
>
> - Space still separates the words
> ![](https://i.imgur.com/PH85Vl1.png)

- Double quotes allow interpretation of `$` while suppressing interpretation of space
    - -> Creates a conflict:
        - Sometimes need to avoid quotes
            - To preserve argument separation
        - Sometimes need quotes
            - To preserve spaces in file names

#### `$@` Syntax

> [!important]+ `$@` solves this problem
>
> - Outside double quotes:
>     - `$@` behaves exactly like `$*`
> - Inside double quotes:
>     - Double quotes are considered to cease between arguments, while still operating within each argument

![](https://i.imgur.com/NP2jqPi.png)

> [!tip]+ Use `$@` inside double quotes when we want to pass all command-line arguments to another program.

#### Processing Command-Line Arguments

- Loops aren’t always needed for command-line argument processing
- Good practice:
    - & Leverage existing tools rather than writing custom code
- Many shell scripts take the form:
    - & `cat "$@" | …`
- Processes zero or more filename arguments without loops
- Handles zero-filename case correctly (reads standard input)

![](https://i.imgur.com/wX4h0gi.png)

#### Simplified For-Loop for Arguments

```sh
for variable
do
    commands
done
```

- Implicitly loops through all command-line arguments

#### `shift` Command

> [!info]+ `shift` moves all command-line arguments down one position
>
> - `$3` becomes `$2`
> - `$2` becomes `$1`
> - `$1` is discarded

- Useful when “consuming” arguments in a loop
- Example: Processing a category name followed by filenames:

  ```sh
  category="$1"
  shift
  cat-$category "$@"
  ```

- After `shift`, `"$@"` contains all remaining arguments

#### Special Variable Interpolation

- `${variable}` syntax allows unambiguous variable interpolation
- Same meaning as `$variable` but useful in specific cases
- Example with `sed`:

  ```sh
  # This doesn't work - "ip" is treated as variable name
  sed -n $ip file

  # This works - "i" is the variable name, followed by "p"
  sed -n ${i}p file
  ```

#### Comments in Shell Scripts

> [!tip]+ The comment character in `sh` is the number sign (`#`)
>
> - Comments extend to the end of the line
