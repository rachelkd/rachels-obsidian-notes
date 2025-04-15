---
dg-publish: true
tags: ["#cs", "#java", "#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 4
Module:
  - "[[Java]]"
Date: 
Chapter: 3.3
Date created: Sat., Oct. 5, 2024, 11:41:16 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

- `super` keyword
    - Can use to call parent’s constructor
        - `super()`, `super(a, b, c)`
    - Can use to call a parent’s method
        - `super.method()`

# Constructors with `super`

- If extending another class $\implies$ Java *requires* a call to a superclass’ constructor to be made in the subclass’ constructor
- Call *must* be the very ==first thing== done
    - If no constructor call is explicitly made in subclass’ constructor $\implies$ implicit call to `super()` will be made

```java
class Child extends Parent {
    int attribute1;
    int attribute2;

    public Child(int a, int b) {
        // There's no super() call here, but it's implicit!
        this.attribute1 = a;
        this.attribute2 = b;
    }
}
```

The above code is equivalent to:

```java
class Child extends Parent {
    int attribute1;
    int attribute2;

    public Child(int a, int b) {
        super();  // What happens if Parent has no empty constructor?
        this.attribute1 = a;
        this.attribute2 = b;
    }
}
```

- If `Parent` did not have a constructor that takes no args $\implies$ error would be raised during compilation
- ==Best to explicitly include our `super(…)` calls in our constructors==
