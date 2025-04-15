---
dg-publish: true
tags: ["#lecture", "#note", university]
Course Code:
  - "[[CSC207]]"
Week: 1
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-03
Quercus Page: https://q.utoronto.ca/courses/353377/pages/lecture-materials-for-week-1?module_item_id=5804013
Slides/Notes:
  - "[[01-Introduction.pdf]]"
  - "[[02-Git.pdf]]"
Date created: Tue., Sep. 3, 2024, 12:59:52 pm
Date modified: Wed., Oct. 30, 2024, 5:51:47 pm
---

> [!goal]- Learning Objectives
>
> 1. The software development lifecycle
> 2. What Java classes look like
> 3. Constructors in Java
> 4. Version control and Git

For course overview, see [[Course Overview]].

> [!info]- Week Info
> `$= dv.current()['Quercus Page'] ? "- [Quercus Page](" + dv.current()['Quercus Page'] + ")" : "No link to Quercus page"`

---

# Software Development Life Cycle

> [!question]+ Questions to be answered this week:
>
> - Who is on the software development team and what do they contribute?
> - What are the main steps in the software development lifecycle? What is the goal of each one?

- [[Software Development Team]]
- [[Software Development Lifecycle]]

# Java

> [!question]+ Questions to be answered this week:
>
> - What is the structure of a Java class? (declaration, variables, …)
> - What are the possible accessibility modifiers and primitive types in Java?
> - What is the difference between the equals method and the `==` operator?
> - All Java classes are subclasses of “Object”. We call Java classes “reference types” as opposed to “primitive types”. What is the difference between the two?
> - How is a constructor different from a method? Include information about what each does as well as the difference in syntax.

- [[Introduction to Java]]

# Version Control and Git

> [!question]+
>
> 1. Why do we use version control? How can we benefit from it? Give at least 7 features.
> 2. What do the four states in the “git status” report mean?
>     - `untracked`
>     - `staged`
>     - `tracked`
>     - `dirty`/`modified`
> 3. What do these git commands mean?
>     - `clone` --
>     - `pull` --
>     - `add` --
>     - `commit` --
>     - `push` --
>     - `branch` --
>     - `merge` --
>     - `status` --
>     - `checkout` -- switch branch
> 4. A conflict is a set of characters you need to delete. Where do these characters come from?

- [[Version Control]]
- [[Git]]
