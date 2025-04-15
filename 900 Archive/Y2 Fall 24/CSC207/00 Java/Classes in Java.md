---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-10-05
Chapter: 1.5
Date created: Sat., Oct. 5, 2024, 2:47:47 pm
Date modified: Tue., Dec. 10, 2024, 4:41:06 pm
---

# Instantiating an Object

- Create an instance of a class (i.e., creating an object) by calling its **constructor**

```java
StringBuilder name = new StringBuilder("Hello");
```

*Use keyword `new`*

- Class may offer more than one constructor
    - Compiler determines which one we are calling by the number and type of the arguments
- When Java evaluates the expression `new StringBuilder("Hello")`,
    - Allocates memory for new object
    - Evaluates arguments
    - Calls appropriate constructor
    - Returns a reference to the newly-constructed object
- Can assign reference to variable (like above) *or* use it *directly*:

```java
System.out.println(new StringBuilder("Professor").append(" Horton"));
System.out.println(new StringBuilder("Balakrishnan").indexOf("kris"));
```

# APIs

- Consult [standard Java Documentation](https://docs.oracle.com/javase/8/docs/api/) for full details on built-in Java class methods and data members of the class available
- Documentation specifies exactly how client code can interact with the class
    - Call this the **Application Programming Interface** (API)
    - Does not tell us implementation
    - Know the interface, though
- Implementers of the class are free to change the implementation in any way without having any impact on client code that may already exist
    - API is maintained → All is well

# Calling Methods

- We can call an **instance method** via an instance

```java
String band = "Arcade Fire";
// Call an instance method via an instance.
int size = band.length();
```

# Class Methods

- **Class methods** (or, **static methods**)
    - Some methods are not associated with individual instances of a class, but ==with the class as a whole==
    - Defined using keyword `static`
    - Access class method via class name

```java
// Call a class method via the class name.
double x = Math.cos(48);
```

- Class `String` has some class methods
    - e.g., `valueOf`
        - Takes `int` and returns equivalent `String`
        - Other versions of `valueOf` convert other types to `String`
        - i.e., `valueOf` is **overloaded** with multiple meanings

```java
int age = 12;
System.out.println("Age is " + String.valueOf(age));
```

# Accessing Data Members

- **Data members**
    - a.k.a **attributes** or **instance variables**
- If class has an instance or class variable (`static` variable) that is accessible to code outside class,
    - Can be referred to via an instance variable or the class name, respectively

<!-- break -->
- Class `Integer` has a class variable called `BYTES` → Reports the number of bytes of memory used to store an int value
    - Class variable $\implies$ Access using class name

```java
System.out.println(Integer.BYTES);
```

- More unusual to find an instance variable that we can access outside of a class
    - Implementation details are usually kept private

```java
Sneetch sam = new Sneetch();
System.out.println(sam.motto);
```

# Getting Rid of Unused Objects

When we construct an object, memory is allocated for it.

> [!question]+ When is memory ever de-allocated?

- Java does automatic **garbage collection**
    - Keeps track of all objects and when it can confirm that no variable refers to an object → De-allocates memory
    - $\implies$ Memory available for other uses
- If we know we do not need an object anymore → Explicitly drop a reference by setting variable holding reference to `null`
    - $\implies$ Hasten garbage collection, improve performance
    - May be unnecessary and make code needlessly messy
- In C, *heap memory* (i.e., object space) is only de-allocated when code explicitly says to do so
