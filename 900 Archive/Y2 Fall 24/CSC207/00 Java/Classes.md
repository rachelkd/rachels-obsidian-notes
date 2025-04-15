---
Date created: Sat., Oct. 5, 2024, 7:24:18 pm
Date modified: Tue., Dec. 10, 2024, 4:42:01 pm
dg-publish: true
tags: [cs, java, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 3
Module:
  - "[[Java]]"
Date:
Chapter: 2
---

# Variables in Classes

- Have to declare variables before using them in Java
- Two kinds of variables we can declare for classes:
    - **Instance variables**
        - Akin to attributes from Python
        - Every instance of the class will contain its own instance of each of these variables
        - Come into existence when instance is constructed (`new` keyword)
    - **Class variables**
        - a.k.a **static** variables
        - All instances of a class *share* a *single* instance of each class variable
        - Updating this variable in one instance of a class will reflect across *every* instance of the class
        - Useful if we want instances to accumulate a value together

# Visibility

- Attributes and methods can either be:
    - **public**
    - **private**
        - Python: leading `_` to indicate private
    - Java: use `public` or `private` keywords to make this distinction
- Private variables *cannot* be accessed from outside of a class
- Can also use `protected` keyword
    - Attribute or method accessible to the entire package and class’ subclasses
    - Not anything else

# Constructors

- **Constructors**
    - Similar to `__init__` method in Python
    - Methods with no return type (not even void)
    - Get called whenever a new instance of a class is created
- Can define as many constructors as we want
    - So long as the method signatures are different
    - Different number of arguments,
    - Having different argument types, or
    - A combination of both

```java
public Something(String name, int size) {
    this.name = name;
    this.size = size;
}
```

- Python: Used `self` keyword
- Java: `this` keyword is equivalent

<!-- break -->

- Could create additional constructors

```java
public Something(String name) {
    this.name = name;
    this.size = 1;
}
```

- Could rewrite the above to call on the first constructor we wrote:

```java
public Something(String name) {
    this(name, 1);
}
```

# Overloading Methods

- **Overloading** the constructor:
    - When we define multiple constructors
- Can do for any method, not just *constructors*

```java
public int my_method(int something, int something_else){
    return something + something_else;
}

public int my_method(int something){
    return my_method(something, 1);
}
```

# Overriding Methods

- **Overriding** a method
    - Re-define a method from a parent class
- Include `@Override` annotation
    - Lets Java know that we are overriding an *inherited* method
    - Annotation will also enforce that we use the correct *method signature* for the override method
    - Can be certain that we do not have any silly typos in our method or incorrect argument types

## `toString`

- `toString` method
    - Equivalent to Python’s `__str__`
    - Gives string representation of our object

```java
@Override
public String toString() {
    return "My name is " + this.name;
}
```

*Method takes no parameters and returns a `String`*

## `equals`

- `equals` method
    - Equivalent to Python’s `__eq__`
    - Method is *not* called implicitly in Java
        - `==` checks for identity equality
            - i.e., IDs of the objects are the same
    - Need to explicitly call the `equals` method to check for equality
        - e.g., `object1.equals(object2)`
- Default behaviour of `equals`:
    - Check of identity equality
    - Often want to override this to check certain attributes

> [!note]+ Designer of a class gets to decide what has to be true in order for two instances to be considered “equals”

- Any implementation of it must **obey these properties**:
    - **Symmetry**
        - For non-null references `a` and `b`, `a.equals(b)` iff `b.equals(a)`
    - **Reflexivity**
        - `a.equals(a)`
    - **Transitivity**
        - If `a.equals(b)` and `b.equals(c)`, then `a.equals(c)`

```java
@Override
public boolean equals(Object obj) {
    if (this == obj){
        return true;
    }
    if (obj == null){
        return false;
    }
    if (this.getClass() != obj.getClass()){
        return false;
    }

    MyClass other = (MyClass) obj;  // Here we're casting the type of obj to our class
    // And then we'll want to compare the attributes of this to those of other
    // For example:
    if (this.name != other.name){
        return false;
    } else if (this.something != other.something){
        return false;
    }
    return true;
}
```

*`equals` method will often look like this.*

## `hashCode`

- **hashCode** of an object is an *integer* value that obeys this property:
    - If two objects are equal (according to the equals method), they have the same hashCode
    - Converse is *not* always true!
    - Fine for two objects with same hashCode to not be equal
- Why do objects have a hashCode and why must it satisfy this property?
    - Some important classes such as `HashMap` use hashCode of an object to decide where to store it
    - Allows them to retrieve object later by comparing hashCodes → Avoid costly calls to the equals method
    - `HashMap` class does all this using a *data structure* called a “**hash table**”
        - Hash tables have remarkable properties → important in CS
    - Dictionaries in Python are one type of a HashMap
- If we do not override the hashCode method in a given class,
    - Default hashCode will be the one from the `Object` class

# Class (static) Methods

- Use `static` keyword to declare that a method is a **class method**
- Class method is associated with the class
    - *not* instance
- Call class methods by ==prefixing it with class name==

```java
public static int population(){
    return MyClass.count;
}
```

*Suppose we have the following class method.*

- How do we call the method?
    - `MyClass.population()`
    - Could also call `m1.population()`
        - Strange since `population` method does not depend on `m1` itself
    - & Instance method can reference a class variable (or call a class method)
        - Opposite is not true
        - & Class method *cannot* access an instance variable or call an instance method directly

```java
public static int population(){
    return this.some_attribute;
}
```

*This would **not** compile.*

- There is no `this` when you are in a class method!
- Only way for a class method to access an instance variable or call an instance method:
    - If a reference to an object is passed to the method
    - Through reference, instance variables and instance methods of the object are accessible
