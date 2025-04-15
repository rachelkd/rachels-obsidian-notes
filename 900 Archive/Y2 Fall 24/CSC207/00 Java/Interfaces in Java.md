---
dg-publish: true
tags: ["#cs", "#java", "#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 4
Module:
  - "[[Java]]"
Date:
Chapter: 3.2
Date created: Sat., Oct. 5, 2024, 11:25:12 pm
Date modified: Tue., Dec. 10, 2024, 4:47:33 pm
---

- In Java, you can only *extend* a single class ([[Inheritance]])
    - Have one parent class and that’s it
- Sometimes want to describe more behaviours for a class in a way that just one parent will not suffice

Consider writing a program to simulate plants.

- Could have a class called `Plant`
    - All `Plant`s are able to `breathe` and `grow`
- Could have subclasses `Wheat` and `Flower` in their own subclasses
- However, suppose we want to indicate that some plants are edible for humans
    - e.g., `Corn` and `Basil` would have an `eat` method
- Not all plants are edible → Cannot add method to `Plant`
- Could *define* an `EdiblePlant` class, but would also need `EdibleWheat`, `EdibleFlower`, etc.
    - → Not clean solution!

<!-- break -->
- If we want to *define a property* of a class → Use **interfaces**
    - Similar to classes, except
    - Traditionally, ==no implementation details==; only *method signatures*
        - As of Java 8, we can have `default` implementations of methods
        - ==Use `default` keyword==
    - Can also have variables, but must be `public`, `static`, and `final`

```java
interface Edible {
    void eat();
}
```

```java title:"Interface with default implementation"
interface Washable {
    // Default method that provides a basic implementation
    default void wash() {
        System.out.println("Washing the edible item...");
    }
}
```

```java
class Corn extends Plant implements Edible, Washable {
    void eat() {
        ...    // Our implementation here!
    }

    // Overriding the default wash method
    @Override
    public void wash() {
        System.out.println("Thoroughly washing the corn...");
    }
}
```

*Use the `implements` keyword.*

- Some food can be steamed, so we might want a `Steamable` interface
- `Steamable` foods $\implies$ is `Edible`

```java
interface Steamable extends Edible {
    void steam();
}
```

*Any class that `implements Steamable` must then have a `steam` and `eat` method.*

- Possible for an interface to extend multiple interfaces!
