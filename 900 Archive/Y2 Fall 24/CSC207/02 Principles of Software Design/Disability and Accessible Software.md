---
dg-publish: true
tags: [cs, ethics, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 9
Module:
  - "[[2 - Principles of Software Design]]"
Lecture:
  - "17"
Chapter:
Slides/Notes:
  - "[[17-Disability-Accessible-Software.pdf]]"
Date: 2024-11-05
Date created: Thu., Nov. 14, 2024, 1:38:38 pm
Date modified: Tue., Dec. 10, 2024, 7:57:03 pm
---

> [!abstract]+ One set of users whose needs may differ from the average user are those with disabilities
>
> - WHO estimates 15% of users have disabilities

# From [[User Diversity|Module 1]]

> [!summary]+ When we fail to design appropriately for users with disabilities, we risk causing both:
>
> - Tangible harms, and
> - Harm to relational equality

# Conceptual Analysis of Disabilities

- **Conceptual analysis**
    - Technique for understanding a concept you use
    - First step: Think of some examples of the concept you’re talking about
    - Conceptual analysis of disabilities: Start by having some examples of disabilities in the back of your mind
- Examples of disabilities:
    - Paraplegia (paralysis of lower limbs)
    - Deafness
    - Blindness
    - Mental illness
    - Speech impairment

## What Do Disabilities Have in *Common* with Each Other?

- Wasserman et al. 2006:
    - A disability is a ==physical or mental impairment== that is associated with a ==personal or social limitation== on the activities one can perform
    - **Physical or mental impairment**
        - e.g., Paraplegia
    - **Personal/social limitation**
        - e.g., Not being able to access public spaces
    - Important that it has *both* these components

<!-- break -->
- Normally think of the **cause** of an event as another specific event that occurred before it
- Other factors are simply **background conditions**
    - Required for event to happen, but not part of the cause

![](https://i.imgur.com/U6evSp2.png)

# The <mark style="background: #D2B3FFA6;">Medical Model</mark> of Disability

- In some disabilities, the limitations are *caused* by the ==physical or mental impairment==
- ==Human world== is a background condition

![](https://i.imgur.com/bgvvore.png)

# The <mark style="background: #D2B3FFA6;">Social Model</mark> of Disability

- Flips the cause and background condition
- In some disabilities, the limitations are *caused* by the ==human world==
    - Whether through stigma or environmental design
- ==Impairment== is a background condition
- e.g., For paraplegia, the impairment is the oxygen needed in fire analogy, and the human world is the lightning storm

![](https://i.imgur.com/6NhOVD1.png)

> [!important] For some impairments and some limitations, the **medical model** will seem more appropriate. Sometimes, the **social model** will seem more appropriate

# Using Medical and Social Models to Think About Interventions that Address Disability

- Medical Model:
    - Intervention reduces the impairment
- Social Model:
    - Intervention prevents impairment from causing limitations

> [!example]+ Prosthetic Leg
>
> - Medical model
>     - ==Intervention reduces the impairment==
> - “Closer” to the impairment
> - Can work for one person without working for everyone
> - Need to do something to trigger
> - Designed with a particular impairment in mind

> [!example]+ Elevators
>
> - “Further” from the impairment
> - Applies to everyone; cannot just apply to one person
> - Requires little work to trigger
> - May be designed with a particular impairment in mind or none in particular in mind

## Connection to Software Design

> [!question] When software is an intervention that helps to address disability, is it more like the prosthetic or the elevator?

- Some examples of software fit the medical model, and some examples fit the social model
- Useful to distinguish between ==software itself== and ==features within software==
    - e.g., Does Night Mode better illustrate the medical model or the social model?

# Software and the Medical Model

# Software Models for Accessibility

## Medical Model Examples

> [!info]+ Software Designed for Specific Impairments
>
> - **Braille Translation Software**
>     - e.g., Index BrailleApp
>     - Converts text to Braille-printable documents
> - **Simplified Phone Interface**
>     - e.g., Simple Smartphone
>     - Designed for users with Down Syndrome
> - **Predictive Text/Correction**
>     - e.g., Ghotit
>     - Learns from user patterns
>     - Assists with spelling/grammar
>     - Targeted for dyslexia
> - **Sip-and-puff Systems**
>     - e.g., Jouse3, Origin instruments
>     - Mouth-controlled joystick interface
>     - Replaces traditional mouse/keyboard
> - **Specialized Voice Recognition**
>     - e.g., MathTalk
>     - Domain-specific applications
>     - Converts speech to specialized text (math symbols)

## Social Model Examples

> [!info]+ Universal Design Features
>
> - **Automatic Captioning**
>     - e.g., Zoom
>     - Benefits all users who prefer visual text
> - **Display Customization**
>     - Zoom functionality
>     - Adjustable icon/text size
>     - Available in most operating systems
> - **Alternative Authentication**
>     - e.g., Biometric/fingerprint unlock
>     - More accessible than traditional passcodes
> - **Text-to-Speech**
>     - e.g., Speechify
>     - Universal audio conversion of text

> [!obs]+ Key Distinction
>
> - Medical Model: Targets ==specific impairments== with specialized solutions
> - Social Model: Creates ==universal features== that benefit all users regardless of ability status

# Principles of Universal Design

> [!info]+ Seven Core Principles
>
> 1. **Equitable Use**
>     - Design is useful for people with diverse abilities
> 2. **Flexibility in Use**
>     - Accommodates a wide range of preferences and abilities
> 3. **Simple and Intuitive Use**
>     - Easy to understand regardless of experience
> 4. **Perceptible Information**
>     - Communicates necessary information effectively
> 5. **Tolerance for Error**
>     - Minimizes hazards and adverse consequences
> 6. **Low Physical Effort**
>     - Can be used efficiently with minimal fatigue
> 7. **Size and Space for Approach and Use**
>     - Appropriate size/space for access and manipulation

## Interface Comparison: BASH Vs Windows File Explorer

> [!example]+ BASH (Text Interface)
>
> - **Strengths**
>     - Low physical effort (keyboard-only)
>     - Flexible (scriptable, customizable)
>     - Efficient for power users
> - **Limitations**
>     - Less intuitive for beginners
>     - Higher learning curve
>     - Limited perceptible information

> [!example]+ Windows File Explorer (GUI)
>
> - **Strengths**
>     - More intuitive visual interface
>     - Better perceptible information
>     - Lower learning curve
>     - Higher error tolerance
> - **Limitations**
>     - Requires more physical effort (mouse movement)
>     - Less flexible for automation

> [!obs]+ Universal Design Analysis
>
> - BASH better supports: ==Flexibility, Low Physical Effort==
> - File Explorer better supports: ==Simple/Intuitive Use, Perceptible Information, Tolerance for Error==
> - Both can support: ==Equitable Use== (with proper implementation)
> - Size/Space principle less relevant for software interfaces

# Clean Architecture and Accessibility

> [!important]+ Key Concept
> Clean Architecture facilitates accessibility updates because accessibility features are primarily located in the ==outer layer==

# Legal Requirements (AODA)

![](https://i.imgur.com/vm8WH6b.png)

> [!info]+ Implementation Guidelines
>
> - Based on W3C Web Accessibility Initiative standards
> - Organizations typically:
>     - Hire accessibility consultants
>     - Employ beta testers with disabilities
>     - Follow WCAG guidelines

[[Clean Architecture]]

# Accessibility Testing and Design

## Accessibility Checkers

- Automated systems that test program/file accessibility
- Common implementations:
    - App store mandatory checks
    - Microsoft PowerPoint checker
    - Other automated testing tools

# Summary

> [!important]+ Essential Points
>
> - Design must accommodate users with disabilities (==15%== of users - WHO)
> - Design approach varies based on disability model used
> - Both specialized and universal solutions have their place

## Disability Models Comparison

> [!def]+ Medical Model vs Social Model
>
> - **Medical**: ==Physical/mental impairment== causes a personal or social limitation
> - **Social**: ==Human world/environment== causes a personal or social limitation
