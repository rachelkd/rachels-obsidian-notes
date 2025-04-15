---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-10-05
Chapter: 1.6
Date created: Sat., Oct. 5, 2024, 3:07:30 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

- `Array`s = simplest type that Java provides for storing a number of items
- Has fixed length
    - Set when we construct array and can never change after that
    - All elements must have the same type
        - Stated at the moment when we declare any variable that will refer to an array
    - Type of the variable is not just “array,” but “array of `int`” or “array of `String`”

# Declaring an Array

- Must say:
    - Type is array
        - Done using `[]` square brackets
    - What type each element of the array will be
        - Said ahead of the square brackets

```java
int[] numbers;
```

*Declares an array of `int`.*

# Constructing an Array

- Array is a **[[Reference Types and Primitive Types|reference type]]**

    - `numbers` will not contain a sequence of `int`s, but will *refer* to an object containing a sequence of `int`s

- Syntax:
    - Use keyword `new`
    - Name of type
    - *Square brackets*
    - Argument: *size* of the array

```java
numbers = new int[5];
```

- Array object now exists on the *heap*
- Has 5 spots
- Each spot can hold an `int` and has default value `0`
- Variable `numbers` refers to whole object

## What Values before We Assign?

- Have assigned no values to these 5 spots, but are initialized automatically to *default value* for an `int` = `0`
- Each built-in type has a default value

## Another Way to Construct an Array: with an Initializer

- Can combine *construction* and *initialization* of an array into one step

    ```java
    numbers = {1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024};
    ```

*Constructs an array of exactly the right length to hold the given values, then assigns them to elements of the array.*

# Determining Length

- Access array’s `length` attribute

    ```java
    int[] sibling = new int[numbers.length];
    ```

# Accessing Array Elements

- Same as Python
- To assign a value to int at position `1`:

    ```java
    numbers[1] = 512;
    ```

- `int`s are **primitive** $\iff$ Value `512` stored directly in array
- Array indices start at zero
    - $\implies$ Code went into second spot
- Java arrays do *not* offer slicing or negative indices
    - Unlike Python

```java
String[] houses = {"Hufflepuff", "Gryffindor", "Slytherin", "Ravenclaw"};
String forHarry = houses[-3];
```

*Generates the error `Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: -3`.*

# Why Have Such a Restricted Type?

- Arrays are more restricted than Python lists
    - e.g., Size can never change
- Arrays are very efficient
- Takes both extra space and extra time in order to provide a list structure that can grow and shrink to arbitrary sizes
- Java gives a choice:
    - If you don’t need this flexibility, you can use a super efficient array
    - If you do, upgrade to a flexible structure e.g., `ArrayList`

## Mixing Types in Arrays Using Inheritance

- Every Java class is a descendant of the built-in `Object` class
- We can declare an array of `Object`s to hold various types

```java
Object[] miscellany = new Object[5];
miscellany[0] = new String("Songbird");
miscellany[1] = new Integer(1872);
miscellany[2] = new Monster("Fred");
miscellany[3] = new int[50];
```

- Primitive types have “wrapper” classes for use as Objects
- Arrays themselves are also Objects

### Consequences of Using Object Arrays

- Java only knows elements are Objects when reading code
- Type checking is limited:

```java
Object element = miscellany[0]; // OK
String s = miscellany[0]; // Error
```

### Casting Objects to Specific Types

- Use **casting** to tell Java the specific type:

```java
String s = (String) miscellany[0];
```

- Incorrect casting leads to runtime errors:

```java
String s = (String) miscellany[1]; // Runtime error
```

*Generates `java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String`.*

## Two-dimensional Arrays

- We can create arrays with multiple dimensions
- Elements of these arrays are themselves arrays

```java
int[][] table;
// Type of table is int[][] ("int array array" or "array of int arrays")
// At this point, we have merely a variable with space for a reference
```

### Creating a 2D Array

```java
table = new int[50][3];
// Now we have an array of 50 elements, each referring to
// an array of 3 elements, each storing an int
// Variable `table` refers to the whole structure
```

### Accessing Elements in 2D Arrays

- Must keep different dimensions straight

```java
table[49][2] = 123; // Legal
table[2][49] = 123; // Not legal
```

### Irregularly Dimensioned Arrays

- We can create levels of arrays separately
- Allows for non-rectangular structures

```java
int[][] table;
table = new int[50][];
// Array of 50 elements, each can refer to an int array, but doesn't yet

for (int i = 0; i < 50; i++) {
    table[i] = new int[3];
}
// Now in same situation as new int[50][3], but with more flexibility
```

- This decoupling of sizes allows for irregularly shaped multi-dimensional arrays
