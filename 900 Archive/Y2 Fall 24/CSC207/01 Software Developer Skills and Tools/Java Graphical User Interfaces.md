---
dg-publish: true
tags: ['lecture', 'note', cs, java, university]
Course Code:
  - '[[CSC207]]'
Week: 4
Module:
  - '[[1 - Software Developer Skills and Tools]]'
Date: 2024-09-24
Date created: Tue., Sep. 24, 2024, 10:28:20 pm
Date modified: Wed., Oct. 30, 2024, 5:51:47 pm
---

> [!goal]- Learning Objectives
>
> - Understand that Java Swing framework has classes for building user interfaces
>     - Buttons, text fields, labels, etc. called **components**
> - be able to use **nested panels** and **layout managers** to organize a UI in a visually structured way
> - Be able to implement an event listener to respond to a button click

---

# Java Swing

> [!def]- Swing
>
> - Graphical User Interface (GUI) toolkit for creating user interfaces

- Most class names begin with a `J`
    - e.g., `JButton`, `JLabel`, `JTextField`, etc.
- **Component**:
    - A graphical user interface class
- Event-driven
    - A method gets called automatically when an event happens
    - e.g., program crashing, incoming email (for an email server), button clicks

[Java GUI Examples](https://github.com/paulgries/JavaGUIExamples)

## Swing Example

![](https://i.imgur.com/UC7pzsn.png)

- `JPanel`
    - Panel is ==holding the other components==
    - Can also add `JPanel`s to `JPanel`s
    - Example uses `JPanel` to organize `JLabel` and `JButton`
    - **Flow layout**: Goes left → right
- `JFrame`
    - Represents a window (top-level window)
    - Has stuff inside of it, but also has title bar
    - Title bar:
        - String parameter when instantiating the `JFrame`
    - `JFrame.setVisible`
        - Show window: param. is `true`
        - Hide window: param is `false`
    - Can have multiple `JFrame`s in program
        - But also just one and keep replace what **content pane** is

# JPanels and Layout Managers

- `JPanel` is a **container**
    - Can add other components to it
    - Including other `JPanel`s
- Each `JPanel` has a **layout manager**
    - `FlowLayout`
        - Default
        - Components flow left → right as you add
        - Resizable; starts a new line if it runs out of horizontal space
    - `BoxLayout`
        - Advanced `FlowLayout`
        - Supports both horizontal and vertical flow
- Nested `JPanel`s
    - Can make some ugly but functional user interfaces

![](https://i.imgur.com/srrpzQO.png)
*`JPanel` uses `FlowLayout` by default.*

# Listening for a Button Click

- A **listener** is an ==object==
- When you *instantiate* the listener:
    - *Inject* it into a particular button object
- Each listener has a *method*, `actionPerformed`
    - Gets called when button is clicked
    - JVM does this for us
- Inside each `actionPerformed` method,
    - May want to look at the values in text fields and other components
        - e.g., checkboxes and option buttons
    - ⇒ Need to ==name== them using variables in an *enclosing* scope
        - From example above, text fields do not have a variable referencing it
            - Cannot use `JTextField.getText`

![](https://i.imgur.com/x3MXKzP.png)
*Making an anonymous implementation of the `ActionListener` interface.*

- `ActionListener` is an interface
- `new ActionListener` creates an **anonymous class**
    - We do not name it
- Java lets us declare implementing classes for interfaces using this syntax
    - When you have a class instantiated like the example above, it is allowed to ==access variables in the *enclosing scope*, incl. local variables declared so far==
