---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 2
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-10
Date created: Tue., Sep. 10, 2024, 9:24:12 pm
Date modified: Tue., Dec. 10, 2024, 5:59:59 pm
---

# Running Programs

> [!question] Questions
> - What is a program?
> - What does it mean to *run* a program?

- To run a program:
    - Must be translated from *high-level* programming language → *low-level machine language*
- Two “flavours” of translation:
    1. **Interpretation**
    2. **Compilation**

# Interpreted vs. Compiled

- **Interpreted**
    - e.g., Python
    - Python program (interpreter) that translated and executes one statement of a program at a time
- **Compiled**
    - e.g., C
    - C compiler translates an entire C program to an *executable* file
    - Translated program contains machine code that runs natively on an operating system
- **Hybrid**
    - e.g., Java
    - Java programs are *compiled* to ==bytecode== (intermediate format) using Java compiler
    - Can take that bytecode to ==Java Virtual Machine== (JVM)
        - an executable that runs this intermediate bytecode
    - JVM translates and executes one statement of the bytecode program at a time
    - As it runs → looks for optimizations to make
    - Is there a clear benefit to introducing that intermediate form?

# Compiling Java

> [!important] You need to *compile*, then *run*.

If you use command line → compile manually:
1. Compile using “`javac`”:

      ```
      javac HelloWorld.java
      ```

2. This produces file “`HelloWord.class`”

      ```
      ls
      HelloWorld.class  HelloWorld.java
      ```

3. Run the program using “`java`”

      ```
      java HelloWorld
      Hello world!
      ```

- Modern IDEs (e.g., IntelliJ) do this for you with the help of a *build system*
    - e.g., Maven, Gradle
    - Build systems take care of the build process → focus on code!

# Computer Architecture

> [!def]- **Operating system** (OS)
> - manages various running applications
> - helps applications interact with the hardware
> - OS works directly with the hardware

- ![|400](https://i.imgur.com/0FOBOAI.png)
    - e.g., Applications: Python, C, Java

# Compiled Applications

- Is it okay to just have apps at the top, or is there something smart we can do to make our lives easier?
- Programs written in languages are *compiled* into OS-specific applications → interface directly with OS
    - e.g., C, Objective C, C++, Pascal
    - Inconvenient: When you compile a C program on a Mac, it only works on that OS → Not portable to other OS
- If you write a program in C, compile, and execute, it directly makes calls to the OS
    - The interface it is making use of
- ![|300](https://i.imgur.com/udvVt2f.png)

# Virtual Machine Architecture

> [!def]- **Virtual machine** (VM)
> - An application that pretends to be a computer
>     - e.g., Java, Python, Scheme have their own VM

- VM application is written for each OS
    - → programs are *portable* across operating systems

![|300](https://i.imgur.com/icAiyRr.png)

For example,
- We write Java files
- → Byte code (program for JVM)
- → Run on the JVM (virtual machine)
- → JVM worries about what OS we are running on

> [!important]+ Takeaway
> - This does not take away from the fact that we are running on different operating systems; just ==changes who is responsible for different OS==
> - Not the programmer’s responsibility; it’s the JVM’s job
> - Someone needs to implement the JVM if there’s an update to the OS to correspond to that change

- When people designed Java, they designed it to be extremely portable
- Can think of JVM as interpreter for byte code
    - `.java` → `.class` → JVM

# Java Architecture

- What is the **JVM**?
    - Application that is something like an OS
- Java programs are compiled to an intermediate state called **byte code**
    - Machine code for the JVM
- Same compiled Java program can run in any JVM on any OS
- JVMs optimize byte code as it runs
    - can be as fast as code written in C

![|300](https://i.imgur.com/b1T5mKu.png)
