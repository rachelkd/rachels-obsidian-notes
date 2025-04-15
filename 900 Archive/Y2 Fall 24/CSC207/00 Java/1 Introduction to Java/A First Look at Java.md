---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-09-07
Chapter: 1.1
Date created: Mon., Sep. 9, 2024, 2:13:44 am
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

# Defining Classes

- In Java, no code can exist outside of a class
- There are no functions — only methods
    - → Need to ==define a class== and a ==method== to put that code in

```java
class Hello {

}
```

*Outline of a class called `Hello`.*

- Python uses indentation
- Java uses curly braces; indentation makes no difference except readability

# Defining Methods

- Need to put code for printing `7 + 5` inside a method in our class
- Want to run that method

> [!info]+ How is a program is run in Java?
> In Python, we can run:
>
> - a single line of code at the shell
> - run an entire module i.e., code that exists in a single file
>     - executed from top to bottom
>
> Java does not have modules; instead: **classes**
>
> - When we execute a program in Java, we actually execute a class
> - A class may have multiple methods in it → special method called `main` that any class may define
> - Run a class → `main` method is executed

```java
class Hello {
    public static void main(String[] args) {
        // The method body will go here.
    }
}
```

*The main method specific signature to be recognized as the special method.*

- `psvm` shortform in IntelliJ IDE
- Similar to `if __name__ == '__main__'` in Python.

> [!info]- What does `public` mean?
>
> - Determines what code and where the code is allowed to call this method

# Printing

- Use a method: `System.out.println`

```java
class Hello {
    public static void main(String[] args) {
      System.out.println(7 + 5);
    }
}
```

- `System` is a class
- `out` is a static data member defined in that class
- Is an instance of another class that has many methods for printing things
    - incl. `println` (“print line”) → puts newline character at the end of whatever is printing
- **Semicolon** marks the end of a statement
