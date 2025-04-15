---
dg-publish: true
tags: ['cs', 'java', 'lecture', 'note', university]
Course Code:
  - '[[CSC207]]'
Week: 1
Module:
  - '[[Java]]'
Date: 2024-10-05
Chapter: 1.7
Date created: Sat., Oct. 5, 2024, 3:07:30 pm
Date modified: Tue., Dec. 10, 2024, 2:48:13 am
---

# Aliasing and Its Implications

- Java has two kinds of types: primitive and reference types
    - [[Reference Types and Primitive Types]]
    - In reference types: Follow reference to get to the values and methods stored in the object

## With References, We Can Create Aliases

- **Aliasing**:
    - Creating an object allows two different variables to refer to the same object

### Example in Java

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
```

- After execution, memory state:
    - ![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-1.png)

### Aliasing Definition

- When two variables reference the same object, they are aliases
- Compare to dictionary definition: an alias is when a person is known by different names
    - Example: “Eric Blair, alias George Orwell”

### Importance of Understanding Aliasing

- Awareness of variable types (primitive or reference) is crucial
- Can completely change the behaviour of code execution

# With Primitives, We Cannot Create Aliases

```java
int number = 42;
int answer = number;
```

![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-2.png)

- No objects are created
- ==No references are created==

# Side-effects of Aliasing

> [!caution] If we have two references to the same object, we have to be aware of this or our code will do things that surprise us!

Suppose we have a class called `Monster`, and it has methods called `grow` (to make a monster bigger) and `size` (to find out how big a monster is).

```java
// Create a Monster of size 12.
Monster one = new Monster(12);
Monster two = one;
// Increase the size of Monster two.
two.grow();
// Monster two now has size 24.  How big is Monster one?
int n = one.size();
```

- State of memory right before the call to `grow`:
    - ![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-3.png)
- State after:

    - ![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-4.png)

- `one` and `two` are aliases $\implies$ changing `two` affects `one`
- Changing the object that `two` refers to changes the object that `one` refers to

## Side Effects only Work if Object is Mutable

Suppose we have aliases for an object that is immutable.

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
```

- No way to create a side effect
    - `String` object cannot be changed
    - Best we can do is make a new `String` object. For example:

```java
String name = new String("Justin Trudeau");
String primeMinister = name;
primeMinister = primeMinister.replace('u', 'U');
System.out.println(name);
System.out.println(primeMinister);
```

```
Justin Trudeau
JUstin TrUdeaU
```

- Did not change `name` by changing `primeMinister`
    - In fact we didn’t change the `String` that `primeMinister` refers to
    - We made a *new* `String`

# Making a Copy to Avoid Side-effects

- When two variables are aliases for the same mutable object, side-effects can occur
- To avoid side-effects, make a copy of the object instead of an alias

## Example: Copying an Array

```java
String[] words = {"hello", "bonjour", "adieu", "tschuss", "ciao"};
// This tries to be a copy, but it's not; it's an alias.
String[] copy1 = words;
copy1[1] = "xox";

String[] copy2 = new String[words.length];
for(int i = 0; i < words.length; i++) {
    copy2[i] = words[i];
}
copy2[3] = "yoy";
```

![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-5.png)

- `copy1` is not a true copy, but another reference to the original array
- Changes to words affect `copy1` and vice versa (they are the same array)
- `copy2` refers to a new array object
    - Each element of `copy2` contains a copy of an element from the original array
    - Changes to `copy2` do not affect the original array (`words`)

# Shallow Copy Vs Deep Copy

- Above, we made a **shallow copy**
- Copied references from `words` to `copy2`, not the objects themselves
- `words` and `copy2` are separate arrays but point to the same `String` objects

## Aliasing at a Deeper Level

- `words[4]` and `copy2[4]` both refer to the same `String` with ID `id14`
- No issues with immutable `String` objects
- Potential problems with mutable objects like arrays or `Monster`s

## Example with Mutable Objects

```java
int[][] table = {
    {11, 22},
    {5, 10}
};

int[][] copy = new int[2][2];
for (int i = 0; i < table.length; i++) {
    copy[i] = table[i];
}
int[] row = {0, 0};
copy[0] = row;
// Changing copy[0] did not change table
// What happens if we do this?
copy[1][1] = -99999;
```

## Memory Model Explanation

![](https://github.com/CSC207-UofT/207-course-notes/raw/master/images/1.7-6.png)

- Changing `copy[1][1]` also changes `table[1][1]` to -9999999
- To avoid this, make a copy at every level (deep copy)

# Other Kinds of Side Effects

- Side effects can occur when passing parameters to methods
- Understanding primitives vs references in the memory model is crucial
- Similar to Python: modifying a list passed as a parameter modifies the original
