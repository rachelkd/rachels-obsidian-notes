---
dg-publish: true
tags: ['lecture', 'note', cs, university]
Course Code:
  - '[[CSC207]]'
Week: 5
Module:
  - '[[2 - Principles of Software Design]]'
Date: 2024-10-01
aliases: [SOLID]
Date created: Tue., Oct. 1, 2024, 7:37:05 pm
Date modified: Tue., Dec. 10, 2024, 6:49:10 am
---

> [!goal]- Learning Outcomes
>
> - Know what the five SOLID design principles are

- What are the five **SOLID** design principles?
    - **S**ingle responsibility principle (SRP)
    - **O**pen/closed principle (OCP)
    - **L**iskov substitution principle (LSP)
    - **I**nterface segregation principle (ISP)
    - **D**ependency inversion principle

# SOLID (Principles of Class Design)

## Summary

- SRP
    - Single Responsibility Principle
    - A class should have one, and only one, reason to change
- OCP
    - Open Closed Principle
    - You should be able to extend the behaviour of a class without modifying the class
- LSP
    - Liskov Substitution Principle
    - Derived classes must be substitutable for their base classes
    - i.e., Subclass better work everywhere the superclass works; shouldn’t remove ability
- ISP
    - Interface Segregation Principle
    - Make fine grained interfaces that are client specific
    - i.e., Want to have interfaces that are quite small
- DIP
    - Dependency Inversion principle
    - Depend on abstractions, not on concretions
    - i.e., Take this and do some magic, and all of a sudden, this dependency is removed

## Single Responsibility Principle

> [!def]- Single Responsibility Principle
>
> - A class should have **one, and only one, reason to change**
> - Every class should have a single responsibility

- Who causes the change?
    - An **actor**
        - User of the program, or stakeholder, or a group of such people, or an automated process

> “This principle is about people. … When you write a software module, you want to make sure that when changes are requested, ==those changes can only originate from a single person, or rather, a single tightly coupled group of people representing a single narrowly defined business function==. You want to ==isolate your modules from the complexities of the organization as a whole==, and design your systems such that ==each module is responsible (responds to) the needs of just that one business function.==”

### A Story of Three Actors

