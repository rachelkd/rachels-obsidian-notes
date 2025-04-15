---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 5
Module:
  - "[[2 - Principles of Software Design]]"
Chapter: 
Slides/Notes:
  - "[[10-Exceptions.pdf]]"
Date: 2024-10-03
Date created: Wed., Oct. 9, 2024, 1:01:19 am
Date modified: Tue., Dec. 10, 2024, 6:29:34 pm
---

> [!goal]- Learning Outcomes
> - Understand how `Exception`s work in Java
> - Be familiar with the `Throwable` hierarchy
> - Know the difference between checked and unchecked exceptions
>     - And when to use each of them

# Throwable Hierarchy

![](https://i.imgur.com/lq7y0Yu.png)

- `RuntimeException`s
    - Are predictable
    - If you program correctly and check value of variables → Never get a runtime exception

> [!question]+ A question to think about:
> You’re writing code to read from a file. That file might not exist for whatever reason.
> - Is there a way for you to predict whether the file exists right before you open it, and make sure it still exists when you try to open it?
> - Or, in that millisecond in between, could the user accidentally have deleted the file?

# Exceptions

> [!def]- Exceptions
> - Report **exceptional conditions**
>     - Unusual, strange, unexpected
> - Conditions deserve exceptional treatment
>     - Not usual go-to-the-next-step, plod-onwards approach

- Understanding exceptions requires thinking about a different model of program execution

> [!tip] Some methods throw exceptions

```java
public boolean addAll(int index, Collection<? extends E> c)
Inserts all of the elements in the specified collection into this list,
starting at the specified position. Shifts the element currently at the index
```

- Throws:
    - `IndexOutOfBoundsException` - if the index is out of range (`index < 0 || index > size()`)
    - `NullPointerException` - if the specified collection is `null`

## Throwing Exceptions

###### To *throw* an exception

`throw new Throwable(message);`

###### To *catch an exception* and Deal with it

```java
try {
    // statements
} catch (Throwable parameter) {  // The catch belongs to the try.
    // statements
}
```

###### To Say a Method is not Going to Deal with Exceptions (or May Throw Its Own)

```
accessModifier returnType methodName(parameters) throws Throwable {
    ...
}
```

### Analogies

- `throw`
    - In trouble → Throw rock through a window with a message tied to it
    - Like return statement (exits current method), but bad
- `try`
    - Someone in the following block of code might throw rocks of various kinds
    - Catchers line up ready for them
- `catch`
    - If a rock of my kind comes by, I will catch it and deal with it
- `throws`
    - Warning: If there’s trouble, I may throw a rock

### An (Awkward) Example

```java
int i = 0;
int sum = 0;
try {
    while (true) {
        sum += i++;
        // We're done
        throw new Exception("i at limit");
    }
} catch (Exception e) {
    System.out.println("sum to 10 = " + sum);
}
```

- Bad code because:
    - Situation that the exception reports is *not exceptional*
        - Obvious that `i` will eventually be 10 $\implies$ Expected
        - In Java, ==exceptions are reserved for exceptional situations==
    - Uncharacteristic
        - Real uses of exceptions are not local
        - `throw`, `catch` are not generally in the same block of code

## Why Use Exceptions?

- Less programmer time spent on handling errors
- Cleaner program structure
    - Isolates exceptional situations rather than sprinkling throughout code
- Separation of concerns
    - Pay local attention to the algorithm being implemented and global attention to errors that are raised

## Cascading Catches

Suppose `ExSup` is the parent of `ExSubA` and `ExSubB`.

```java
try {
    // ...
} catch (ExSubA e) {
    // We do this if an ExSubA is thrown.
} catch (ExSup e) {
    // We do this if any ExSup that is not an ExSubA is thrown.
} catch (ExSubB e) {
    // We never do this, even if an ExSubB is thrown.
} finally {
    // We always do this, even if no exception is thrown.
}
```

## `finally`

- After last `catch` clause, you can have a `finally` clause
- Not like `else` on `if` statement
- ==Always executes== whether an exception was thrown or not, or thrown and caught
    - e.g., Use `finally` to close open files as a clean-up step

# Recap

- If you call code that may throw a non-Runtime exception, you have two choices:
    - Wrap code in `try-catch`, or
    - Use `throws` to declare that code may throw an exception
- Exceptions do not follow normal control flow
- Guidelines for using exceptions:
    - Use exceptions for exceptional circumstances
    - Throwing and catching should not be in same method

# Throwable Methods

- Constructors:
    - `Throwable()`, `Throwable(String message)`
- Methods:
    - `getMessage()`
    - `printStackTrace()`
    - `getStackTrace()`
- Can also record and look up within a `Throwable` its *cause*
    - Another `Throwable` that caused it to be thrown
    - Through this, can record and look up a chain of exceptions

# Checked vs. Unchecked Exceptions

- **Checked exceptions**
    - Method *must* include `throws ExceptionClassName` in its declaration
    - Compiler will check to make sure that the Exception is caught by the method that calls it, or the method that calls that method, or …
- **Unchecked exceptions**
    - Program will throw these without checking to see if it is eventually caught
    - Will crash program and print stack trace to terminal
        - Can then see where Exception was thrown and fix code so that is does not happen again
        - Useful for debugging

> [!important] You don’t have to handle errors or runtime exceptions

- **Error**
    - “Indicates serious problems that a reasonable application should not try to catch”
    - Programmer does not have to handle these errors because they are “abnormal conditions that should never occur”
- **RuntimeException**
    - Called *unchecked* because you are not required to catch them
    - Typically indicate that something bad and unexpected happened
        - e.g., Not being able to read from text file because of a permission problem, not being able to send a message because network connection failed
    - Typically handled at one place in your program
    - Details are logged so that programmers can investigate

# What Should You Throw?

- Can ==throw an instance of `Throwable`== or any subclass of it
    - Whether an already defined subclass, or subclass you define
- ==Do not throw an instance of `Error` or any subclass of it==
    - Error $\implies$ unrecoverable circumstances
- Do not throw an instance of `Exception`
    - ==Throw something more specific==
- Okay to throw instances of:
    - Specific subclasses of `Exception` already defined
    - Specific subclasses of `Exception` that you define

# Extending Exception

**Example.** A method `m` that throws your own exception `MyException`, a subclass of `Exception`.

## Version 1

```java
class MyException extends Exception {...}

class MyClass {
    public void m() throws Exception {
        // ...
        if (...) throw new MyException("Oops!");
        // ...
    }
}
```

## Version 2

```java
class MyClass {
    public static class MyException extends Exception {...}
    
    public void m() throws MyException {
        // ...
        if (...) throw new MyException("Oops!");
        // ...
    }
}
```

# Aside: Classes Inside Other Classes

> [!tip] You can define a class inside another class

- Two kinds:
    - **Static nested classes**
        - Use keyword `static`
        - ==Cannot access any other members== of enclosing class
    - **Inner classes**
        - Do not use keyword `static`
        - ==Can access all members== of enclosing class (including private ones)
- Nested classes increase *encapsulation*
    - Make sense if you ==won’t need to use class outside its enclosing class==

# Documenting Exceptions

```java
/**
 * Return the mness of this object up to mlimit.
 * @param mlimit The max mity to be checked.
 * @return int The mness up to mlimit.
 * @throws MyException If the local alphabet has no m.
 */
public void m(int mlimit) throws MyException {
    // ...
    if (...) throw new MyException("oops!");
    // ...
}
```

- Javadoc comment is for human readers.
- Keyword `throws` is for compiler
- Both the reader and the compiler are checking that caller and callee have consistent interfaces.

# Extend Exception or RuntimeException?

- Recall: ==`RuntimeExceptions` are *not* checked==

> [!question] How do you choose whether to extend `Exception` or `RuntimeException`?

- Should we always extend `Exception` so that they are *checked* and benefit from compiler’s exception checking?

- `Exception`
    - Class `Exception` and its subclasses are a form of `Throwable` that ==indicate conditions that a reasonable application might want to catch==
- `RuntimeException` (*not checked*)
    - RuntimeException is the superclass of those ==exceptions that can be thrown during the normal operation of JVM==
    - e.g., `ArithmeticException`, `IndexOutOfBoundsException`, `NoSuchElementException`, `NullPointerException`
    - Exempted from compile-time checking because having to declare such exceptions ==would not aid significantly in establishing the correctness== of programs
        - Info available to compiler and level of analysis the compiler performs are not usually sufficient to establish that such RuntimeExceptions cannot occur, even though it might be obvious to programmer
        - Requiring such exception classes to be declared would be an irritation to programmers
    - RuntimeExceptions often ==indicate precondition violations==
- non-`RuntimeException` (*checked*)
    - e.g., `IOException`, `NoSuchMethodException`

> [!summary]+
> - Use `Exception`s for conditions from which the caller can reasonably be expected to recover
> - Use `RuntimeException`s to indicate programming errors

> [!tip]+ If the programmer could have predicted the exception, do not make it checked!
> - Suppose method `getItem(int i)` returns an item at a particular index in a collection and requires that `i` be in some valid range
>     - Programmer can check that range before they call `o.getItem(i)`
>     - Passing an invalid index should not cause a checked exception to be thrown in this case

> [!tip]+ Avoid unnecessary use of checked exceptions
> - If user did not use the API properly, or if there is nothing to be done
>     - Make `RuntimeException`

## Example of a RuntimeException if Declared Exception

- Imagine code that implements a circular linked list
- Suppose that, by construction, this structure can never involve null references
- Programmer can then be certain that a `NullPointerException` cannot occur
- But, would be difficult for compiler to prove it!
- If `NullPointerException` were checked, programmer would have to *handle* them (catch or declare that one might be thrown) *everywhere* that a `NullPointerException` might occur

# SOLID and Exceptions

Recall [[Solid Design Principles]]

- SRP:
    - Yes
    - Allows separation of normal return conditions and exceptional situations
    - → Programmers are not trying to deal with both normal and exceptional cases using return mechanism
    - Exception classes are created in the context of a problem specification
        - Used to indicate violates of the core business logic of a system
        - Actor is the person responsible for deciding on that core business logic
        - No behaviour (so no methods) in exceptions
- OCP
    - Yes
    - Can extend `Exception` or `RuntimeException` to enrich how our programs handle various exceptional situations
    - We throw `Exception` objects → Inheritance also applies to exceptions
    - Catch block that originally caught on subtype of `Exception` will be able to catch subsequent subtypes if we decide to make exceptions more specific by extending the ones we were already using
    - Follows OCP → Can add new subtypes of `Exception` and throw them in place of their superclasses without having to change our catch blocks
- LSP
    - Can we replace instances of an exception with instances of a subclass without causing the program to break?
        - Yes
    - Required (using `throws` keyword) to declare when we throw checked exceptions
    - Not required when we may throw unchecked exceptions (can still declare `throws` though)
    - Having checked exceptions above unchecked in the hierarchy is consistent with LSP
        - Opposite would not be
    - Subclass that overrides a method which throws a checked exception is ==only allowed to throw exceptions which are unchecked== or ==`instanceof` the original exception== that could be thrown in parent method
        - `throws` clause in method signature → Cannot throw a plain Exception or any other non-Runtime exception in an overriding method
        - Code will not compile
        - Prevents us from introducing Liskov violations
- ISP
    - Yes
    - Public interface of `Throwable` is focused
    - Have little to nothing to implement ourselves when subclassing an `Exception` or `RuntimeException`
    - Exceptions declared to be thrown by methods in an interface should be relevant to all its potential implementers
        - $\implies$ Implementing classes do not need code to handle irrelevant exceptions
- DIP
    - Maybe?
    - Do not have any abstractions → DIP not directly relevant

# Who is the Exception For?

- **Throwable Cause**:
    - Another throwable that caused this throwable to be constructed
    - A throwable can have a cause, forming a chain of exceptions
- **Layered Abstraction**:
  - Bad design to let lower layer exceptions propagate outward
      - Unrelated to upper layer abstraction
      - Ties API to implementation details
- **Wrapped Exception**:
  - i.e., An exception containing a cause
  - Allows upper layer to communicate failure details without exposing lower layer exceptions
  - Maintains flexibility in upper layer implementation
  - Preserves API consistency (specifically, set of exceptions thrown by its methods)

# DIP Example

- **SQL Database Interaction**:
    - Classes interacting with SQL databases may encounter `SQLException`.
    - Client code should not handle `SQLException` directly as it’s an implementation detail.

- **Abstraction with Custom Exception**:
    - Define a `DataAccessException` class.
    - Wrap `SQLException` in `DataAccessException` to abstract database details.

```java
try {
    doSomeSQLDatabaseStuff();
} catch (SQLException e) {
    // Wrapping the low-level exception in a custom exception
    throw new DataAccessException("Failed to save user to the database.", e);
}
```

- **Client Code Abstraction**:
    - Allows client code to work abstractly with different databases

```java
public void registerUser(User user, DatabaseInterface myDatabase) {
    try {
        myDatabase.saveUser(user);
    } catch (DataAccessException e) {
        // Handle the error in a way relevant to the service layer
        System.out.println("Error: Unable to register user. Please try again later.");
        // Log exception details or take other actions as needed
    }
}
```

- **Database Interface**:
    - Defines a method that throws `DataAccessException`

```java
interface DatabaseInterface {
    void saveUser(User user) throws DataAccessException;
}
```
