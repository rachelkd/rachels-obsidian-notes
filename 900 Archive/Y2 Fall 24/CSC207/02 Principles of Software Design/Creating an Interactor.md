---
dg-publish: true
tags: [lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 10
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "19"
Chapter: 
Slides/Notes:
  - "[[19-CatchupClass.pdf]]"
Date: 2024-11-12
Date created: Fri., Nov. 15, 2024, 5:40:20 pm
Date modified: Wed., Nov. 27, 2024, 10:40:27 pm
---

# From User Story to Code

- Prerequisite:
    - A fully planned user story

- Steps:
1. Figure out the data involved in the user story
    - Identify and create necessary Entities
    - Map out data relationships
    - Define data attributes

2. Plan sequence of actions
    - Break down user story into specific, atomic steps
    - Keep stories small and focused
    - Document dependencies between actions

3. For each user interaction:
    - Create required components:
        - Input Data structure
        - Output Data structure
        - Data Access Interface
        - Input Boundary interface
        - Interactor class
        - Output Boundary interface
    - Implement basic DAO
        - Start with in-memory implementation
        - No file I/O initially
        - Can use real API if available
    - Development process:
        1. Write unit tests for Input Boundary
        2. Implement Interactor code to pass tests
        3. Connect UI to Interactor
        4. Develop production-ready DAO

# Figuring Out Entities

- User Story:
    - Transfer money from one bank account to another
    - User needs to choose two bank account numbers and the amount to transfer
        - Will worry about the UI in a bit, but
            - Maybe use drop-down menus to choose the accounts and a “dollars” text field and a “cents” text field, or
            - Maybe type the bank account numbers
- Entities involved might be:
    - `BankAccount`
    - `Money`
    - Write those

# Plan a Sequence of Actions for Transferring

- First action:
    - When the user opens the app
    - If app displays any persistent information, this must trigger a use case interaction
        - Will deal with that as a separate user story and assume it as a prerequisite for this “transfer” user story
- Relevant actions:
    - Choose the first bank account
    - Choose the second bank account
    - Enter the amount of money
    - Click a button
        - or type “return”/“enter”?

# For Each User Action

## Input and Output Data

- In “transfer” user story, there is only one user action that involves persistent data
    - When user clicks the button (or types “return”/“enter”?)
- Input Data:
    - Two bank account numbers
    - Amount of money to transfer
    - & Create Input Data class
- Output Data:
    - Include new balances of bank accounts
        - Might use Map where keys are bank account numbers (as Strings), and values are the balances
    - Need to report on the amount of money transferred
    - Failing case:
        - Output Data will need a message describing the problem
        - e.g., “Insufficient funds”
    - & Create Output Data class

## Input and Output Boundary

- Boundaries are quite simple (and usually look the same)
    - `public void execute(InputData)`
    - `public void prepareSuccessView(OutputData)`
    - `public void prepareFailView(OutputData)`

## Data Access

- Transfer interactor will need `BankAccount` and `Money` entities
- Amount of money is in the Input Data, so Interactor might use a `MoneyFactory`
- All the bank account information is in the persistence layer
    - Might make a `getBankAccount(accountNumber)` that goes in the DAI
- Write a basic DAO that implements the DAI
    - Simple implementation might just use a Map of a bank account number to BankAccount object
    - Write real DAO later

## Start the Interactor

- Create Interactor that implements the Input Boundary
- Leave empty for now
    - Other than `return null` in method `execute`
- Will fill this in soon, but first a test!

## Write a Test

- Now have all the Interactor parts
- Write a JUnit test
    - Like the ones in lab5 code
- These all follow a pattern:
    - Create Input Data object
    - Create a Presenter that implements the Output Boundary
        - Job is to validate the Output Data
    - Create Interactor, injecting Presenter and any necessary DAOs
    - Invoke Interactor, passing in the input Data
- Run the test
    - Will fail as `execute` is not implemented yet

## Finish the Interactor

- Complete method `execute`
- For our example:
    - Get BankAccount objects from the DAO
    - Create a Money object
    - Subtract money from one account and add it to the other
    - Create an OutputData object
    - Call Presenter through OutputBoundary
- If you get this right, your test will pass
- Write any failing test cases
    - e.g., Not enough money to transfer
    - May require updating Interactor code

# Hook the Interactor Up to the UI

- Now create the View, Controller, View Model, Presenter

## The View

- May need to create text fields, menus, etc
    - to get the bank account numbers and amount to transfer
- A button to click
    - Maybe called Transfer
    - Write an actionPerformed method that gathers the bank account numbers and amount to transfer and invokes the Controller
    - Information may be in the View Model, or
        - Fetch it directly from the UI fields
        - Either choice is fine

## The Presenter

- Write the Presenter
- Will be called by the Interactor
- Updates View Model and then instructs it to notify the View

## The View (Again)

- View needs to add itself as an observer to the View Model
    - If it hasn’t already
- View needs to update based on new View Model values
- Write any necessary methods to do this
    - e.g., Get the state, update any (text) fields necessary

# Create the Whole Thing in a Factory or Builder

- View Model and DAOs can be instantiated first
    - Because they depend on nothing
- Presenter only depends on View Model → Create it next and inject the View Model
- Interactor needs the DAOs and the Presenter → Create Interactor and inject them
- Controller needs Interactor → Create it next and inject Interactor
- View needs View Model and Controller → Create it next and inject them
