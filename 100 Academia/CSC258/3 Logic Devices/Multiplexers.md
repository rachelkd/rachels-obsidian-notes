---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
aliases: []
Week: 3
Module:
  - "[[3 - Logic Devices]]"
Lecture:
  - "7"
Chapter: 
Slides/Notes:
  - "[[devices_slides.pdf]]"
Date: 2025-01-20
Date created: Mon., Jan. 20, 2025, 3:48:53 pm
Date modified: Fri., Jan. 24, 2025, 11:53:49 pm
---

# Multiplexers

## Logic Devices

- Certain structures are common to many circuits, and
    - Have block elements of their own
    - e.g., **Multiplexers** (mux)
- Behaviour of 2-to-1 mux:
    - Output is $X$ if $S$ is 0, and $Y$ if $S$ is 1

> [!idea]+ Multiplexer Analogy
> - Picture two train tracks that are coming together
>     - Merging onto a third
> - When they merge together, only one of the two input tracks can go to the output track
> - ? How do we determine which one gets to go through?
>     - **Switch** has to determine which of the two gets to go to the output track

- This is what a mux does
    - Instead of trains, we have ==electrical signals==

![](https://i.imgur.com/TOWsiLQ.png)

- Slash symbol
    - So far, we have looked at single-bit inputs
        - Single bit for $X$ and $Y$
    - Typically, muxes are not handling one-bit values
    - Can use muxes to handle multi-bit values
        - e.g., Numbers
        - Say that the output $M$ is a $n$-bit number
        - $X$ and $Y$ could be two input numbers
        - Which of the two are going to drive the end bits of our output $M$?
    - We just do not want to draw $n$ lines if $n$ is large, so this is just a shorthand

> [!note] If $X, Y, M$ are multi-bit values, select $S$ is still a one-bit value
> - Purpose of $S$ is to say which of the two inputs is going to the output
> - Input zero, or input one?

## Multiplexer Design

![](https://i.imgur.com/G1dHcVw.png)

![](https://i.imgur.com/Rp80Np7.png)

- Right diagram shows implementation using NAND gates

## Multiplexer Uses

- Useful whenever you need to ==select from multiple input values==
    - e.g., Surveillance, video monitors, digital cable boxes, routers

## Demultiplexers

- Related to decoders: **demultiplexers**
    - Does multiplexer operation in reverse
    - e.g., Modems receiving Internet data

> [!idea]+ Analogy: Wireless Router
> - It has managed communication with four people’s wireless radios
> - When it gets the request back, it needs to decide which of the four receives the data
> - Select bit says who has been the one sending the data
>     - So that is the person it sends it back out to

![](https://i.imgur.com/AyQT22I.png)


![](https://upload.wikimedia.org/wikipedia/commons/e/e0/Telephony_multiplexer_system.gif)

- Select bits determine which of the conversations is the one getting “serviced”
    - i.e., Who gets to send requests to a single output line
    - Who gets to receive a request from the same output line
