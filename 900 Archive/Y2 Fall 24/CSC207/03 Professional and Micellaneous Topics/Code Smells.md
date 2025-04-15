---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 11
Module:
  - "[[3 - Professional and Miscellaneous Topics]]"
Lecture:
  - "22"
Chapter: 
Slides/Notes:
  - "[[22-CodeSmells.pdf]]"
Date: 2024-11-21
Date created: Fri., Nov. 22, 2024, 3:24:56 am
Date modified: Fri., Nov. 22, 2024, 6:58:00 pm
---

> [!abstract]+ “Code smells”
> - Analogy: When the refrigerator smells, the food might not have gone bad yet. But it will soon!
> - Same idea with code; when there is a code smell,
>     - Program isn’t bad yet, but
>     - More code you add, the more it will have to accommodate the “smelly” code, causing your program to be more and more difficult to handle.
> - ==Code needs to be **refactored** and fixed==

- Some problems can be fixed any time, e.g.,
    - An inconvenient variable name can be changed using “search and replace”
    - File path leading to a CSV file where you store the data can be changed at any time
- Code smells should be fixed as soon as you see them
    - To avoid extra work later

# Refactoring

> [!def]+ Refactoring
> - A systematic process of improving code without creating new functionality that can transform a mess into clean code and simple design
>     - e.g., Apply a **[[Design Patterns|design pattern]]**

- **Clean Code** is
    - ==Well-formatted==
    - ==Well-documented==
    - Contains ==good variable names==
    - Does not contain duplicate code
    - Has nice small methods
    - Reasonably sized classes
    - No magic numbers
    - Passes all its tests

# Categories of Code Smells

- Bloaters
    - Too much code
- Object Orientation Abusers
    - Know what OOP is and how to define an (abstract) class, interface, etc., but
    - There are right and wrong ways to use these things
        - e.g., Introducing inheritance when there is no “is” relationship,
        - Introducing composition when it is not clear that one object should have an instance of another
    - Can be improved by changing use of inheritance, composition, or by redistributing responsibilities
- Change Preventers
    - Features that make it difficult to extend or update the code
- Dispensables
    - Things that can be deleted or combined
- Couplers
    - Opposites of “lessening dependencies”

## Fixes

> [!tip]+ The solution to a code smell is almost always **refactor**
> - **Composing methods**
> - **Moving features between objects**
> - **Organizing data**
> - **Simplifying conditional expressions**
> - **Simplifying method calls**
> - **Dealing with generalization**
> - See [refactoring.guru/refactoring/techniques](https://refactoring.guru/refactoring/techniques)

# Long Method: Example of a Problem and Solutions

- See [refactoring.guru](https://refactoring.guru/smells/long-method)

> [!warning]+ Signs and Symptoms
> - A method contains too many lines of code
> - Generally, ==any method longer than ten lines should make you start asking questions==

> [!hint]+ Reasons for the Problem
> - Mentally, it is harder to create a new method than to add to an existing one
>     - “But it’s just two lines! There’s no use in creating a whole method just for that…”
> - → Then another line is added, and then yet another, giving birth to a tangle of spaghetti code

> [!tip]+ Possible Fixes
> - **Extract method** to make original method shorter
> - **Replace temp with query**, **introduce parameter object**, **preserve whole object** if variables or parameters make it difficult to extract method
> - **Replace method with method object** to encapsulate the method (if you cannot make it smaller)
> - If an if-statement prevents such a move, use **decompose conditional**
> - If a loop prevents the move, use **extract method**

# Refactoring and the Open Closed Principle

Recall:

![[Solid Design Principles#^dfe828]]

- We should write code in a way that lets us change as few things as possible when adding new features
- Refactoring amounts to making changes to the code

> [!question]+ Does that go against the principle?
> - We use refactoring *now* to get the code to a point where we can avoid making too many changes when we add new features *later*

- It is okay to change now during its development
- Very often when you design things,
    - Can design it such that even if you refactor, you only refactor implementation details
    - Public interfaces remain the same
        - → Consistent with OCP; did not change anything the user directly depends on

# When to Refactor?

- Whenever you see a place where your code could be improved, or when you identify a ==code smell==
- When you find yourself doing the same thing for the third time
- When trying to understand someone else’s code, but it is confusing
- When trying to fix a bug, but the code is difficult to follow
- During or immediately after a code review, when conducted with at least one of the authors of the code

## Adding New Features ≠ Refactoring

- When you refactor, the ==functionality of your code should not change==
    - Refactoring should ==change the readability and understandability== of your code
- If you refactor before adding new features, it should be easier to add the new features
    - Working “smart” now can save you hard work later

> [!goal] Make it *easier* to grow your program, and *save time* for you and your team
