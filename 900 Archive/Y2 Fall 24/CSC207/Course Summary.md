---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 12
Module: 
Lecture:
  - "23"
Chapter: 
Slides/Notes:
  - "[[23-CourseSummary.pdf]]"
Date: 2024-11-26
Date created: Sun., Dec. 1, 2024, 4:08:47 am
Date modified: Wed., Dec. 25, 2024, 1:36:51 pm
---

# Course Summary

## Final Exam Advice

- Carefully read *all the words* in each question and looking at how many marks things are worth
- Clearly express yourself using *terminology* introduced in the course
- Write something down for every question
- Be *explicit* in your answers

### Example Question

> [!question]+ Explain why having a Square be a subclass of a Rectangle is an example of a Liskov violation. (2 marks)
> - A minimal answer:
>     - A square is more constrained than a rectangle
> - Need to elaborate: does not look like a 2-mark answer
> - What is a [[Solid Design Principles|Liskov]] violation?
> - How is it more constrained?
> - Would a picture or small code example help?

## Communication

- Should be able to communicate design ideas using
    - Java code
    - Words
    - UML diagrams
- Should be able to read and interpret design ideas from other people in each of the above formats
    - e.g., Write code based on a UML diagram

## Features of Object-Oriented Programming Languages

- **Abstraction**
    - & The process of distilling a concept to a set of essential characteristics
- **Encapsulation**
    - & Process of ==binding together data== with methods that manipulate the data and hiding the internal representation from the user
    - The result of applying *abstraction* and *encapsulation* is (often) a class/object with instance variables and methods that together ==model a concept== from the real world
    - Encapsulation does not necessarily mean that you have hidden information
        - e.g., Class `C` with `public String s` and `public int i` encapsulates these two pieces of data, but hasn’t hidden them
- **Inheritance**
    - & Concept that when a subclass is defined in terms of another class, the features of that other class are inherited by the subclass
        - Enabling reuse of common code
- **Polymorphism**
    - & The ability of an expression (such as a method call) to be applied to objects of different types
        - e.g., A method that takes in a list of integers and does something with that list
            - ? If you call `List.get(3)`, which method belonging to what class gets called here?
            - Trick question: Could be anything that implements List
            - Can be called with an `ArrayList`, `LinkedList`, `SortedList`, `TreeList`
                - Anything that implements the `List` interface
            - This, polymorphically, might end up executing the `ArrayList`, `LinkedList`, `SortedList`, etc. method
            - This call can end up executing any `get` methods

### UML Mentor

- A tool to practice designing systems using UML diagrams to satisfy a written specification
    - Developed by past CSC207 students on the UTM campus
- <https://umlmentor.utm.utoronto.ca/home/>

## SOLID Design Principles

