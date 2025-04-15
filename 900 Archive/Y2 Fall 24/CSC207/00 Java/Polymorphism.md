---
dg-publish: true
tags: ["cs", "java", "lecture", "note", university]
Course Code:
  - "[[CSC207]]"
aliases: []
Week: 4
Module:
  - "[[Java]]"
Date:
Chapter: 3.4
Date created: Sat., Oct. 5, 2024, 11:41:16 pm
Date modified: Mon., Feb. 10, 2025, 6:01:32 pm
---

# Polymorphism

- **Polymorphism**
    - Ability of an object to take many forms
    - Consider an object to be *polymorphic* if it passes multiple `instanceof` tests

```java
class Dog extends Canine implements Domesticatable {...}
```

- `Dog` is also a `Canine` (which might have further superclasses like `Animal`)
- Also `Domesticatable`
- $\implies$ Pass `instanceof` tests for all these types

<!-- break -->
- Example of polymorphism in use:
    - When we have a variable whose value may be of a type other than the variable’s type itself
    - e.g., being a subclass of that type, or if type in question is an interface, a class implements it

```java
Animal[] animals = {new Cat(), new Dog(), new Axolotl()};

for (Animal a : animals) {
    a.eat();    // 'a' in this line of code can have various types!
}
```

*This example exhibits polymorphism.*
