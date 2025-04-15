---
dg-publish: true
tags: ["#cs", "#java", "#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 4
Module:
  - "[[Java]]"
Date: 
Chapter: 3.5
Date created: Sat., Oct. 5, 2024, 11:41:16 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

- All variables have a declared type in Java
- Often want to convert between types used
    - Sometimes a more general superclass is better for one situation, but a subclass is better in another
- **Casting**
    - When we change the type of an object to another
    - Often in order to access more specific functionality

```java
Animal[] animals = {new Cat(), new Dog(), new Axolotl()};

for (Animal a : animals) {
    a.eat();
    if (a instanceof Cat){
        ((Cat) a).purr();
    }
}
```

*We want a `Cat` to `purr` after eating.*

- `(Cat) a`
    - Converts `a` into type `Cat`
    - → Allow use of `Cat` method `purr()`
    - **Downcasting**
        - Casting type of a variable into its subclass
- **Upcasting**
    - `Animal[] animals = {new Cat(), new Dog(), new Axolotl()}`
    - Subclasses were cast into a superclass

# Primitive Conversions

- Can also cast *some* primitives
    - e.g., `int` → `double`, and vice versa

```java
int x = 1;
double y = 1.1;
double double_x = (double) x;
int int_y = (int) y;
```

- When we cast objects, we essentially “re-label” the type
- Otherwise, leave it unchanged
- When we cast primitives:
    - Adjusting the value of the variable itself
    - Changes may potentially be irreversible