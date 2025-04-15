---
dg-publish: true
tags: ["#lecture", "#note", cs, java, university]
Course Code:
  - "[[CSC207]]"
Week: 3
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-19
Date created: Fri., Sep. 20, 2024, 1:19:24 pm
Date modified: Tue., Dec. 10, 2024, 4:56:13 pm
---

# Java Collections Framework

> [!def]- Collection
> - An object that represents a group of objects
>     - e.g., set, list, or map

> [!def]- Collections framework
> - A unified architecture for representing and manipulating these collections in a unifying manner
>     - Enables collections to be manipulated independently of implementation details

- Examples of classes and interfaces in the framework:
    - `List`
    - `ArrayList`
    - `LinkedList`
    - `Set`
    - `Map`
    - `HashMap`

# Iterable and Map Interfaces

![](https://i.imgur.com/kmrYOdp.png)

- Specifying what it means to be a `Collection`
    - A `Collection` is iterable and unordered
- In Java, an iterable can use `for (T x : iterableObject)`
- In Python, we use `for x in iterable_object`

# Map Hierarchy

![](https://i.imgur.com/0KDTDW9.png)

- Maps are not collections
- `TreeMap` *implements* a `NavigableMap` and also *extends* the `AbstractMap`
- `AbstractMap` (abstract class) implements the `Map` interface
    - Probably providing some kind of partial implementation
- Abstract classes are classes you are not supposed to say `new` with
    - e.g., `new Shape` should not be written in code (from [[Interfaces#Demo. Shapes.]])

# Set Hierarchy

![](https://i.imgur.com/YbzHPwg.png)

- Set is a collection that is unordered
    - Lists are not sets; sets are not lists

# What Can You Do with a Set?

- Already have an idea what you might want to do with a `Set` and a `Queue`
    - Actual method names are different, but operations are there
- Both interfaces extend `Collection`, which is unordered

## HashSet

- Implements the `Set` interface, backed by a hash table (actually a `HashMap` instance)
    - e.g., Python `dict`
    - Every class that you write should probably override the hash code method
- Hash function:
    - Used to determined where in this backing array to put this thing
    - Gives you an index
    - Nicely distributed along the implementation of the hash table ⇒ constant time for basic operations
- Makes no guarantees as to the iteration order of the set
    - Does not guarantee that the order will remain constant over time
- Class permits the null element
- Class offers ==constant time performance== for basic operations
    - e.g., `add`, `remove`, `contains`, `size`
    - Assuming hash function disperses elements properly among the buckets

## TreeSet

- A `NavigableSet` implementation based on a `TreeMap`
- Elements are ordered using their natural ordering, or by a `Comparator` provided at set creation time, depending on which constructor is used
- Implementation provides guaranteed $\log(n)$ time cost for basic operations i.e., `add`, `remove`, `contains`

# Queue Hierarchy

![](https://i.imgur.com/taafSPx.png)

- `Deque` is a double-ended queue
    - Can be implemented by an `ArrayDeque`
- `PriorityQueue`
    - Every element has a priority
    - When you insert, you don’t know where it is going other than it will have a priority

# List Hierarchy

![](https://i.imgur.com/Q64dmX9.png)

## ArrayList vs. LinkedList

- ArrayList uses an array to store the contents
- When it runs out of room, it makes a new, bigger array and copies over the items. For add(e), get(i) and remove(i),
    - What are the best case running times? Worst? When does it perform well?
        - `add(e)` best case: Don’t have to resize array
        - `get(i)` best case: Always
        - `remove(i)`
- LinkedList uses a doubly-linked list. For add(e), get(i) and remove(i),
    - What are the best case running times? Worst? When does it perform well?
        - `add(e)` best case: Inserting at the beginning or end
            - Inserting in middle → bunch of links to run along before getting to spot where you want to insert
- When would you choose LinkedList over ArrayList?

# Primary Advantages of Defining a Framework

- Framework *reduces* the programming effort
    - Framework provides data structures and and algorithms
        - Don’t have to write them yourself
    - JCF has done all the heavy lighting to give us these basic CS concepts
- Increases performance
    - Provides high-performance implementations of DSAs
- Various implementations of each interface are interchangeable
    - ⇒ programs can be tuned by switching implementations
    - e.g., You write a bunch of code using an `ArrayList` and find that you are inserting at the beginning a lot.
        - Switch implementation to `LinkedList` is easy
- Provides interoperability between unrelated APIs by establishing a common language to pass collections back and forth
- Reduces effort required to learn APIs; requires you to learn multiple ad hoc collection APIs
    - Do not have to write your own collections because someone already did it
- Fosters software reuse
    - By providing a standard interface for collections and algorithms with which to manipulate them

# JCF Main Design Goal

- To produce an API that was small in size and *conceptual weight*

# What’s In the Framework

- Collection interfaces
    - Represent different types of collections
        - e.g., sets, lists, maps
    - Interfaces form the basis of the framework
- General-purpose implementations
    - Primary implementations of the collection interfaces
- Legacy implementations
- Special-purpose implementations
- Concurrent implementations
    - Designed for highly concurrent use
    - Can have a program that consists of multiple mini programs running alongside each other
        - Called a thread
        - Concurrent: executing at the same time
- Wrapper implementations
- Convenience implementations
- Abstract implementations
- Algorithms
    - Static methods that perform useful functions on collections
    - e.g., `sort`
- Infrastructure
- Array utilities

> [!important] Various implementations of each interface are interchangeable
> - Programs can be tuned by switching implementations
> - To most easily encourage and take advantage of that, should ==choose a `Collection`s interface for (almost) every parameter type, return type, and variable type==

> [!question]- Why does `Map` not extend `Collection`?
> - What are the *elements* in `Map`?
>     - Key-value pairs ⇒ limited (and not useful) `Map` abstraction
>     - Cannot ask what value a given key maps to
>     - Cannot delete the entry for a given key without knowing what value it maps to
