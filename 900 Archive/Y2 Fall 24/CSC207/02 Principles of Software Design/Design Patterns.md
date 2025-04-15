---
dg-publish: true
tags: [cs, java, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 8
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "15"
  - "18"
Chapter:
Slides/Notes:
  - "[[15-DesignPatterns.pdf]]"
Date: 2024-10-22
Date created: Thu., Oct. 24, 2024, 7:28:27 pm
Date modified: Tue., Dec. 10, 2024, 10:30:01 pm
---

> [!question]+ Questions to be answered this week
>
> 1. What is a design pattern?
> 2. What are the three categories of design patterns we will cover?
> 3. For each of the following design patterns: Dependency Injection, Builder, Strategy, Observer, Adapter, Façade
>    1. Name the pattern
>    2. Describe the problem that it fixes
>    3. How the solution (the pattern) works including the roles of each class that is involved
> 4. In the ethics portion of this weeks lectures, we discussed why designing for the majority of users can cause “tangible harms” and “harm to relational equality”. Give an example of each.

# Design Patterns

> [!def]+ Design Pattern
>
> - A general description of the solution to a well-established problem
> - Every design pattern is aimed at solving a design problem
>     - e.g., Maybe code is too dependent; how do you break this?

- Patterns describe the shape of the code rather than the details
- Means of communicating design ideas
- Not specific to any single programming language

More about patterns in CSC301 (Intro to Software Engineering) and CSC302 (Engineering Large Software Systems)

> [!important] [Design Pattern Examples](https://github.com/CSC207-UofT/design-pattern-samples/tree/main/src)

## Loose Coupling, High Cohesion

> [!goal]+ Two goals of object-oriented design
>
> > [!def]- Coupling
> >
> > - The interdependencies between objects
> > - Fewer couplings → Better $\implies$ Can test and modify each piece independently
> > - If you have a complex object graph where there are many dependencies → *Highly coupled* → Changing code in one part causes ripple effect across rest of program
> > - ! Want to avoid this
>
> > [!def]- Cohesion
> >
> > - How strongly related the parts are inside a class
> > - About a class
> >     - A class is *cohesive* if the data and behaviour of these objects makes sense
> > - **High cohesion**:
> >     - A class does one job, and does it well
> > - **Low cohesion**:
> >     - Class has parts that do not relate to each other
> > - e.g., `Customer` class might have `getName`, `getAddress`, but also `sendEmail` that sends an email to the customer → Weird; not a major responsibility of the customer → ==not cohesive==

> [!important] Design patterns are often applied to <mark style="background: #D2B3FFA6;">decrease coupling</mark> and <mark style="background: #D2B3FFA6;">increase cohesion</mark>

- Notice that you can increase cohesion by removing an irrelevant method
    - Not always about *adding*!
- & Keep [[Solid Design Principles|SOLID]] principles in mind when discussing each design pattern
    - SRP, OCP, LSP, ISP, DIP

# Gang of Four

- First codified by the Gang of Four in 1995
- Original book described 23 patterns
    - More have been added

> [!summary]+ Book provides an overview of
>
> - **Design Pattern Name**
> - **Problem**
>     - When to use the pattern
>     - Motivation:
>         - Sample application scenario
>     - Applicability:
>         - Guidelines for when your code needs this pattern
> - **Solution**
>     - Structure:
>         - **UML Class Diagram of generic solution**
>     - Participants:
>         - Description of the basic classes involved in the generic solution
>     - Collaborations:
>         - Describes relationships and collaborations among the generic solution participants
>         - e.g., Does your Observer have an instance variable that is your Observable, does the Observable have a list of Observers that are observing it? Or does an object manage all of that communication
>     - Sample code
> - **Consequences, Known Users, Related Patterns, Anti-patterns**

# Design Pattern Categories

> [!def]+ Creational
>
> - Patterns related to how we *create* instances of our classes

> [!def]+ Behavioural
>
> - Patterns related to how instances of our classes *communicate*

> [!def]+ Structural
>
> - Patterns related to how classes can naturally *fit* together
>     - While obeying SOLID principles

- Design patterns covered in [[CSC207]]
    - **Creational**:
        - Dependency Injection, Simple Factory, Builder
    - **Behavioural**:
        - Strategy, Observer
    - **Structural**:
        - Adapter, Façade

# Creational Patterns

## Dependency Injection

> [!def]+ ==Dependency== relationship between two classes
>
> - i.e., “using” relationship
> - ==One class has an instance variable or uses a parameter of another class==
>     - One class depends on another
> - If the second class changes, the thing that depends on it is likely to change as well
>     - e.g., A class `Course` may depend on a class `Student` because a `Course` contains instances of class `Student`

> [!def]+ Dependency Injection Design Pattern
>
> - A design pattern where dependencies are provided to a class from the outside rather than created inside the class
> - Reduces coupling by removing hard dependencies
> - Allows for greater flexibility and easier testing by enabling different implementations to be “injected”
>
> > [!warning]+ Problem
> >
> > - We are writing a class, and we need to assign values to the instance variables, but we do not want to introduce a **hard dependency**

### Dependency Injection Example

> [!def]+ Hard Dependency
>
> - Using operator `new` inside first class can create an instance of a second class that cannot be used nor tested independently

#### Before

```java title:"Example. Hard Dependency"
public class Course {
    private List<Student> students = new ArrayList<>();

    public Course(List<String> studentsNames) {
        for (String name: studentNames) {
            Student student = new Student(name);
            students.add(student)
        }
    }
}
```

- If you change the `Student` constructor, you would have to change how `Course` creates new `Student`s
- Violates SRP: `Course` is also responsible for *creating* `Student`s
- If you want multiple types of `Student`s (i.e., subclasses), you can’t
- ! Hard dependency prevents you from writing subclasses of `Student` and injecting it

> [!warning]+ Problem
>
> - We are writing a class, and we need to assign values to the instance variables, but we do not want to introduce a **hard dependency**

#### After

> [!success]+ Solution
>
> - Create the `Student` objects *outside* and *inject* them into `Course`
> - $\implies$ Allows subclasses of `Student` to be injected into `Course`

> [!def]+ Inject
>
> - Pass in as a parameter to a constructor or a setter or adder

```java title:"Example. Dependency Injection"
public class Course {
    private List<Student> students = new ArrayList<>();

    // Student objects are created outside the Course class and injected here.
    public add(Student s) {
        this.students.add(s);
    }

    // We might also inject all of them at once.
    public addAll(List<Student> studentsToAdd) {
        this.students.addAll(studentsToAdd);
    }
}
```

- Purpose of the `Course` is to *organize* students, not create them
    - Up to the Main program to decide what kind of Student object to instantiate
    - No longer responsibility of `Course`
- ? Is there another hard dependency in the code?
    - `ArrayList`

> [!important] Hard dependencies are not necessarily bad, except in the context of data that needs to be *flexible*

### Dependency Injection: In Practice

- Where have we seen **Dependency Injection** before in [[Clean Architecture|CA]]?
    - Anytime you use a reference type as a parameter
    - Anytime you have a method that takes any class type
    - Not injecting if it is primitive
- How does the [[Solid Design Principles|Dependency Inversion Principle]] relate to the **Dependency Injection Design Pattern**?
    - Kind of all of them except for perhaps ISP
    - SRP: Should `Course` create Students? Probably not.
        - Another example of why old way was bad: You can’t share Students in two different courses
    - LSP: Can pass in subclasses of Students and use them
    - DIP: Dependency injection allows you to invert a dependency → Directly related

## Simple Factory

> [!def]+ ==Simple Factory== Design Pattern
>
> > [!warning]+ Problem
> >
> > - One class wants to interact with many possible related objects
>
> > [!check]+ Solution
> >
> > - We want to obscure the creation process for these related objects
> > - Later, we might want to change the types of the objects we are creating → Avoid hard dependencies

> [!summary]+ Paul’s Analogy
>
> - Let’s say you are building a car in a factory. You also want to build the car manual.
> - Volkswagen does this all in one piece of software; Don’t want to rewrite software every time they make a car
> - Want to repurpose the Factory:
>     - Factory needs to be able to build only some of the things
>     - e.g., No heated seats in this model
>     - Also need to build the car manual that has exactly the same features as the car you bought
> - May want a Factory for building car and another Factory for building the manual
>     - Make sure they take the same steps
>     - Define those steps in the abstract Factory that your two subclasses inherit from
>     - One’s building a car, one’s building a book that can follow the same abstract Factory steps
> - ==Want to obscure the creation process for these related objects==
>     - Might want to change the types of objects to avoid hard dependencies

### Example. Shape Factory

![](https://i.imgur.com/Px1luCf.png)

- & Whole point is to *decouple* the Shape factory and allow it to create all of these different kinds of things
    - Returning an interface type
    - “Could you build me a Square/Rectangle/Circle?”
    - Method that you called is going to be creating all of these shapes
    - Pass in a String e.g., `"circle"` in `getShape` if you want a circle

### Factory: In Practice

> [!question]+ What do we gain by having the factory be responsible for creating instances of objects for us?
>
> - Factory’s sole responsibility is to build these objects
> - Taking away responsibility of construction from these objects themselves

> [!question]+ Is it still a “Factory” if the method only returns instances of one class (e.g., `Rectangle`) and not instances of a subclass?
>
> - i.e., Is it still a Factory if we replace a `ShapeFactory` with a `RectangleFactory`, where the `Rectangle` class has no subclasses?
> - Is it still useful to replace `ShapeFactory` with `RectangleFactory` if all we need are `Rectangle` objects

- In our CA example, we have a `CommonUserFactory` that always builds a `CommonUser`
    - ? Is that bad?
    - Allows you to give anyone who wants it a factory for building these things
        - e.g., Things on the use case might need a factory for making users, Data Access Layer might need a factory for making users
- & By moving the responsibility of creation from the object in some class itself to a factory, you can pass this factory object all around
    - Might help us in the way we think about designing the rest of the program
- Helps with OCP:
    - Allows you to create other kinds of shapes if you decide to in the future

## Builder

> [!def]+ ==Builder== Design Pattern
>
> > [!warning]+ Problem
> >
> > - Need to create a complex structure of objects in a step-by-step fashion
>
> > [!check]+ Solution
> >
> > - Create a `Builder` object that creates the complex structure

![](https://i.imgur.com/8iFvy4H.png)

*`Builder` is an interface or abstract class (italicized).*

- Which of these boxes is `AppBuilder` in Homework 5?
    - `ConcreteBuilder`
- `Director`?
    - `Main` program
    - Does not always exist

<!-- break -->
- Back to Paul’s Analogy of a Car Factory:
    - If you define an abstract builder class, you can use that abstract builder and have a concrete car builder and a concrete car manual builder
    - Director will be handed — one at a time — the car builder (will go build the car) and car manual builder (will use the exact same sequence of method calls, but build the manual instead)

### Without App Builder Version

Note: Only Signup Shown.

```java title:Director
final JFrame application = new JFrame("Login Example");
final CardLayout cardLayout = new CardLayout();
final JPanel views = new JPanel(cardLayout);
application.add(views);

final ViewManagerModel viewManagerModel = new ViewManagerModel();
final ViewManager = new ViewManager(views, cardLayout, viewManagerModel);

final SignupViewModel signupViewModel = new SignupViewModel();
final DBUserDataAccessObject userDataAccessObject = new DBUserDataAccessObject(
    new CommonUserFactory()
);

final SignupView signupView = SignupUseCaseFactory.create(
    viewManagerModel,
    loginViewModel,
    signupViewModel,
    userDataAccessObject
);

views.add(signupView, signupView.getViewName());
```

### App Builder Version

```java title:Main.java
final AppBuilder appBuilder = new AppBuilder();
final JFrame application = appBuilder
                .addLoginView()
                .addSignupView()
                .addLoggedInView()
                .addSignupUseCase()
                .addLoginUseCase()
                .addChangePasswordUseCase()
                .build();
```

### Sequence Diagram: Before

![](https://i.imgur.com/hp61Phy.png)

- Sequence for `Main.main` from Homework 5, Phase 1
- A ==subset of the diagram== for setting up the Signup part of the CA engine
- Note: Factory helps hide some of the details for us

### Sequence Diagram: After

![](https://i.imgur.com/iRJ3zTJ.png)

- Sequence diagram for when we create the lab-5 application in `Main.main`
    - Only shows part related to the Signup Use Case
- A ==subset of the digram== for setting up the Signup part of the CA engine
- Note: The final `JFrame` is created by the `Builder` for us when we build the app

![|center|400](https://i.imgur.com/XxrI8Jc.png)

- Sequence diagram for when we create the lab-5 application in `Main.main` like before
- Hides calls to constructors
- Details are hidden in `AppBuilder`!

### More Builder Examples

- [Repo](https://github.com/spotify-web-api-java/spotify-web-api-java) that extensively uses builder (See [SpotifyApi.java](https://github.com/spotify-web-api-java/spotify-web-api-java/blob/master/src/main/java/se/michaelthelin/spotify/SpotifyApi.java))
- [IntelliJ refactoring to replace a constructor with a builder](https://www.jetbrains.com/help/idea/replace-constructor-with-builder.html)
- Comparison of Factory and Builder [article](https://medium.com/javarevisited/design-patterns-101-factory-vs-builder-vs-fluent-builder-da2babf42113)

### Builder Design Pattern: In Practice

> [!question]+ Where have we seen builders before?
>
> - StringBuilders

> [!question]+ How complicated does an object have to be to require a builder?
>
> - If all you have is the `build` method where you only call the constructor and return that thing, then you probably don’t need a builder
> - Hard to distinguish from a Factory at that point, anyway
> - ==If there’s a sequence of steps, and you’re allowed to choose which steps you want to include → Use `Builder`==

> [!question]+ Which SOLID principles does the Builder design pattern follow?
>
> - SRP:
>     - ==Responsibility of the Director to choose which features to build==
>     - ==Responsibility of the Builder to maintain the currently built application and return it at the end==
> - OCP:
>     - Can add new steps pretty easily

# Behavioural Patterns

## Strategy Design Pattern

> [!def]+ Strategy Design Pattern
>
> > [!warning]+ Problem
> >
> > - Multiple classes ==differ only in how they are implemented==
> > - i.e., Multiple implementations of the ==same idea==; ==solve the same problem==, but how they do it is different
> > - High-level logic is same except for which algorithm is being used to solve part of the task
> > - Other classes may also benefit from code implementing the algorithms, but code is currently *coupled* to the class using a specific algorithm
>
> > [!goal]+
> >
> > - Want to **decouple** (separate) the implementation of a class from the implementation of the algorithms which it may use
> > - Want to make more generic so that it can be useful elsewhere as well

### Without the Strategy Pattern

![](https://i.imgur.com/oRVjUpP.png)

- An Author has a name and list of Books, and there’s a way to sort books
    - Book implements Comparable
    - Sort does so by Comparable `compareTo`
- Two subclasses: `AuthorInsertion` and `AuthorSelection`

> [!warning]+ Problem: Cannot swap sort class part-way through
>
> - Make decision when you build the `Author` class → Constraining and limiting

### With the Strategy Pattern

![](https://i.imgur.com/QPtBwnX.png)

- `Author` has a `sorter` instance variable
    - Moved algorithm out into separate class
    - When you instantiate an `Author`, you can give it a `Sorter`
    - Able to add a `setSorter` method that swaps the algorithm that the `Author` was using
- Replacing inheritance as composition
    - Generally, more flexible to use composition as inheritance
    - Do this through **[[#Dependency Injection|dependency injection]]**

### Strategy: Standard Solution

![](https://i.imgur.com/m6ISyns.png)

> [!question]- What was our *context* in the `Author` example?
>
> - `Author`
> - Our strategy was `sort`

> [!def]+ Context
>
> - The object that knows which strategy is being used

> [!question]+ What counts as a strategy? Does it have to be an algorithm?
>
> - Everything is an algorithm, so yes

> [!question]+ Which of the SOLID principles are followed by this pattern?
>
> - SRP
>     - What we do with the Author is no longer responsible for implementing its own sorting algorithm
>     - Pushing that work to another object, `Sorter`
> - OCP
>     - Can make new sort strategies without changing Author code at all
>     - Just inject into sorting algorithm
> - DIP
>     - Author does not know which sorting strategy it is using
>     - Thinks its *generic*; told which one it is using

## Observer Design Pattern

> [!def]+ Observer
>
> > [!warning]+ Problem
> >
> > - Need to maintain consistency between ==related== objects
> > - Whenever this object changes, it needs to go through and tell all the things that depend on it
> > - Two aspects: ==one dependent on the other== (**cause and effect**)
> > - An object should be able to ==*notify* other objects about changes== to itself without making assumptions about who these objects are
>
> > [!goal]+
> >
> > - You want one object to *listen* for changes in another

### Old Java Implementation

![](https://i.imgur.com/6LCKHep.png)
*UML Class Diagram for `Observable`*

- `Observable` abstract class
    - Maintains a list of **observers**
    - Can add/remove observers
- Typically, the Builder will inject all the things its observing (Observables) into the Observer
- A way to tell if there has been any changes to the state of the Observable
- A way to *notify* all the Observers
- YourObservableClass overrides the Observable abstract class
    - Sole responsibility: Maintain any state it needs
    - Does not need to think about the list of observers → Responsibility of abstract Observable class

> [!question]+ When you set state, what needs to happen?
>
> - Let everybody know
> - In `setState` method, there is going to be a ==call to (inherited) `notifyObservers`==
> - Will loop through observers and make observers `update` based on new state

### Example. Parcel

![](https://i.imgur.com/lwYqHxW.png)

- Parcel has a location → Changes → Update location (`updateLocation`) → Trigger call on `notifyObservers`

> [!important] `Observable` is an **abstract** class; `Observer` is an **interface**

- `Parcel` *extends* `Observable`
- `Company` and `Customer` ==must have an `update` method==
    - In that code, do any updates you need to do for a Company or Customer
    - e.g., Talk to a Presenter; update stuff to the view
- The `Observable` has 0+ observers

### Why Did Java Deprecate Observable?

- `YourObservableClass` can ==only extend== `Observable`
    - Cannot have a class in an inheritance hierarchy also *extend* `Observable`
- Class was deprecated due to several limitations:
    - Event model is quite limited
    - Order of notifications is unspecified
    - State changes are not in one-to-one correspondence with notifications
- Recommended alternatives:
    - `java.beans` package for a richer event model
    - `java.util.concurrent` for reliable and ordered messaging between threads
    - `Flow` API for reactive streams style programming

> [!tip] Instead of using inheritance, we are going to use **composition**

- Why should `YourObservableClass` care how the list of observers is maintained?
    - If you inherit from Observable, then that is what you get
    - But instead, we ==use an object as an instance variable== → *Delegate* requests to that object
        - Similar to [[#Strategy Design Pattern]]

### Implementation Using Delegation

![](https://i.imgur.com/tFDh4ET.png)

> [!info]+ Arrow Types in UML Diagram
>
> 1. **Open triangle with dashed line**
>     - Represents *interface implementation*
>     - `View` implements the `PropertyChangeListener` interface
>
> 2. **Diamond with solid line**
>     - Represents *composition*
>     - `ViewModel` has a `PropertyChangeSupport` object as an instance variable
>     - The filled diamond indicates the lifecycle of `PropertyChangeSupport` is controlled by `ViewModel`
>     - `ViewModel`contains the `State` object → State’s lifecycle is managed by the `ViewModel`
> 3. **Dashed arrow**
>     - Represents a *dependency* relationship
>         - `View` depends on `State` but doesn’t “own” it
>         - Typically means `View` uses `State` in some way (perhaps reading from it) but isn’t responsible for its lifecycle
>
> The overall structure shows:
>
> - `ViewModel` contains a `PropertyChangeSupport` object
> - `View` implements `PropertyChangeListener` interface
> - When state changes, `PropertyChangeSupport` notifies all listeners through their `propertyChange` method

- `PropertyChangeSupport` object has `firePropertyChange` method
    - Similar to `notifyObservers`
    - `PropertyChangeSupport` is an object created specifically to ==maintain a list of observers==
        - Also has an `addListener` method not in class diagram
- & `firePropertyChange` calls `propertyChange` on each listener
    - Similar to the `update` method from deprecated Java implementation

> [!info]+ `PropertyChangeSupport`’s Single Responsibility:
>
> - Maintain list of listeners and call their `propertyChange` method

- Rather than extending it like in [[#Old Java Implementation]], your observable class has an **instance variable** pointing to the code that is *managing* its listeners (`PropertyChangeSupport`)
    - Taking out your observable code into its own class → Injecting it your observable class → ViewModel has an instance of `PropertyChangeSupport`
        - Pass on call to `addPropertyChangeListener`/`firePropertyChange` to support instance variable
            - One line method implementation:
                - `support.addPropertyChangeListener(PropertyChangeListener obj)`, or
                - `support.firePropertyChanged()`
    - In Java 8, you would have had `ViewModel` extend Observable

### In Practice

> [!question]+ Where have we already seen observers?
>
> - In our Clean Architecture implementation:
>     - `View` observes `ViewModel`
>     - `ViewModel` notifies `View` of state changes
> - GUI frameworks often use observer pattern:
>     - Button click listeners
>     - Event handlers

> [!question]+ Which part(s) of clean architecture can benefit from the observer pattern?
>
> - Interface Adapters layer:
>     - View-ViewModel relationship
>     - Presenter updating View
> - Can help maintain separation of concerns while allowing necessary communication
> - Particularly useful for updating UI based on data changes

> [!question]+ Why is this a good pattern to implement across a boundary?
>
> - Maintains loose coupling between layers
> - Observable doesn’t need to know details about its observers
> - Allows for communication across architectural boundaries without violating dependency rules
> - Can notify multiple observers without tight coupling to any of them

# Structural Patterns

## Adapter

> [!def]+ Adapter Design Pattern
>
> > [!warning]+ Problem
> >
> > - Want to reuse a class that already exists, but it does not have the methods (public interface) required by the rest of the program
>
> > [!success]+ Solution 1: Use [[Inheritance]]
> >
> > - Create a subclass that extends the old class and include the missing methods
>
> > [!success]+ Solution 2: Use a **wrapper** and **delegation**
> >
> > - Create a container class that has an instance of the old class as a variable
> > - Rest of the program can call the container’s methods, which then call the old class’ methods

### Analogy: Square Pegs in a Round Hole

- Imagine you have a round hole
- You want to be able to put square pegs in this round hole
- What kind of square pegs can fit into this round hole?
    - Radius has to be the diagonal of the square from the center
- The methods are incompatible, but maybe you can write an **adapter** to make it fit

![](https://i.imgur.com/1T9Pvhl.png)

### In Practice

> [!question]+ Which SOLID principles are followed by this pattern?
>
> - OCP:
>     - Can add new code that isn’t compatible by writing one class to do the adapter part
> - SRP:
>     - Single responsibility is to adapt
> - Liskov:
>     - SquarePegAdapter is a subclass of RoundPeg
>     - Therefore behaves like a RoundPegs

> [!question]+ Where have we seen adapters before?
>
> - Real life
>     - Adapt this USB style into another USB style
> - Same thing in code

> [!question]+ When might you *not* want to use this pattern?

## Façade

> [!def]+ Façade Design Pattern
>
> > [!warning]+ Problem
> >
> > - A single class is responsible to multiple *actors*
> > - We want to encapsulate the code that interacts with individual actors
> > - We want a simplified interface to a more complex subsystem
>
> > [!success]+ Solution
> >
> > - Create individual classes that each interact with only one actor
> > - Create a Façade class that has (roughly) the same responsibilities as the original class
> > - *Delegate* each responsibility to the individual classes
> >     - $\implies$ Façade object contains references to each individual class

### Before

- In some restaurant software, we have a class called `Bill`
- Responsible for:
    - Calculating total based on a frequently-changing set of discount rates
        - “10% off before 11 am”
        - Interacts with a discount system that contains a list of rates
    - Logging the amount paid and updating the accounting subsystem
        - Interacts with the accounting system
    - Printing a nicely-formatted bill to give to the customer
        - Interacts with the print device

### After

- Factor our an `Order` object that contains the menu items that were ordered
- Create classes called `BillCalculator`, `BillLogger`, `BillPrinter` that all use `Order`
- Create `BillFacade`, which
    - **delegates** the operations to `BillCalculator`, `BillLogger`, `BillPrinter`

e.g., `BillFacade` might contain this instance variable and method:

```java title:"BillFacade.java"
BillCalculator calculator = new BillCalculator(order);

public calculateTotal() {
    calculator.calculateTotal();
}
```

### In Practice

> [!question]+ When did we see an example of a Façade? Which SOLID principle was it demonstrating?
>
> - Employee Facade in [[Solid Design Principles#A Story of Three Actors|Single Responsibility Principle]]

> [!question]+ How do Facade classes create a boundary within your program?
> Facade classes create several types of boundaries:
>
> 1. **Abstraction Boundary**
>     - Creates simplified interface hiding complex subsystem interactions
>     - Single entry point to multiple subsystems
>     - External code only needs to know Facade interface
>
> 2. **Encapsulation Boundary**
>     - Hides subsystem implementation details
>     - Allows subsystem modifications without affecting client code
>     - Enforces separation of concerns
>
> 3. **Architectural Boundary**
>     - Functions as layer boundary in layered architecture
>     - Separates high-level components from implementation details
>     - Enforces dependency rules by providing stable API
>
> 4. **Integration Boundary**
>     - Facilitates external system integration
>     - Adapts incompatible interfaces
>     - Controls interaction between system components
>
> 5. **Testing Boundary**
>     - Provides clear seam for mocking/stubbing
>     - Enables isolated subsystem testing
>     - Simplifies integration testing at Facade level
>
> These boundaries promote loose coupling, increase cohesion, and help manage system complexity through abstraction.

- [Wikipedia Facade pattern](https://en.wikipedia.org/wiki/Facade_pattern) has a nice discussion of Adapter, Façade, and Decorator
    - Decorator was not covered in [[CSC207]]