See [[Solid Design Principles#Summary|SOLID]].

- **Single responsibility principle**
    - A class should have one, and only one reason to change
- **Open/closed principle**
    - You should be able to extend the behaviour of a class without modifying the class
- **Liskov substitution principle**
    - Derived classes must be substitutable for their base classes
- **Interface segregation principle**
    - Make fine grained interfaces that are client specific
- **Dependency inversion principle**
    - Depend on abstractions, not on concretions

## Clean Architecture

See [[Clean Architecture]].

- **Frameworks and Drivers**
- **Interface Adapters**
- **Application Business Rules**
- **Enterprise Business Rules**

> [!question]- What goes in Enterprise Business Rules and Application Business Rules?
> - Enterprise:
>     - **Entities**
> - Application:
>     - **Use cases**

> [!question]- What tells you how to call a use case in the interactor?
> - **Input Boundary** (interface)
>     - Says “this is how you invoke this use case”
>     - Has **Input Data** as a parameter
> - Also in Application Business Rules

> [!question]- What does the **Output Boundary** interface tell you?
> - How to present things
> - “This is the method that the interactor is going to call when it is sending information back out”
> - Has **Output Data** as a parameter

> [!question]- What else is in the Application Business Rules layer?
> - **Data Access Interface**
>     - Says “this is what this use case is dependent on”

> [!question]- What goes in the Interface Adapters layer?
> - **Controllers**
> - **Gateways**
>     - e.g., Network connection (not on test)
> - **Presenters**
> - Also, **View Models**
>     - Contains all the information that is showing in the view (in the *state*)
>     - Also maintains a *list of observers* using the [[Design Patterns#Observer Design Pattern|property change support]] design
>     - Extracted — from the view model — the *state* of the view as a separate object from the view model
>         - Could’ve had it as instance variables in view model,
>         - Decided for a nice design choice to extract it in the state object
>         - So, ==**State** is also in Interface Adapters==
>         - Conceptually, all the information of what is displayed belongs to the View Model
>             - → State belongs to View Model
>         - State is a **data structure**
>             - Hence, the `<DS>` in the CA engine photo

> [!question]- What is in the Frameworks and Drivers layer?
> - User interface
> - Database
> - External interface
> - Web

![](https://i.imgur.com/YfL0utb.png)

### CA As Concentric Circles

![|center|500](https://i.imgur.com/7mGKBUW.png)

- Often visualize the layers as ==concentric circles==
- The flow of control is different from the dependencies
    - Interactor *implements* the **use case input port**
    - Controller knows what method to call, but
        - Controller does not depend directly on the interactor! ([[Solid Design Principles#Dependency Inversion Principle|Dependency Inversion Principle]])
        - An interface is introduced (Input Boundary)
    - On the way out when the interactor is done, it talks to the **use case output port**
        - Does not know anything more than that, but
        - There is a Presenter object that implements the output port that the interactor is talking to
    - Flow of control:
        - View → Controller → Interactor → Presenter
        - This flow of control is not so clear in the picture and CA engine

### The CA Engine

![](https://i.imgur.com/iDoupgO.png)

- View updates the View Model to make sure when a user triggers something (e.g., button click), View Model is updated with the appropriate values
- View tells Controller to go make something happen
- Controller talks to Input Boundary (to the Interactor), passing in any useful information in the Input Data
- Interactor will do its thing, possibly asking the Data Access layer — through the Data Access Interface — for entities
    - Data to manipulate
- When it is done, Use Case Interactor *constructs* Output Data, and talks to the Presenter — through the Output Boundary — to update whatever View Models are involved
- Presenter knows which View Models are involved
    - Might be more than one view model
    - e.g., Presenter takes information from Signup View to update the Login View (with the username, so the user does not have to type it again)
    - Updates the View Model
        - Note: Every View Model is observed by at least one view
    - Calls `firePropertyChanged`: Tell all the Views watching the View Model that the View Model has changed

> [!info]+ The View depends on the Controller. What is another way we could have done this to remove the dependency?
> - Currently, View says: “Hey Controller, go execute this use case”
>     - e.g., User clicks a button, go do it
> - Instead:
>     - Controller could have ==observed the View Model==
>     - When user triggers something (e.g., button click), View can tell View Model to let everyone know who is watching (`firePropertyChanged`)
>     - Controller would be a observer, and be told that there is a change → Execute use case
> - The arrow from View to Controller is not absolutely necessary, if we had used the Observer pattern
>     - Then, would only have direct dependency between View and View Model

### Dependency Rule

- Dependence on adjacent layer
    - From outer to inner
- Dependence within the same layer is allowed,
    - Try to minimize coupling
- On occasion, you might find it unnecessary to explicitly have all four layers, but
    - Most certainly want at least three layers
    - Sometimes more than four

> [!important] The name of something (functions, classes, variables) declare in an outer layer ==must not be mentioned== by code in an inner layer.

> [!question]+ How do we go from the inside to the outside then?
> - **Dependency Inversion**
> - An inner layer can depend on an interface, which the outer layer implements
> - The outer layer is injected into the inner layer

### Testing

#### Entities: Unit Testing

- Unit tests for each entity
- Unit tests help show that the program data is being represented properly
- Entities do not depend on the rest of the program, so they are naturally isolated

#### Interactors: Unit Testing

- Given some Input Data, does the Interactor produce the correct Output Data?
- Higher-level unit than for the Entities, but still a unit
- Interactors
    - Receive Input Data from a Controller
    - → Produce Output Data for a Presenter
- Test class serves as the Controller
- Test class needs to *create* a Presenter that checks the Output Data and any Entity and persistence mutation
- DAOs are slow
    - ? How do we isolate the Interactor from the DAOs?

#### Integration Test

  - When we test Interactor with a real DAO → **Integration test**
    - Because you integrate program with data code and use case code
- Can probably reuse the Interactor units tests
- Integration tests can take a long time
    - Usually run less frequently than unit tests

#### End-to-End Test

- Test starting at the View
- Testing the whole system

## Exceptions

### Throwable Hierarchy

![](https://i.imgur.com/z9egA0h.png)

- `Error`
    - Really bad; things you cannot recover from
    - Never need to subclass `Error`
    - When you run out of memory, there is nothing to be done
        - → Let program crash with an `Error`
- `Exception`s:
    - `RuntimeExceptions` and subclasses
        - ==Predictable and preventable==
        - `ClassCastException` can be prevented:
            - Check if the thing is an `instanceof` a class, then cast to that class
        - `IndexOutOfBoundsException`:
            - Take a look at index to see if it is `>= 0` and `< array.length`
    - Direct subclasses of `Exception` are *not* predictable
        - e.g., `IOException`, `FileNotFoundException`
        - You can write an if statement that checks if file exists, then open it
            - Can imagine a case that right after that if statement is true, the user deleted/moved the file
        - Cannot prevent input/network errors

> [!important] Java makes you deal with plain `Exception`s, but not `RuntimeException`s

### Checked vs. Unchecked Exceptions

- Checked Exceptions:
    - A method will include `throws ExceptionClassName` in its declaration
    - Compiler will check to make sure that the Exception is caught by the method that calls it, or the method that calls that method, etc.
    - Need `try-catch` block
- Unchecked Exception:
    - Subclasses of `RuntimeException`
    - Program will throw these without checking to see if it eventually caught
    - Will crash program and print the stack trace to the terminal
    - Can then see where the Exception was thrown and fix the code, so that does not happen again
        - Helpful when debugging
- No exceptions should reach the user in the CA program
    - Need to catch exception and produce an error message for the user
        - e.g., Network errors, file not found, etc.

### Don’t Use Throwable or Error

- `Throwable`, `Error`, and its descendants:
    - Probably not suitable for subclassing in an ordinary program
    - Throwable not specific enough
    - Error and its descendants describe ==serious, unrecoverable== conditions
        - Typically at system level
        - e.g., `OutofMemoryError` (a child of `VirtualMachineError`, which is a child of `Error`)

> [!tip] Throw a *specific* exception, so that the client code can catch that specific exception

## Packaging

![|center|300](https://i.imgur.com/5Aykxr0.png)

> [!question]+ How can you package this code?
> - `OrdersController`
>     - A web controller, something that handles requests from the web
> - `OrdersService`
>     - An interface that defines the *business logic* related to orders
> - `OrdersServiceImpl`
>     - The implementation of the orders service
> - `OrdersRepository`
>     - An interface that defines how we get access to persistent order information
> - `JdbcOrdersRepository`
>     - An implementation of the repository interface that saves information in a database

> [!question]- Which one of these is the interactor?
> - `OrdersServiceImpl`

![](https://i.imgur.com/Ni5Tkzg.png)

> [!question]- Which corresponds to clean architecture?
> - By Layer vs. Inside/Outside:
>     - Interface for orders (in Inside/Outside) turns into OrdersRepository in ByLayer
>     - Just design choices
> - & Inside/Outside corresponds to clean architecture
>     - `Orders` interface tells you how to access the database

## Design Patterns

- **Design Pattern**
    - General description of the solution to a well-established problem
    - Patterns describe the shape of the code, rather than the details
    - A means of communicating design ideas
    - Are not specific to any one programming language

### Loose Coupling, High Cohesion

> [!goal] These are the two goals of object-oriented design

- **Coupling**
    - The interdependencies between objects
    - Fewer couplings → Better
        - Can test and modify each piece independently
- **Cohesion**
    - How strongly related the parts are inside a class
    - *High cohesion*
        - A class does one job, and does it well
    - *Low cohesion*
        - An object has parts that do not relate to each other
