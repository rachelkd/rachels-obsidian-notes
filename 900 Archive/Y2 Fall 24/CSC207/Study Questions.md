---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 12
Module:
Lecture:
  - "24"
Chapter:
Slides/Notes:
  - "[[24-StudyQuestions.pdf]]"
Date: 2024-11-28
Date created: Mon., Dec. 2, 2024, 2:36:40 am
Date modified: Wed., Dec. 25, 2024, 6:44:46 pm
---

# Study Questions

## Clean Architecture

We spent a lot of time referring back to the “typical scenario” of Clean Architecture. Roughly draw the diagram and briefly explains how it works — starting from the input entering the system from the UI and ending with something being displayed to the user. You can omit the part relating to the database. (4 marks)

- You have an **Interactor**
    - Where the real work happens
- Gets its input from the **Input Data**
    - Input Data is passed into the **Input Boundary** interface
- ? What implements the Input Boundary in CA?
    - Interactor
- Input Boundary specifies what method to call in order to run the use case (in the Interactor)
- On the way out, Interactor creates Output Data and hands it to the **Presenter**
- ? What does the Presenter implement?
    - The **Output Boundary**
- Output Boundary specifies the method that the use case is going to call once it is done processing and wants to show something to the user

<!-- break -->
- We start at the **View**
    - View gathers information from the input fields
    - Tells the **Controller**: “Here’s the data from the user. Could you please package it nicely for the Interactor”
- Controller packages the data from the View → Puts it in an **Input Data** object → Passes it to the Interactor *through* the **Input Boundary**
- **Interactor** has a task → Will do that task, but before it can do the task, it needs **Entities**
    - Needs to get data from the Data Layer (DAO) through the **Data Access Interface**
- **Data Access Object** returns the Entity → Use Case Interactor *manipulates* those Entities
- When Interactor is done, needs to present stuff to the user → Wraps data in an **Output Data** object → Passes to the **Presenter** through the **Output Boundary**
- Presenter is going to update the **View Model** with this new information
    - Because the ==View is observing the View Model==, the **View** is going to be updated as well

<!-- break -->
- Interactor is expecting data
- Data is going to come from the View, but the Interactor cannot look at that directly
    - Violation of **Dependency Rule**
    - Always have to go inwards
- Controller is packaging up the data into an Input Data object, and passing it into the Interactor
    - Interactor has said: “This is the important method to call to execute me”
    - Specifies that in the **Input Boundary**
- & Input Boundary exists to tell the Controller how to start the use case
- Whenever you make an Interactor, you have to say:
    - “Here’s how to call me”
        - Input Boundary
    - “Here’s the data that I expect”
        - Input Data
    - “Here’s the Presenter method that I am going to call when I am done”
        - Output Boundary
    - “Here’s the Output Data that I am going to send to the Presenter”
        - Output Data
    - In between, it needs access to Entities
        - → Talk *through* the Data Access Interface to the Data Access Object to get these Entities

> [!question]- What does the **Dependency Inversion Principle** say?
>
> - High level classes should *not* depend on low-level classes
> - In fact, both should depend on abstractions
>     - These abstractions are the Input Boundary and Output Boundary
>     - Use Case Interactor knows nothing about the outside
>         - Only knows: “Here’s how to call me,” “Here’s what I am going to call when I am done,” “I need to get Entities — here are the ones that I want”
>             - Says that in the Data Access Interface

## Java

> [!goal]+ To Study:
>
> - Basic features of a Java class
> - Contents of the quizzes
> - Labs

> [!question]- What does a constructor do?
>
> - Initialize the instance variables

> [!question]- Do you have to include a constructor in every class? What happens if you *don’t* write a constructor for a class
>
> - & Default constructor
>     - No parameters
> - ? What’s in the method body
>     - `super`
> - Default constructor does not initialize any variables locally
>     - *Does* call the inherited constructor

> [!question]- If you write a constructor, do you get the default constructor?
>
> - No
>     - Java only makes a default constructor if you do not provide one
>     - Simply because you need to construct an object
>     - Need to make sure you’ve got your inherited part constructed

