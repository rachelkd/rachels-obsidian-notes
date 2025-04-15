---
dg-publish: true
tags: [cs, java, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 7
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "13"
Chapter:
Slides/Notes:
  - "[[13-TestingInCA.pdf]]"
Date: 2024-10-15
Date created: Sun., Oct. 20, 2024, 11:28:19 pm
Date modified: Tue., Dec. 10, 2024, 7:15:30 pm
---

> [!question]- Questions to be answered this week
>
> - How can we test a program which adheres to Clean Architecture?
> - What is mocking?
> - What are packages in Java?
> - How can we effectively use packaging in our Java projects?

# Beyond Simple Unit Testing

- More pieces in CA structure → More complicated to test everything
- e.g., How can we test a `UseCaseInteractor` since it depends on implementations of various interfaces to function correctly?

# Three Types of Tests

1. **Unit test**
    - See [[Unit Testing]]
2. **Integration test**
3. **End-to-end test**

> [!def]+ Unit Test
>
> - Tests the smallest unit of cohesive code (often a method)
>     - e.g., Can create a test class called `EntityTests` of unit tests for all methods in class `Entity`

> [!def]+ Integration Test
>
> - Tests how two components interact with each other
>     - e.g., Two classes

> [!def]+ End-to-end Test
>
> - Tests an entire path through the code from input to output
>     - e.g., Start and end with the View
> - [c] Fragile
>     - Depends on details (e.g., literally look at UI implementation and test formatting)
>     - If anything changes in program, test is probably going to break
>     - → Do we change the test, or did we introduce a bug?

# How Can We Test This?

![](https://i.imgur.com/0wKb3xC.png)

## Entities and Interactors: Unit Testing

- [*] Entities:
    - Unit tests for each entity
    - Unit tests help show that program data is ==being represented properly==
    - Entities do not depend on the rest of program → Naturally isolated
- [*] Use Case Interactor:
    - Given some Input Data, does the Interactor produce the correct Output Data?
    - Higher-level unit than for the Entities, but still a unit
    - [?] DAOs are slow: How do we isolate Interactor from DAOs?
    - [n] Interactor depends on Interfaces (Output Boundary, Data Access) → Need to provide implementation of these interfaces
        - Can implement however we want (since interface)
        - Do not actually have to plug in real database; can plug in dummy data specific to case we are testing; do not have to worry about communicating with real database

## Interactors and DAOs: Integration Testing

- It’s nice that we can plug in a fake data access object and mock its behaviour to execute our use case in testing
- At the end of the day, we are going to plug in our real database and want that to work

![|center|400](https://i.imgur.com/bltXNNs.jpeg)

- Connecting Interactor with Data Access and Database
    - Can also separately unit test the Database of code
- Tend to run these tests less frequently; more *expensive*
- Typically, if you unit tested both pieces → Confident that they should work together

## Isolating Each Layer: End-to-end Testing

- Starting from the view → Do some interaction with program → Whole thing executes → Look at final result
- When we test our entire use case, starting at the View, it is an **end-to-end test**
- One implementation of this:
    - Literally interact with the view
    - A lot of UI frameworks provide functionality to create fake button events/clicks
    - Write a program that programmatically interacts with UI, hits a button, program runs, look at View after and inspect UI elements to make sure it is what you expect it to be
- [c] Fragile:
    - If anything changes in UI, the test is going to break
- [n] Alternative:
    - View Model has the same info as View, but a data structure with fields in it
    - Can think of it like a list of Strings, for simplicity
    - → More stable, easier to reason about vs. details of UI
- [*] Don’t have to go all the way to the end, but should get as close to it as possible
    - Not quite end-to-end, but it gets you close

# Design for Testability

- CA designed to encourage testing
- Ability to *mock* each layer is powerful
- Refactor code if it is hard to test

# Use Case Interactor Testing

- Interactors ==receive Input Data== from a Controller, and
- ==Produce Output Data== for the Presenter

> [!note]+ Test class serves as the ==Controller==
>
> - Write test code for *setup*

> [!note]+ Test class needs to ==create a Presenter== that ==checks the Output Data and any Entity and persistence mutation==
>
> - Write test code that *asserts*

- [?] In which layer should your test class go?
    - Should be where it is interacting with things
    - If we are writing test class for use case → End up in use case layer

Example Tests from [Homework 5 starter code](https://github.com/CSC207-2024F-UofT/CAUserLogin/tree/main/src/test/java/use_case)

## Use Case Interactor: the Success Test

```java file:SignupInteractorTest.java
void successTest() {
    SignupUserDataAccessInterface userRepository = new InMemoryUserDataAccessObject();

    SignupOutputBoundary successPresenter = // Make a presenter here that asserts things

    SignupInputData inputData = new SignupInputData("Paul", "password", "password");

    SignupInputBoundary interactor = new SignupInteractor(
        userRepository, successPresenter, new CommonUserFactory()
    );
    interactor.execute(inputData);
    // This will eventually send Output Data to the successPresenter
}
```

- Setup: Create Input Data and Use Case to then execute
- In order to create our Use Case, we ==need all the things that it depends on==
    - `userRepository`; what our DAO is going to be
        - For testing purposes, we are not going to connect to a real database → Connect to `InMemoryUserDataAccessObject` (stores users in a `Map` in memory)
    - `successPresenter`; our Presenter
        - Contains our asserts
        - Looks at our Output Data to make sure it is correct
    - `inputData`; the Input Data we are testing

> [!obs]+ `execute` Interactor method returns nothing (fundamental to CA)
>
> - ==Does not mean that it has no output!==
> - Output happens through `successPresenter` where we have our asserts

## Use Case Interactor: the Presenter

```java file:SignupInteractorTest.java
// This instantiates an anonymous SignupOutputBoundary implementing class
SignupOutputBoundary successPresenter = new SignupOutputBoundary() {
    @Override
    public void prepareSuccessView(SignupOutputData user) {
        // 2 things to check: the output data is correct, and the user has been created in the DAO.
        assertEquals("Paul", user.getUsername());
        assertTrue(userRepository.existsByName("Paul"));
    }

    @Override
    public void prepareFailView(String error) {
        fail("Use case failure is unexpected.");
    }
};
```

*This goes in the `successTest` method.*

- Assert that username is correct
    - Tests if our output is correct
- Assert that we store user in repository
    - Is the state of our DAO what we expect it to be?
- [*] Both testing output and *state* of the program

<!-- break -->
- When we execute our Interactor → Calls `successPresenter` to prepare success view → Calls two assert statements to check output and state correctness

# Extra Advice

- If you want to test the same use case with different inputs…
    - Construct necessary parts of the CA engine once in a `@BeforeAll` method or make use of `@BeforeEach`
- Test for both success and failures
- Taking the time to write these tests for each use case as you develop the use case will help ensure the quality and correctness of your final code ([[Unit Testing#Test-Driven Development|test-driven development]])
- In a team environment, expect your team members to write test for any code they are submitting in a pull request for *before* you approve their request
