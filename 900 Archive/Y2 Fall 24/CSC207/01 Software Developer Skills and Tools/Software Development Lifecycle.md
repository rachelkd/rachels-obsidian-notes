---
dg-publish: true
tags: ["#lecture", "#note", cs, university]
Course Code:
  - "[[CSC207]]"
Week: 1
Module:
  - "[[1 - Software Developer Skills and Tools]]"
Date: 2024-09-03
Date created: Wed., Sep. 4, 2024, 8:52:34 pm
Date modified: Wed., Oct. 30, 2024, 5:51:46 pm
---

![](https://i.imgur.com/4ymcnhv.png)

# What Are the Main Steps in the Software Development Lifecycle? What is the Goal of Each One?

### <mark style="background: #D2B3FFA6;">1. Planning and analysis</mark>

**Goal:** evaluate feasibility of project, revenue potential, needs of end users

- Are we actually doing this project or not? And what is required of it?
    - Business of Software (4th year course)
- What does the client want built, and what problems will it solve?
- What features will the end users need from the product?
- Roughly how long will it take to build?
- What will it cost to build it?
- What is the revenue potential?

### <mark style="background: #D2B3FFA6;">2. Defining requirements</mark>

**Goal:** Create clear requirements for the dev team

- Arguably most crucial part of every project: defining what we are doing
- What does it mean to be successful in the project?
- What is the functionality that this project is actually going to provide
- Document the use cases for the software
    - What will the end users want to do?
- Make a list of specific tasks

More in CSC318 Design of Interactive Computational Media

### <mark style="background: #D2B3FFA6;">3. Design</mark>

**Goal:** Decide on tech needs, develop a prototype

- Decide on your “stack”
    - iOS, Android, and/or web
    - Server hosting
        - Google Cloud, Amazon AWS, Microsoft Azure, self-hosted
    - Programming language(s)
- Develop a prototype
    - No programming, just design
    - Draw some pictures to capture what the screens will look like
    - Validate prototype with customer

CSC318, CSC309 Web Programming

### <mark style="background: #D2B3FFA6;">4. Development</mark>

**Goal:** Develop the actual product

- Design and grow a product that looks and behaves like the prototype
- Manages real data
- Apply fancy techniques learned in CSC207
    - ==Maintainable== (==modular==, good programming style and ==documentation==)
    - ==Testable==
    - If your client wants something to be added, it can be done without re-designing everything and in an acceptable amount of time → *clean architecture*
- Often biggest part of the work
- Primary focus of CSC207

### <mark style="background: #D2B3FFA6;">5. Testing</mark>

**Goal:** Make sure it works

- Performance, functionality, security, usability, acceptance
- [[CSC207]] focuses on *unit-testing*

More in CSC301 Software Engineering

### <mark style="background: #D2B3FFA6;">6. Deployment</mark>

**Goal:** Deliver it to end users

e.g.,
- Publish in app store
- Create and send an executable
- Update a website

More in CSC301

### <mark style="background: #D2B3FFA6;">7. Maintenance</mark>

**Goal:** Keep it going

- Track and address bugs
- Ensure client needs are being met
- Plan for new features