> [!question]- Does the constructor have to call `super`?
>
> - Constructor’s relationship with `super`:
>     - If the class extends another class (has a parent/superclass):
>         - `super` must be called if:
>             - Parent class does not have a no-argument constructor
>             - You want to call a specific parent constructor
>         - `super` can be omitted if:
>             - Parent class has a no-argument constructor
>             - Java will implicitly add `super` call
>     - If the class does not extend any class (besides Object):
>         - No need to call `super`
>         - Java implicitly extends Object which has a no-arg constructor
>     - Key rules:
>         - If called, `super` must be first statement in constructor
>         - Cannot access instance variables/methods before `super`
>         - Cannot use `this` before `super`
>     - Common mistakes:
>         - Forgetting `super` when parent has parameterized constructor
>         - Calling `super` after other statements

> [!question]- What are the “Java lookup rules”?
>
> - Process of finding methods/fields when using dot notation (e.g., `obj.method`)
>     - Start at declared type of reference variable
>     - Search process:
>         1. Look in current class for exact match
>         2. Look for compatible matches in current class
>         3. Look in superclass for exact match
>         4. Continue up inheritance chain
>         5. If nothing found, compilation error
>     - For method calls:
>         - Must match method name
>         - Parameters must be compatible
>         - Return type must be compatible
>     - For fields:
>         - Direct name match
>         - Type must be compatible
>     - Important notes:
>         - Compile-time: Uses declared type
>         - Runtime: Uses actual object type
>         - Private members not visible to subclasses
>         - Protected/package members visible within package
>     - Dynamic Dispatch (Runtime Polymorphism):
>         - For overridden methods, Java uses actual object type
>         - Example: `Shape shape = new Rectangle()`
>             - Compile-time: Checks if method exists in Shape
>             - Runtime: Calls Rectangle’s version if overridden
>         - Only applies to methods, not fields
>         - Static methods use declared type (no dynamic dispatch)
>             - Example: `Shape shape = new Rectangle()`
>             - Static method call `shape.staticMethod` uses Shape’s version
>             - Better style: Use class name `Shape.staticMethod`

> [!question]- What makes a class *generic*? Abstract? When should you use generics? When should you use abstract classes and methods?
>
> - Symbol for generic
>     - `< >`
> - e.g., `List<T>`
>     - `T` is a type variable
>     - Can fill in `T` when you *instantiate* an object
>     - e.g., `List<String>`
> - Behind the scenes, it makes you a whole copy of the class and instantiates it with that type filled in everywhere
>     - → Do not have to cast every time
> - When to use generics:
>     - When class/method needs to work with different types
>     - Common use cases:
>         - Collections (lists, maps, sets)
>         - Methods that operate on different data types
>         - Classes that need type flexibility
>     - Benefits:
>         - Type safety at compile time
>         - Avoid casting
>         - Code reusability
>     - Examples:
>         - Pair class that holds two items:
>
>             ```java
>             class Pair<T, U> {
>                 private T first;
>                 private U second;
>             }
>             // Usage: Pair<String, Integer> grade = new Pair<>("Alice", 95);
>             ```
>
>         - Generic method to find max value:
>
>             ```java
>             public static <T extends Comparable< T >> T findMax(T[] array) {
>                 T max = array[0];
>                 for (T item : array) {
>                     if (item.compareTo(max) > 0) max = item;
>                 }
>                 return max;
>             }
>             // Usage: Integer[] nums = {1, 2, 3};
>             // Integer max = findMax(nums);
>             ```
>
> - & Make a class *abstract* when you do not want people to instantiate it
>     - Want them to make a subclass and instantiate that
> - ? Does an abstract class have to have an abstract method?
>     - No
>     - Can have an abstract class that does not have any methods
>     - Abstract only means no one can make a new one directly
>     - Need to do so through inheritance to make a subclass
> - ? If you have an abstract method, what does it mean about your class?
>     - Subclass has to implement
> - ? Does your class have to be abstract if it has an abstract method?
>     - Yes
>     - Converse is not true; can have abstract class that implements methods