- Domain: an `Employee` class from a payroll application

    - ![|100](https://i.imgur.com/GWVcqvD.png)

- Who is responsible for `calculatePay`?
    - HR, Accounting; Chief Financial Officer
- Who is responsible for `reportHours`?
    - HR; Chief Operations Officer
- Who is responsible for how information is `save`d?
    - IT, Databased administrators; Chief Technical Officer

Suppose methods `calculatePay` and `reportHours` share a helper method to calculate `regularHours` and avoid duplicate code.

- CFO decides to change how non-overtime hours are calculated → dev makes the change
- COO does not know about this: what happens?
    - Breaking how hours are reported

### Cause of Problem and Solution

- Cause of problem:
    - Code is “owned” by more than one actor
- Solution:
    - Adhere to Single Responsibility Principle
        - Factor out the data storage into an `EmployeeData` class
        - Create three separate classes, one for each actor
        - ![|300](https://i.imgur.com/ydmgztz.png)

### Façade Design Pattern

- Downside of solution:
    - Need to keep track of three objects
    - NOT one!
- Solution:
    - Create a **façade**
    - Create an Employee Facade that has three instance variables
        - One for `PayCalculator`, `HourReporter`, and `EmployeeSaver`
    - ![|400](https://i.imgur.com/Vsu5qjd.png)

Every method in the facade will look like:

```java
public double calculatePay(...) {
    payCalculator.calculatePay(...)
}
```

- All three methods are one line long
- Facade delegates to the appropriate object
- Responsibilities are still covered by the three objects in the middle
- Can pass an instance of the facade around, and it is going to carry the other things with it in its instance variables
    - i.e., it *delegates* to the appropriate class

> [!important]+ We want each of these responsibilities to be in its own class
>
> - The proposed `regularHours` helper method is going to cause problems!

## The Open/Closed Principle

> [!def]- Open/Closed Principle
>
> - Software entities (e.g., classes, modules, functions, etc.) should be **open for extension, but closed for modification**

^dfe828

- Add new features — *not* by modifying original class — but rather by:
    - Extending it and adding new behaviours, or by adding plugin capabilities
    - i.e., We’re adding behaviour, not breaking behaviour

### Example, Using Inheritance

![|300](https://i.imgur.com/n9Bxi87.png) ![|300](https://i.imgur.com/nWDrSkW.png)

- `area` method calculates the area of *all* `Rectangle`s in the given array
    - ? What if we need to add more shapes?
    - → Might want to make it work for circles too
- Could implement a `Circle` class and *rewrite* the area method to take in an *array of `Object`s*
    - use `isinstance` to determine if each `Object` is a `Rectangle` or a `Circle` so it can be cast appropriately
    - ![|300](https://i.imgur.com/LXhMJJt.png)
    - ![|300](https://i.imgur.com/1Q3esAz.png)
    - ? What if we need to add more shapes?

![|600](https://i.imgur.com/jGhX1SP.png)

- `Shape` is an **[[Interfaces|interface]]** (Class name is in italics)
- `AreaCalculator` is providing a service:
    - Calculate the total area of all the shapes in the `shapes` array
- & With this design, we can add any number of shapes (open for ext.)
    - Don’t need to re-write the `AreaCalculator` class (closed for modification)

<!-- break -->
- ? Where does a class `Square` fit into this hierarchy?
- Can it extend `Rectangle`?
    - No.
    - `Square` has an extra representation invariant that `width == height`
    - `Rectangle`’s `setWidth` and `setHeight` methods can break that
    - $\implies$ `Square` = own class that only implements `Shape` interface

## Liskov Substitution Principle

> [!def]- Liskov Substitution Principle
>
> - If `S` is a subtype of `T`, then objects of type `S` may be substituted for objects of type `T`, without altering any of the desired properties of the program

- `S` is a subtype of `T` means:
    - `S` is a child class of `T`, or
    - `S` *implements* interface `T`

> “A program that uses an interface must not be confused by an implementation of that interface.”
> — Uncle Bob

- i.e., A `List` is not an `ArrayList`; an `ArrayList` is a `List`

### Example. Is a Square a Rectangle?

![|center|300](https://i.imgur.com/3kj9wQw.png)

- Mathematically, a square “is” a rectangle
- In OOP design, that is *not* the case!
- Square is *not* a rectangle
    - `Rectangle` has *more* behaviours than a `Square`, not less
    - Rectangles can vary width and height
    - Square can only vary both simultaneously
        - Rectangles: Can do so independently

> [!important]+ LSP is related to the [[#The Open/Closed Principle|Open/Closed principle]]
>
> - Subclasses should only *extend* (add behaviours), not modify or remove them

- In the [[Java Collections Framework|JCF]], all of these have immutable and mutable versions
    - → In a hierarchy
    - `List` interface does not talk about `add` being immutable
    - What happens when you get an immutable version of `List`?
        - In these immutable versions, they throw new unsupported operation exception in anything that mutates the list
        - [c] Sounds like removing behaviour
        - Java designers did it anyway
            - [p] Other design factors (lightweight, small set of core classes, clarity) outweighed this
            - Explicitly mentioned that this violates Liskov in documentation
- Sometimes there are other design considerations

## Interface Segregation Principle

- What does **interface** mean in this context?
    - The public methods of a class
    - In Java, these are often specified by defining an interface → other classes then implement
- A class that provides a *service* for other “client” programmers usually requires:
    - That clients write code that has a particular set of features
- Service provider says:
    - “Your code needs to have this interface”
- e.g., `AreaCalculator` for `Shape`s provides a service
    - Client programmers who want to use it need to use `Shape`s that implement the interface
    - i.e., Client programmers’ new shapes have to implement the `area` method

<!-- break -->

- & No client should be forced to implement irrelevant methods of an interface
    - ==Better to have lots of small, specific interfaces== vs. fewer larger ones
    - → Easier to extend and modify design
- & Keep interfaces small so that users don’t end up depending on things they don’t need
    - We still work with compiled languages
    - Still depend upon modification dates to determine which modules should be recompiled and redeployed
    - As long as this true, will have to face the problem that:
        - When module A depends on module B at compile time, but not at run time, changes to module B will force recompilation and redeployment of module A
        - e.g., “If I write a class and you write a class, and in my class, I have an instance of your class, and you change your code, my code has to recompile because I mentioned specifically the name of your class”

## Dependency Inversion Principle

- When building a complex system,
    - Often tempted to define “low-level” classes first
    - Then build “higher-level” classes that use the low-level classes directly
    - e.g., “You write a class that does something I want to make use of → Declare a variable of that type, make a new instance of that class → Have a dependency on your class name (because I mentioned your class name)”
- ! Approach is not flexible!
    - What if we need to replace a low-level class?
    - Logic in high-level class will need to be replaced
    - → Indication of high-coupling

> [!question]- How do we design a program that does not have that dependency?
>
> - We introduce an **abstraction layer** between low-level classes and high-level classes

### Example of Bad Code

![](https://i.imgur.com/4sG4mEm.png)

*HL: high-level, ML: middle-level, LL: low-level*

- Pre-order traversal
    - Do the root, do everything on the left, do everything in the middle, do everything on the right
- If we change LL5 (low-level 5), we might have to change LL3, LL2, Main
    - We don’t want this
- Use the **dependency inversion principle**!

### Goal of Dependency Inversion Principle

> [!goal] Want to decouple our system so that we can change individual pieces without having to change anything more than the individual piece

> [!def]- Two aspects to the Dependency Inversion Principle
>
> - High-level modules should not depend on low-level modules
>     - ==Both should depend on abstractions==
>     - “Nothing here should talk about Swing, nothing here should talk about SQL.”
> - Abstractions should not depend upon details
>     - Details should depend upon abstractions

- How do we invert the source code dependency? ![|300](https://i.imgur.com/BG5zERN.png)
    - `HL1` will call `ML1.F` → Bad; we want to break this flow of control
    - ==Introduce an **interface**==
        - Has same function `F` defined
    - `HL1` depends on the interface
        - `HL1` is only going to know about the interface
        - Does not mention the implementing class `ML1`
    - `ML1` implements that interface
    - ![|300](https://i.imgur.com/OJa5A5E.png)
    - When `HL1` calls method `F`, dotted line shows what actually happens
        - Recall: When we call an instance method, we call the lowest one in the class
    - $\implies$ No longer a source code dependency between `HL1` and `ML1`
- **Dependency injection**
    - We can add a replacement for `ML1` and *inject* it into `HL1` (i.e., pass as parameter)
        - $\implies$ No longer have this dependency problem

### Ex. DIP on OODesign

![](https://i.imgur.com/Ig9rW9g.png)

- A company is structured with managers and workers. The code representing the company’s structure has Managers that manage Workers. Let’s say that the company is restructuring and introducing new kinds of workers. They want the code updated to reflect this change.
- Your code currently has a Manager class and a Worker class. The Manager class has one or more methods that take Worker instances as parameters.
- Now there’s a new kind of worker called SuperWorker whose behaviour and features are separate from regular Workers, but they both have some notion of “doing work”.

![](https://i.imgur.com/xTSDGrO.png)

- How do we make `Manager` work with `SuperWorker`?
    - Rewrite code in `Manager`
        - Add another attribute to store a `SuperWorker` instance, add another setter, update body of `manage()`
- Solution:
    - Create an `IWorker` interface
    - Have `Manager` depend on it instead of directly depending on the `Worker` and `SuperWorker` classes

> [!note] In this design, `Manager` doesn’t know anything about `Worker` nor `SuperWorker`. The code will work with any class implementing the `IWorker` interface and the code in `Manager` does not need to be rewritten.
