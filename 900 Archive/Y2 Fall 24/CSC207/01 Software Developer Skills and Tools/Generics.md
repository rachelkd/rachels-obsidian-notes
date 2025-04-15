---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 3
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-19
Date created: Fri., Sep. 20, 2024, 1:07:06 pm
Date modified: Tue., Dec. 10, 2024, 9:32:42 pm
---

# Generics (Fancy Type Parameters)

- `class Foo<T>`
    - Introduces a class with a type parameter `<T>`
- `<T extends Bar>`
    - Introduces a type parameter that is required to be a descendant of the class `Bar`
    - with `Bar` itself a possibility
    - In a type parameter, “extends” is also used to mean “implements”
- `<? extends Bar>`
    - Type parameter that can be any class that extends `Bar`
    - Will never refer to this type later ⇒ Don’t give it a name
- `<? super Bar>`
    - Parameter that can be any ancestor of `Bar`

# Ex. Interface with Generics

```java
public interface Comparable<T> {
    /*
     * Compare this object with o for order.
     * Return a negative integer, zero, or a
     * positive integer as this object is less
     * than, equal to, or greater than o.
     */
    int compareTo(T o); // No body at all.
}

public class Student implements Comparable<Student> {
    // Other class members...
    
    public int compareTo(Student other) {
        // Here we need to provide a body for the method.
    }
}
```

# Interface ← Left, Class → Right

- `List<String> ls = new List<>();`
    - This code fails
- `List<String> ls = new ArrayList<>();`
    - Compiles
- Can choose a different implementation of `List` at any point by editing a single line of code

# Naming Conventions

- Java Language Specification recommends these conventions for names of type variables:
    - Very short; single character, preferably
    - Evocative
    - All uppercase to distinguish them from class and interface names
- Specific suggestions:
    - Maps: `K`, `V`
    - Exceptions: `X`
    - Nothing particular: `T` (or `S`, `T`, `U`, or `T1`, `T2`, `T3` for several)
