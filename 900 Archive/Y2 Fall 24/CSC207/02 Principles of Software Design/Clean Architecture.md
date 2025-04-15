---
dg-publish: true
tags: [cs, java, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 6
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "11"
  - "12"
Chapter:
Slides/Notes:
  - "[[11-CleanArchitecture.pdf]]"
Date: 2024-10-08
Date created: Thu., Oct. 10, 2024, 7:58:31 pm
Date modified: Fri., Nov. 15, 2024, 5:00:29 pm
---

> [!question]+ Questions to be answered this week
>
> - What is a **use case**?
> - What is **Clean Architecture** (CA)?
> - What is the CA **dependency rule**?
> - What is a **sequence diagram**?

> [!goal]- Learning Objectives
>
> - Become more familiar with terminology related to architecture and design
> - Understand clean architecture and its dependency rule
> - Understand how sequence diagrams can help us convey flow of execution in our program

- [[Solid Design Principles|SOLID Principles]] are design principles
    - Allow us to reason: Is our design good or not?
    - Can use these principles to *derive* **clean architecture**
    - Principles for OOP in general

# Architecture

Brief summary of Ch. 15 from Clean Architecture textbook.

- What do we mean by **architecture**?
    - ==Design== of the system
    - ==Dividing it into logical pieces== and ==specifying how those pieces communicate== with each other
        - Logical pieces e.g., classes, modules
    - Communication → Thinking about input and output between **layers**

> [!goal]+ “Goal is to facilitate the **development**, **deployment**, **operation**, and **maintenance**, of the software system”
>
> - What are we actually going to be doing with this program in the future? Are we designing it so that we can do all these things easily?

> “The strategy behind this facilitating is to leave as many options open as possible, for as long as possible.”

- We want to ==avoid making decisions==!
    - Want architecture to be designed so that we don’t lock ourselves into a specific decision
    - Should be designed so we can make those decisions later
- Sign of bad decision:
    - Have to lock in very specific details early on that do not feel fundamental to what you’re trying to do
    - e.g., Locking into a specific database early on (Maybe you want to use a different database later)
- & Good architecture strives to *maximize* programmer productivity
    - Always keep programmer in mind when we design our software

# Policy and Level

Brief summary of Ch. 19 from Clean Architecture textbook.

> [!def]- Policy
>
> - Describing transformations in our program
> - Given an input, what is the *policy* (i.e., ==logic of the program==) that gives us our output
>     - What does it do with that input?
>     - Think of it like a description of a function

> “A computer program is a detailed description of the **policy** by which inputs are transformed into outputs.”

- Software design seeks to *separate* policies and *group* them as appropriate
    - Ideally form a directed acyclic dependency graph between components
- A policy has an associated **level**
    - The distance from the inputs and outputs
- Some policies are *low-level*
    - i.e., close to the input or output
    - A ==detail== of how we are moving data around
    - Lowest level are those managing inputs and outputs
- Some policies are *high-level*
    - Higher-level policies are farther from the inputs and outputs
    - Policies that describe what matters in our program
    - ==What actually matters==; what we’re actually trying to do

# Use Cases for a Program

> [!def]- Use Case
>
> - A single interaction between the user and computer

Imagine you were asked to write a program that allows users to:

- ==Register a new user account== (with username and password)
- ==Log in to a user account==
- ==Log out of a user account==

<!-- break -->
- Planning to have a few different kinds of accounts, but start with one for now
- Highlighted words are **use cases**
    - ==What will the user want to do?==

1. What data needs to be represented?
2. What data structure might you use while the program is running?
    - As our program is running, we have multiple interactions from our user
    - May register new account, and want to go back and log in
3. What should happen if the user quits and restarts the program?
    - How do we make sure that when the user comes back after quitting, their account is still there?
    - → Persistence of data

## Use Case: User Registers New Account

- & We want to think of our use case as a description of a policy in our program
    - Some parts of policy will be low-level or high-level
    - Core logic of what we are trying to achieve = high-level policy we have

<!-- break -->
- Input:
    - User chooses a username
    - User chooses a password and *enters it twice* (to help them remember)
- Higher-level policy (of when we try to register a new user):
    - **If username already exists** → ==system alerts the user==
    - **If two passwords do not match** → ==system alerts the user==
    - **If username doesn’t exist in system and the passwords match** → ==system creates the use but does not log them in==
    - % These are all things that describe what the program does
    - Highlight: Output
    - Bold, red: Our high level policy

## Use Case: User Logs In

- User enters a username and password
- If username exists in system and passwords match → System shows that user is logged in
- If no such username → System alerts the user
- If password does not match the one in the system → System alerts the user

## Use Case: User Logs Out

- System logs the user out and informs the user

## Use Case: When Logged Out, Choose Use Case

> [!note]+ As we introduce what a use case is, we want to think of it as a specific interaction a user has with the program
>
> - Typically corresponds to some input from user
> - Response from program
> - User being displayed new information

- User chooses between the user *registers* a new account use case and the *user logs in* use case
    - Can think of this as another use case in our program
    - Almost trivial: The user presumably clicks some button → Changes the view for user

## Burning Questions

> [!abstract] If you start to think about how this looks, you almost automatically have to think of the following details

- ==What is the *user interface*?==
    - How are we ==getting the input==, and what does the user see (==display==)?
        - & <u>Emphasis:</u> We should not have to choose UI at this point.
            - If our goal is to design a system that allows user to register → Don’t want to restrict ourselves to one of the four following choices right off the get-go
            - Client may have different ideas about what UI they should have
            - Shouldn’t matter to you as the designed
    - Webpage
    - Java application on computer
    - Python command line program
    - Mobile app
- ==How to do *data persistence*?==
    - How do we store the accounts somewhere?
        - Another detail in the program that we do not have to think about designing right now
    - Text file? JSON?
    - Database?
    - Google Drive/OneDrive/etc.?

> [!goal] We want to design our system so that it is easy to implement these details after the fact.

## Design Conundrums

- How can we design the use cases so that they ==do not directly depend== on the UI and persistence choices?
    - → Then can test all the use cases thoroughly without dealing with input/output
- What are the use case APIs?
    - What is the interface to each use case?
    - What public methods do we want to provide to call the use cases from the UI?
- What persistence methods will we need in any storage?
    - Saving
    - Finding a user by username
    - etc.

# Clean Architecture

## Business Rules

Brief summary of Ch. 20 from Clean Architecture textbook.

- Terminology all centred around **use cases** in our program

> [!def]+ Entity
>
> - An ==object within our computer system== that embodies a small set of Critical Business Rules operating Critical Business Data
> - Objects that represent critical business data (variables) and critical business rules (methods)

- Previously, given a written specification → do noun-verb analysis → come up with *classes* that represent core data in program ([[OOP in Java]], [[Representing Data in Your Program]])
    - We really have been creating a set of **entities** for our system
- Think of entities as our ==domain-specific knowledge==
    - Set of classes that represents all the information we need
    - e.g., User account example: Might have the entity `User` in our program
- These are the very **high-level policies** that we might have in our program
    - What actually matters
    - Represents what we can do in our program

> [!def]+ Use Case
>
> - A *description* of the way that an automated system is used
> - ==Specifies input== to be provided by user, ==output== to be returned to user
>     - Does not actually say anything about *how*, just what
> - Specifies ==processing steps== involved in producing that output
> - Use case describes *application-specific* business rules ==as opposed to the Critical Business Rules within the Entities==

- Application-specific:
    - The interactions that a user can have within the application
- Processing steps:
    - Have sequence of steps → Allows us to leave a lot of details out still
    - May have a list of steps → Does not mean use case is going to do all the work itself
    - → Delegates work to other parts of the program

## Entities

> [!def]- Entities
>
> - Objects that represent critical business data (variables) and critical business rules (methods)

- In CA, **entities** are the:
    - **Highest-level** policies
        - Can think of it like a set of policies
    - ==Core== of the program
- Examples:
    - a `Loan` in a bank
    - a `Player` in a game
    - a set of high scores
    - an item in an inventory system
    - a part in an assembly line

## Use Case

> [!def] Use Case
>
> - Description of the way that an automated system affects **entities**
> - Use cases *manipulate* entities
>     - Making use of our entities (objects in our systems)
>     - Use case make use of these objects, decides which methods to use that are in the entities
>     - → Introduces the idea of **layers** in our system
>         - Use case built on top of our entities
> - Specifies ==input== to be provided by the user, ==output== to be returned to the user, ==processing steps== involved in producing that output

- Think of use case as a *function*/method in our program
    - Inputs, outputs, and implementation logic
- Use cases know *nothing* about the details of the UI or data storage mechanism; just that they exist
    - Need this idea that we *can* get info from outside world; don’t need to worry about how

## Clean Architecture Layers

> [!question]- What are the four clean architecture layers?
>
> 1. **Enterprise Business Rules**
> 2. **Application Business Rules**
> 3. **Interface Adapters**
> 4. **Frameworks and Drivers**
>
> ![](https://i.imgur.com/joXqKc6.png)

- Can think of this as a UML diagram
    - Use cases are going to depend on our entities
    - ![|300](https://i.imgur.com/Sx1wcdI.png)
    - If we stop at these two layers, these two are fully *describing* what our program does (high-level policy)
    - System is a set of use cases → Manipulates entities

> [!abstract]+ In order for people to actually use and interface with our system, we need to build some layers on top of that:
>
> - **Interface adapters**
> - **Frameworks and drivers**

- We get input from the UI → Plug it into our interface adapters → Change it into input for our use cases
- Top two part of our program describes how we transform inputs and outputs to what is expected for our program
    - We define what the inputs/outputs are in our use case
    - Rest of the program needs to be designed for this (hence, *adapters*)
- **Interface adapters** connects us with our actual program
    - **Controllers**:
        - Responsible for getting input from outside world
        - Takes input from frameworks and drivers
        - Converts it to inputs that our use case needs
    - **Presenters**:
        - Get the output from use case/program
        - Turn it into whatever format our framework and drivers need
- $\implies$ Our lowest-level policies are at the top, highest-level at bottom
- ==Dependencies flow from top to bottom==
    - Everything low-level at top *depends* on high-level policies at the bottom

![|500](https://i.imgur.com/4roBsL8.png)

- Often visualize layers as concentric circles
- Reminds us that entities are at the core
- Input and output are both in outer layers
- Bottom right picture:
    - Flow of control in our program: Represents **dependency inversion**
    - Go from input from user → Producing an output (while not violating this dependency rule we introduced in CA)
- We go from outer layer to inner layer, then coming back out (if you look at flow of control)

## Dependency Rule

- All our ==dependencies point inwards==
- Dependence within same layer is allowed
    - But try to minimize coupling
- On occasion, might find it unnecessary to explicitly have all four layers
    - Most certainly want at least three layers and sometimes even more than four
- & Name of something (functions, classes, variables) declared in an outer layer must not be mentioned by code in an inner layer
    - Look at `import` statements to see if you’re violating this
    - e.g., Importing a Controller class from inside an Entity class → <u>Violation of CA</u>
        - And Controller referencing an Entity also likely a violation
    - We do not want any high level policies to depend on low level policies
    - Can use IntelliJ generate a dependency graph for project
- Use **dependency inversion** to go from inside to outside
    - Inner layer can depend on an interface, which outer layer implements

![](https://i.imgur.com/WbzCyLO.png)
*Dependency inversion.*

## Benefits of Clean Architecture

- [p] All the “details” (frameworks, UI, Database, etc.) live in the outermost layer
- [p] Business rules can be easily tested without worrying about those details in the outer layers
- [p] Any changes in outer layers do not affect business rules

## Example of a Program with Clean Architecture

![](https://i.imgur.com/IfosESf.png)

- Plain arrow:
    - Dependency
- Open-headed arrow:
    - Implements

### Enterprise Business Rules

- **Entities**
    - Responsibility:
        - Each entity ==represents something from the problem domain==
            - e.g., Bank account
        - Core data of our program
        - Little logic here other than fancy data structures
            - e.g., Sets, Lists, Trees, and Dictionaries

### Application Business Rules

- Where all the interesting things happen in our program
- Everything that matters for our *application* are in this layer
    - Everything that matters for our *program* are in this layer or Enterprise Business Rules

> [!note]+ **Use case interactor**
>
> - A class that has a method that does all the interesting logic in our program
>     - Makes use of various *interfaces* (labelled `<I>`)
>         - ==Interfaces are defined as part of our **Application Business Rules**==
> - Programming the actual function of the use case
>     - Client has hired us to make a program → Would like to make sure that we can enrol students in a course
>         - Not about the database, not about the UI
>     - Core logic of “enrolling a student” goes in the **use case interactor**
> - The **use case interactor** is the most important box
>     - Represents an *object*

> [!note]+ **Data structures** `<DS>`
>
> - Essentially a Java class with no logic, ==only instance variables, and getters/setters==
> - We have some kind of data that we are going to pass between layers in our system

- These two layers give us the core of what our system is
- At this layer, we have defined the logic of our system
- In order to actually do things, we need to interface with the outside world → *Implementations* of various interfaces

### Interface Adapters

- Typical scenario: The interface between the user and use case interactor
- Most interactions happen in the top left corner of our [[Clean Architecture#Example of a Program with Clean Architecture|diagram]] here
    - ![](https://i.imgur.com/zLAd5zd.png)
- Input coming in from **controller**
- → Controller passes information to our system through **input boundary** interface
- → Information flows out of our system through **output boundary**
- → **Presenter** displays that information to our user
- **View Model** (`<DS>`)
    - Mechanism by which we pass information from the interface adapter layer → the **View**
    - Think of View Model as a model of what *information* the user should see

### Frameworks and Drivers

- **View**
    - ==User interface==
    - Decides what user sees
        - Does that based on what is represented in the **View Model**
        - i.e., View is responsible for making View Model happen; how that information from View Model is going to be displayed
- **Data Access** (and **Database**)
    - Where we access external data in our system
    - For the Use Case Interactor to implement the use case, it may need to get data somewhere
        - Does that through the **Data Access Interface**
        - Implemented by the **Data Access** object
    - Use Case Interactor can ask Data Access object to perform tasks for it ==through the interface==
    - Responsibility:
        - ==Manage access to persistent data==
            - Reading from and writing to files or databases
        - ==Creating temporary Entities== that the Use Case uses to do its work

### How Does Information Go from the Controller to the System?

1. **View**
    - ==User interface== code for a feature
        - Buttons, text areas, scrollbars, panels, windows, etc.
    - Responsibility:
        - Display information and react to user interaction
        - Will ask a Controller to do something the user wants
    - User has entered some input → Control of system is passed into a Controller object
2. **Controller**
    - Essentially an ==action-listener== (recall [[Java Graphical User Interfaces|Java GUIs]])
    - Aside: **Input Boundary** interface
        - Specifies what the Use Case parameter expects the Controller to give it
        - Defines the API for our Use Case
    - Responsibility:
        - ==*Convert* raw user data== to something useful
            - e.g., a String → a Date object, float → currency value
        - ==*Create* an **Input Data**== object containing that info, and
            - Type of parameter that the use case is expecting
        - ==*Call* a method== (defined in our Input Boundary) to start a Use Case, ==*passing in* the Input Data==
            - Typically, `useCase.execute(inputData) -> void`
                - Use Case Interactor does not return anything!
            - We get information from our Presenter
            - ! If `useCase.execute(inputData) -> outputData`, Controller is responsible for both input and output
                - Don’t want to do this!
3. **Use Case Interactor**
    - Contains the logic of implementing the use case
    - Responsibility:
        - Take the Input Data, and
        - ==*Execute* the use case==, ==looking up information in the Data Access== object when necessary and ==manipulating **Entities**==
            - Might create new data that needs to be saved in **Data Access** layer
            - i.e., Call a method from the **Data Access Interface**; request data from a database
        - When complete → Get information back out to user → ==*Create* an Output Data object== (use case result), and
        - ==*Pass* it to the Presenter==
            - By calling a method in Output Boundary interface (implemented by Presenter)
4. **Presenter**
    - Responsibility:
        - ==Take the Output Data==, and
        - ==Turn it into raw strings and numbers== to be displayed
        - ==Update **View Model**== with this information
            - Need a mechanism for the View to know that the View Model has changed
                - “Observer” or “Listener”
            - View is waiting for changes to View Model
5. **View**, again
    - Responsibility:
        - When View Model is updated, View is alerted through a callback
        - → View ==displays the new information to user==, possibly activating another View

> [!note] Interfaces (Input Boundary, Output Boundary, Data Access Interface) help us plug everything together → Makes this engine testable

# Terminology

- **Entity**
    - Basic bit of data that we are storing in our program
        - e.g., user with a username and password
    - Often called a *model* of the real world
- **Factory**
    - An object that knows how to instantiate a class or a collection of interrelated classes
- **Use case**
    - Something a user wants to do with the program
- **Interactor**
    - Object that responds to a user interaction
    - Usually part of a use case (implements the input boundary)
- **Input boundary**
    - Public interface for calling the use case
- **Output boundary**
    - Public interface that Interactor will call when the use case is complete
- **Data access object (DAO)**
    - Involves persistence (a file or database)
    - Often called a *Gateway* or a *Repository*
    - Reads data and creates Entities
- **Controller**
    - Object that the UI asks to run a use case
- **Presenter**
    - Object that tells the UI what to do when a use case finishes
- **Model**
    - Model of a concept from the problem domain
    - A collection of data representing a concept from the problem domain
    - *Entities* are often called *models*

# Clean Architecture Follows SOLID

> [!check]+ Single Responsibility Principle
>
> - Each role ==divides up the responsibilities== for each class
> - Only Gateway classes are responsible to other hardware or software that is outside of your program
> - Only Presenters are responsible to whomever decides which information is displayed to the user
> - Each entity is responsible for storing info about one building block of the program
>     - e.g., User class in a program that allows Users to login and save their session

> [!check]+ Open/Closed Principle
>
> - CA ==separates layers of code== based on how close or far they are from the details
>     - e.g., What does the use see, which hardware/OS is running the program, where we store persistent data, etc.
>     - → Easy to reuse the backend in an app for a different system (web app, Android, etc.)
>     - → Easy to reuse entities across different apps that follow the same Enterprise Business Rules
>     - → Possible to add new Use Cases without changing much of the original code

> [!check]+ Liskov Substitution Principle
>
> - Various interfaces introduced should help us write code consistent with this principle, but
>     - Still need to ensure our code consistently uses interfaces and that they are designed and documented properly

> [!check]+ Interface Segregation Principle
>
> - Each ==interface has a specific role== and is ==associated with one Use Case==
> - → No unnecessary methods are implemented

> [!check]+ Dependency Inversion Principle
>
> - In CA, this is closely related to the **Dependency Rule**
> - Principle is applied to remove dependencies on the low-level details of the program

# User Login Example

- Recall specification for a user login system we talked about earlier
- Had started to develop a design for it in terms of use cases
    - See [partial implementation](https://github.com/paulgries/UserLoginCleanArchitecture)

## Use Case APIs

- A class for each use case
- `UserRegisterInteractor`
    - <u>Public API:</u> A method called `create`
        - Perhaps with username and password as parameters, or maybe wrapped up in a data class
    - Needs to tell UI to prepare a success view or a failure view
    - Needs to know how to make a new user
    - Needs to ask the persistence layer whether a username exists
    - Needs to tell the persistence layer to save a new user

## Register New User Use Case Sketch

```java
class UserFactory {
    public User create(String name, String password) { ... }
}

class UserRegisterInteractor {
    private UserFactory userFactory;  // A class to create User objects
    private UserPresenter userPresenter;  // Controls the UI
    private UserRegisterGateway userRegisterGateway;  // Persisting data

    public UserRegisterResultModel create(String name, String password) {
        // On success, this method returns an object with the
        // username and creation time, but not the password
    }
}
```

- Interactor *interacts* with various pieces of our program
    - Class itself doesn’t do that much
- Every task that it needs to do gets ==delegated to something else==
