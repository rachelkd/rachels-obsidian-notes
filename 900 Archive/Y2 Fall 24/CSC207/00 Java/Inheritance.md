---
dg-publish: true
tags: ["#cs", "#java", "#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 4
Module:
  - "[[Java]]"
Date: 
Chapter: 3.1
Date created: Sat., Oct. 5, 2024, 11:16:30 pm
Date modified: Tue., Dec. 10, 2024, 4:42:14 pm
---

- See [[Week 3 - Interfaces, Generics, and the Java Collections Framework]]

```java
class Child extends Parent {
    ...
}
```

- Class `Child` inherits all of the methods and variables defined in `Parent`
- `Child` is an *instance of* `Parent`
- “Parent/Child” and “Superclass/Subclass” are interchangeable

# Abstract Classes

- **Abstract classes**
    - Classes that are not meant to be initialized
    - Any non-abstract class that *extends* an abstract class has to ==implement the body of all abstract methods==
- Use `abstract` keyword to signify that a class is abstract
    - Enforces that ==no instance of the class should be created==, even if there are no abstract method sin the class
- Use `abstract` keyword for any abstract *methods*

```java
abstract class AbstractClass {
    abstract void something();    // Abstract methods have no body!
}
```

- Non-abstract class *extends* an abstract class → Has to implement the body of all abstract methods

```java
class NonAbstract extends AbstractClass {
    void something() {
        ...   // Method body here!
    }
}
```

# Overriding Methods

- How to override methods in Java:
    - Re-define parent class’ methods
    - Include an `@Override` annotation
        - Informs compiler that method is meant to override an element in a superclass
        - Not required, but helps prevent errors (e.g., misspelling name of method, forgetting parameter, etc.)

Consider the following parent class:

```java
class Parent {
    void something(){
        ...    // Some method body here!
    }
}
```

We can override the method `something` as follows:

```java
class Child extends Parent {
    @Override
    void something(){
        ...    // Our new method body here!
    }
}
```
