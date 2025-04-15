---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 4
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-26
Date created: Sun., Sep. 29, 2024, 5:49:12 pm
Date modified: Tue., Dec. 10, 2024, 7:36:48 pm
---

# First Year Testing

- Mostly focused on **unit testing**
    - When you call a function, does it behave correctly?
- Other kinds of tests:
    - End-to-end testing
        - Once you’ve got your app built, you would like to know that when you click a button, it goes and grabs the stuff in the database and displays the next week
    - Integration test
        - Make sure that code representing what should happen when user clicks a button and repo on the back end are working well together

# Selecting Test Cases

- Test for success
    - General cases, well-formatted input, boundary cases
    - Classics:
        - 0, 1, more
        - odd, even
        - beginning, middle, end
- Check for data structure consistency (**representation invariants**)
    - Soon as method is called, does the data satisfy the representation invariant
- Test for atypical behaviour
    - Does it handle invalid input (if required)?
    - Does it throw exceptions it is supposed to?

# Unit Testing

- Unit testing follows a pattern:
    - Lots of small, *independent* tests
        - Every time you run a unit test, you want to make sure you’re starting again
            - Do not want the execution of one test to affect whether the next test that gets run is affected
            - Want to write it so that you don’t care what *order* the tests happen
    - Report passes, failures, and errors
    - Some optional setup and teardown shared across tests
    - Aggregation (combines tests into test suites)
        - Happens naturally
        - Write a `JUnit` test class
        - Has test methods in their group
- We could accomplish all of this “by hand,” but this common structure inspired the development of `JUnit`
    - ==When you see a pattern, build a framework==
    - Where [[Java Collections Framework|JCF]], [[Java Graphical User Interfaces|Swing]] came from

# JUnit

- Like `pytest` or `unittest` from Python
- May encounter JUnit4 or JUnit5

## Generate JUnit Tests in IntelliJ

1. Create a test directory at the root of your project
    - Mark as Test Sources Root
    - Will soon have the same package hierarchy as src
    - ![](https://i.imgur.com/NmSl2Qs.png)

2. Select a class to test and choose Create Test
    - ![](https://i.imgur.com/o61mZ7S.png)

3. Use JUnit5
    - ![|400](https://i.imgur.com/mAK6Iz3.png)

```java
package entity; 

class CommonUserTest { 
    
    private CommonUser user; 
    
    @BeforeEach 
    void init() { 
        user = new CommonUser( "Paul", "password", LocalDateTime.now()); 
    } 
    
    @Test 
    void getName() { 
        assertEquals("Paul", user.getName()); 
    } 
    
    @Test 
    void getPassword() { 
        assertEquals("password", user.getPassword()); 
    } 
```

*Placed in the test/entity directory (but with empty bodies to fill in).*

- `@BeforeEach` methods are called before every `@Test` method
- `@AfterEach` exists too

# Testing Code with Exceptions

```java file:ExceptionDemoTest.java
class ExceptionDemoTest {

    @Test
    void exceptionTest() {
        Calculator calculator = new Calculator();

        // Assert that the calculator.divide call throws an ArithmeticException
        Exception exception = assertThrows(
            ArithmeticException.class,
            // Anonymous method called by the assertThrows method
            () -> calculator.divide(1, 0)
        );
        assertEquals("/ by zero", exception.getMessage());
    }
}
```

```java file:Calculator.java
class Calculator {
    public void divide(int i, int j) {
        int result = i / j;
    }
}
```

# Setup and Teardown

- There are three steps in running a test:
    - **Setup**
    - **Run**
    - **Teardown**
- Setup phase
    - is in a single method annotated with `@BeforeEach`
- Teardown phase
    - is in a single method annotated with `@AfterEach`
- These are called before and after every test method
- Methods annotated with `@BeforeAll`
    - Run once before all test methods in that test class are executed
- Methods annotated with `@AfterAll`
    - Run once after all test methods are executed
- `@Before*` and `@After*` methods are used to avoid repetition
    - e.g., To create/destroy data structures required for more than one test method

# Assertion Methods

- Single-outcome assertions
    - `fail();` or `fail(msg);`
    - e.g., If statement → if you’re in this situation, then it’s definitely broken → fail
- Stated outcome assertions
    - `assertNotNull(object);` or `assertNotNull(msg, object);`
    - `assertTrue(booleanEx);` or `assertTrue(msg, booleanEx);`
- Equality assertions
    - `assertEquals(exp, act);` or `assertEquals(msg, exp, act);`
- Fuzzy equality assertions (for floating-point numbers)
    - `assertEquals(msg, expected, actual, tolerance);`

# Test-Driven Development

- Write tests first
- Then, start implementing to pass one of those tests
- Tests…
    - Are based on requirements rather than code
    - Determine the code you need to write
- Later think of a situation that code does not handle → add a test for it
- Approach…
    - aids in the definition of requirements
    - provides tangible evidence of progress