> [!question]- How can you choose between interfaces and abstract classes?
>
> - Key differences:
>     - Multiple inheritance:
>         - Can implement *multiple* interfaces
>         - Can only extend *one* abstract class
>     - State and implementation:
>         - Interface: Only constants (static final instance variables)
>         - Abstract class: Can have instance variables and constructor
>     - Method implementation:
>         - Interface: All methods abstract by default (except default methods)
>         - Abstract class: Can mix concrete and abstract methods
> - When to use interface:
>     - Defining a contract/behaviour
>     - Need multiple inheritance
>     - Want to specify “what” without “how”
>     - Classes from different hierarchies need same behaviour
>         - So you can provide some sort of service for a class with that behaviour
>     - Examples:
>         - `Comparable`, `Runnable`, `Iterable`
>         - `InputBoundary`, `OutputBoundary` provide some sort of service in CA engine
> - When to use abstract class:
>     - Need to share code among related classes
>     - Want to provide a base implementation
>     - Need to declare non-public members
>     - Need to maintain state
>     - Examples:
>         - `AbstractList`, `AbstractMap`
> - Implementation requirements:
>     - Interface: Must implement all methods (or be abstract)
>     - Abstract class:
>         - Must implement all abstract methods, or
>         - Subclass must also be abstract
>     - Can override concrete methods (except `final`)
> - Modern Java features:
>     - Interfaces can have:
>         - Default methods (with implementation)
>         - Static methods
>     - Still cannot have state/constructor
>
> If you have an abstract class,
>
> ```java
> abstract class A {
>     void m;
>     abstract void t;
> }
> ```
>
> - If you write a subclass of `A`, you have to provide an implementation of method `t`, or
>     - Subclass also has to be abstract
> - Can override `m` in your subclass
>     - Except when the method is `final`

### Notable Aspects of Java

- Types
- Primitives
- Casting
- Generics
- Exceptions
- Interfaces
- Overloading
- Memory model
- JUnit
- Javadoc
- Constructors
- Access modifiers
- Packages

## Git

> [!goal]+ To Study:
>
> - Basic commands
> - Resolving conflicts
> - Branch
> - Merge

> [!question]- What are some benefits of using version control?
>
> - Everybody has their own copy of the repository
> - Can work on their own feature and make progress without clobbering the work that anyone else is doing
> - Makes file sharing, isolation of work easier
>     - Through branching and merging

> [!question]- What are some best practices that you have noticed when using git?
>
> - Lots of small commits
>     - Easier to track changes
>     - Easier to revert if needed
>     - Better for code review
> - Pull before pushing
> - Create new branch for each feature
> - Test code before committing
> - Keep main/master branch stable

> [!question]- What makes a commit message *good*?
>
> - Clear and concise
> - ==Describes what changed and why==
> - Uses present tense
> - References issue number if applicable
> - Specific about changes made
> - Bad examples:
>     - “fixed bug”
>     - “changes”
>     - “update”
> - Good examples:
>     - “Fix login validation error”
>     - “Add user profile page”
>     - “Update database schema for new features”

> [!question]- What is a branch and how can you use it to benefit your group’s project?
>
> - Separate workspace for development
> - Allows parallel work without interference
> - Good for:
>     - Developing new features
>     - Testing experimental changes
>     - Fixing bugs
> - Benefits group work by:
>     - Preventing code conflicts
>     - Enabling code review
>     - Making it safe to experiment
>     - Keeping main branch stable

## Testing

> [!goal]+ To Study:
>
> - Testing-related definitions
> - Basics of JUnit

> [!question]- What is the difference between end-to-end, integration, and unit tests?
>
> - Unit test tests one class
>     - Have to mock the other classes to make sure that one unit is working correctly
> - Integration tests two different layers
>     - Run a unit test with a *mock* → unit test
>     - Run the same test with the actual class e.g., Data Access Object and Interactor → integration test
>     - One can even argue that an end-to-end test is an integration test, because it integrates everything

## Exceptions

> [!goal]+ To Study:
>
> - When to throw
> - Syntax
> - Inheritance hierarchy
> - Checked vs. unchecked

> [!question]- What syntax makes an Exception checked? Unchecked?
>
> - `throws`
