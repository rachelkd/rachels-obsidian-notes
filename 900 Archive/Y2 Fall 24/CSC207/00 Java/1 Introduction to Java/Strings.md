---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 4
Module:
  - '[[Java]]'
Date: 2024-09-23
Chapter: 1.4
Date created: Mon., Sep. 23, 2024, 2:43:04 am
Date modified: Wed., Oct. 30, 2024, 5:51:49 pm
---

# Class String

- **String** class:
    - Represents sequences of characters

```java
String s1 = new String("Hello");
```

*Created a new string ==object== to represent the text Hello.*

- Use *double quotes*

```java
String s2 = "bye";
```

*Java short-cut for creating string objects.*

# String Pool

- **String pool**:
    - Used to store the values of string literals
    - Avoid excessive memory
    - a.k.a. string intern

```java
String s1 = "Hello";
String s2 = "Hello";
```

*Create two strings without using the `new` keyword.*

- When `s2` is created, Java goes to the string pool and looks for this phrase
    - `"Hello"` has already been added to the string pool
    - ⇒ `s2` points to the same `"Hello"` phrase as `s1`
- `s1 == s2` evaluates to `true`, just like Python

```java
String s1 = new String("Hello");
String s2 = new String("Hello");
System.out.println(s1 == s2);
```

*`false` is printed to the console.*

- Created ==two **instances**== of the `String` class
    - ⇒ `s1` and `s2` are different objects
    - ⇒ Result is `false`

# Strings Are Immutable

- `String` objects are immutable in Java
    - Can never change a `String` object once it has been created
- Can ==perform operations== on `String`s
    - Returns a new `String` object

# String Operations and Methods

#### String Concatenation

```java
// We can't change s1 or s2, but we can make a new String out of them.
String s3 = s1 + s2;
```

#### `String` Methods

```java
// Indexing
char c = s1.charAt(2);  // In Python: s1[2]
// Slicing
s1 = s1.substring(2, 4); // In Python: s1[2:4]

// Stripping
s1 = "   Here is my string  .   ";
s1 = s1.trim();  // In Python: s1.strip()
```

- Also `length`, `startsWith`, `indexOf`

# Mutable Strings: StringBuilder

- `StringBuilder`:
    - Represents a sequence of characters
    - Object is **mutable**

```java
StringBuilder sb = new StringBuilder("ban");
// We don't have to create a new object in order to append;
// We can modify sb itself.
sb.append("phone");
// Some of the other methods allow us to modify a StringBuilder:
sb.insert(3, "ana");
sb.setCharAt(3, 'o');
sb.reverse();
```

Incorrect declaration of `StringBuilder`:

```java
StringBuilder sb = "ban";
```

*Generates the error: `incompatible types: String cannot be converted to StringBuilder`.*

# Single Character Strings: `char`

- We can have a `String` that contains just one character

    ```java
    String s = "x";
    ```

- Also a **[[Reference Types and Primitive Types|primitive type]]** capable of holding a single character called `char`

    ```java
    char c = 'x';
    ```

    *Use single quotes when providing a literal value of type `char`*

- `StringBuilder` method `setCharAt` requires second argument to be of type `char`
    - $\implies$ Used single quotes on `o` when we called the method above

# Mutating Strings vs. New Strings

- `String` is immutable
- `StringBuilder` is mutable
- Constructing a new object is slower than modifying an existing one!

```java
String a = "Joshua";
String b = "Giraffe";
a = a + b; // Constructs a new String -- slow.
```

```java
StringBuilder c = new StringBuilder("Baby");
StringBuilder d = new StringBuilder("Beluga");
c.append(d); // Mutates -- faster.
```
