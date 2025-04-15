---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-09-09
Chapter: 1.3
Date created: Mon., Sep. 9, 2024, 3:08:40 am
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

# More Java Types

```java
int age = 21;

// Strings must be double-quoted in Java, unlike Python.
String name = "Jude";

// This type is named boolean in Java, rather than bool (as in Python).
// Its values are true and false in Java (with no capital letter).
boolean graduated = false;

// Type double is like float in Python.
double gpa = 3.82;
```

- Additional integer-valued and real-valued types that allow us to either *save memory* (and give up precision), or *gain precision* (at the cost of using more memory)

# References vs. Primitives

- In Python, every value you can store (incl. `int`s) is an object
    - No variable stores a `1` directly
    - Must instead ==store a reference to an object== of type `int` that contains the value `1`
    - Recall memory model diagrams
- ==Java has two kinds of types==:
    - **Reference** types, and
    - **Primitive** types
- Type `String` is a reference type
- Type `int` is a primitive type
    - i.e., a Java variable can hold an `int` value directly inside itself
- All primitive types begin with a *lowercase* letter
- All reference types begin with an *uppercase* letter

### Primitives and References in Memory

Suppose we run the following code:

```java
public class Simple {
    public static void main(String[] args) {
        int age = 21;
        String name = "Jude";
        System.out.println("Ciao!");
    }
}
```

- Java has to keep track of many things to run code:
    - Every variable (name, type, location in memory, value)
    - Every object that has been *constructed*
    - Any method that is running must be tracked

Consider the state of computer memory at the moment when we reach the `println` statement in the code above:
![](https://i.imgur.com/4s7Appu.png)

- Three areas in memory:
    - **Call stack**
        - Where we keep track of the method that is currently running
    - **Object space**
        - Where objects are stored
    - **Static space**
        - Where static members of a class are stored
- On the call stack, we have a **stack frame** for the `main` method of class `Simple`
    - Currently running
    - Has our two variables:
        - `age` with value `21` directly inside it (primitive)
        - `name` that stores a *reference* to a `String` object that contains the value `"Jude"`
