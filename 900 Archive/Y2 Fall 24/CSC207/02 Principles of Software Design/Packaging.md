---
dg-publish: true
tags: [cs, java, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 7
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "14"
Chapter: 
Slides/Notes:
  - "[[14-Packaging.pdf]]"
Date: 2024-10-17
Date created: Sun., Oct. 20, 2024, 11:27:41 pm
Date modified: Sat., Dec. 21, 2024, 2:23:54 am
---

# Packages in Java

![|400](https://i.imgur.com/E5GgpEH.png)

*Homework 5 Project Structure: `src/main/java/`*

- Everything inside `src/main/java/` is a package

> [!def]+ Package
> - Contains related classes and interfaces
> - Full name of a class includes the package name: `java.util.ArrayList`
>     - ArrayList is in the `java.util` package
> - Path from sources root to a give class, delimited by “`.`”

- When you get to a dozen or more classes, package organization is essential
    - e.g., `use_case.signup`, `use_case.login`

```java file:LoginInteractor.java
package use_case.login;  
  
import entity.User;  
  
/**  
 * The Login Interactor. 
 * */
public class LoginInteractor implements LoginInputBoundary {

// ... more code
```

> [!question] What does Java actually do with packages? Why are they there?

# Access Modifiers Revisited

- & If we have things that are related to each other and connected → Should be in the same package and prevent anything from outside the package from accessing it
    - e.g., Private helper classes:
        - Implementation of a LinkedList: Node class can be package-protected since no one else but the LinkedList needs to know about it
- Classes can be:
    - **public**
    - **package-private** (no explicit modifier)
- Members of a class (i.e., methods, variables) can be:
    - **private**
        - Within class only
    - **protected**
        - Within package and in *subclass* outside of package

> [!important]+ There is no concept of providing access specifically to “subpackages”
> - Other than for organization purposes, the packages `use_case.signup` and `use_case` have ==no connection==
>     - e.g., If we defined a class in the `use_case` package, we have no way of saying that classes in `use_case.signup` can access it, but not classes in the `app` package
>     - All or nothing: Must make the class public

# Example. Online Book Store

When building an online book store, one of the use cases we’ve been asked to implement is about customers being able to view the status of their orders

## How Could You Package This Code?

- `OrdersController`
    - A web controller; something that handles requests from the web
    - In this case, `OrdersController` also acts as a Presenter
- `OrdersService`
    - An interface; defines the *business logic* related to orders
- `OrdersServiceImpl`
    - Implementation of orders service
    - Use Case Interactor
- `OrdersRepository`
    - An interface; defines how we get access to persistent order information
    - i.e., Data Access Interface
- `JdbcOrdersRepository`
    - Implementation of the repository interface that saves information in a database

![|center|200](https://i.imgur.com/LJvDfYo.png)


![](https://i.imgur.com/iWHDMqB.png)

- **Inside/Outside**:
    - Similar to what we do in CA
    - Bundle every interface that use case interactor implements or relies on
    - i.e., Defining entire domain; all the details of how these interfaces are used/implemented reside in other packages
    - Similar to by Layer, except
        - `OrdersRespository` moves from `data` package to `domain` (i.e., business rules), and
        - `data` is renamed to `database` (because now it is only the implementation detail of how we are getting our data: from a database)
- **By Component**:
    - OrdersController is its own component that uses the Orders component
        - Logic in Controller shouldn’t be tied to logic in Orders → Different package
    - If we make a new Orders Controller, then we can implement that and still connect it to our Orders component
    - Similar to Inside/Outside, except you group in the database

# Screaming Architecture

[Reading](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html)

- The structure of the code should primarily reflect *what* the program is doing
    - i.e., What the domain/application is
    - Rather than *how* it is doing it
        - i.e., What the frameworks and details of the implementation involve
- Want to organize our code so that if someone new to the program is looking at it, they know what our program is about (domain, use cases, what can you do with this program)

![|250](https://i.imgur.com/czRPhqi.png)

*Non-screaming version.*

- All we know is that this program uses Django
- No idea about the domain

![|300](https://i.imgur.com/4UxwOcA.png)

*Screaming version.*

## Package by Layer of Clean Architecture

![|300](https://i.imgur.com/8JUiljX.png) ![|300](https://i.imgur.com/SNosvE4.png)

# Conclusions from Ch. 34 from Clean Architecture Textbook

> [!abstract]+ Packages can potentially provide both *organization* and *encapsulation* (using access modifiers)
> - Organization by screaming architecture
> - Encapsulation by access modifiers to hide implementation details, if you wanted to

> [!tip] Choose your access modifiers to reflect your design, ==exposing only the necessary methods==

- i.e., Make everything private, unless you have a reason to give someone else access to it

# Aside: Project Structures

- In IntelliJ, you can create a project which will use one of three build systems:
    - IntelliJ (default)
    - Gradle
    - Maven
- Each of these define their own structure for how they organize your files
- In all cases: Have a Sources Root and Test Sources Root marked
- Within these: Have your usual Java package structure
    - & Parallel directory structures in your Sources Root and Test Sources Root directories will correspond to the same Java packages
    - When you’re testing things, you may want to access things only accessible within the package → Put test class in the same package as you are testing

![|300](https://i.imgur.com/x5QkBGv.png) ![|300](https://i.imgur.com/ldy06yA.png)
