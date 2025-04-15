---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-09-07
Chapter: 1.2
Date created: Sat., Sep. 7, 2024, 7:29:36 pm
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

# Declaring Types

**Python vs. Java**

- Rather than writing type hints (in Python), we make it a part of the code in Java
- When we say `type(stuff)` in Python, we are told the type of the *object* that `stuff` refers to
    - Variable `stuff` has no type; can *refer to an object* of any type
- In Java, every value has a type and so does ==every variable==

```java
int i;
```

- Specify a variable’s type before assigning a value to the variable
- A variable’s type can never change
- We *declare* a variable called `i` to be of type `int`

# Declaration and Assignment

```java
int i = 42;
```

*We can assign a value immediately after declaring the variable in the same line of code*

- In the example in [[#Declaring types]], we would be postponing *assigning* a value to `i` until later
    - Variable’s name is known to Java
    - Space has been reserved to store its value
    - Variable is given a **default value**

### Default Values

- For `int`s:
    - `0`
- For objects:
    - `null` (equivalent to Python’s `None`)

# Keeping Track of Our Variables

- Java must keep track of ==four things== associated with each variable:
    1. **Variable name**, provided when we *declare* the variable
    2. **Variable type**, provided when we *declare* the variable
    3. **Memory space** used to hold the value of the variable
    4. **Value** of the variable, given with an **assignment statement**
- Only the ==value of the variable can change==

# Errors

There are some errors related to variables and types that Java can detect.

### Didn’t Declare

```java
public static void main(String[] args) {
    i = 42;
}
```

*Used a variable that was not declared.*

- `"i cannot be resolved to a variable"`
- i.e., Java is trying to find a variable called `i` and is unable to

### Assign Value of the Wrong Type

```java
public static void main(String[] args) {
    int i = 19.6;
}
```

*Assigned the wrong type of value to a variable.*

- `"Type mismatch: cannot convert from double to int"`
- Java tried to be accommodating by converting `19.6` to variable `i`’s type of value, but it could not → would have caused a loss of info

If we assign an `int` value to a `double` variable, it can make the conversion:

```java
public static void main(String[] args) {
    double i = 19;
}
```

### Declare a Variable Using a name that Already Exists

```java
public static void main(String[] args) {
    int i = 15;
    int i = 42;
}
```

*Declared a variable called `i`, then did so again.*

- `"Duplicate local variable i."`
