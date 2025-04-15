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
Date created: Mon., Jan. 20, 2025, 4:39:38 pm
Date modified: Sun., Apr. 13, 2025, 3:19:11 pm
---

# Decoders

> [!def]+ Decoder
> - **Decoder** *translates* from the output of one circuit to the input of another

> [!idea] Think of them as providing a ==mapping== between two different encodings.

- Decoders do not have one single implementation like muxes

> [!example]+ Binary signal splitter (one hot decoder)
> ![|214](https://i.imgur.com/OkFBcro.png)
> - Activates one of four output lines based on a two-digit binary number
>     - If inputs are $00 \implies A$ is turned on
>     - If inputs are $01 \implies B$ is turned on
>     - If inputs are $10 \implies C$ is turned on
>     - If inputs are $11 \implies D$ is turned on

^88700d

- Same information on the left-hand side and right-hand side in another format

## 7-Segment Decoder

- Common and useful decoder application

> [!info]+ 7-segment decoder
> - Translates from a binary number to the seven segments of a digital display
>     - i.e., You have a four-digit binary number
>     - Want to express that in a different way e.g., visual way
> - Each output segment has a particular logic that defines it

![|406](https://i.imgur.com/WOEn73H.png)

- To display any digit:
    - Send signal to each of the segments you want to light up
    - e.g., Number 8 requires sending a high signal to every segment
- These segments are **active-high**
    - Setting it to high turns it on
- Start making decoder by looking at individual segments
    - Case-by-case

> [!example]+ If we want to display digits from 0 to 9, inclusive, what numbers should make segment 0 input high?
> - Activate segment 0 for values: $0, 2, 3, 5, 6, 7, 8, 9$
> - In binary: $0000, 0010, 0011, 0101, 0110, 0111, 1000, 1001$
>
> ![|146](https://i.imgur.com/NDA4MGg.png)
>
> ![|361](https://i.imgur.com/qp9rudz.png)
>
> $$
> \texttt{HEX0} = \overline{X_{3}} \cdot X_{2} \cdot X_{0} + X_{3} \cdot \overline{X_{2}} \cdot \overline{X_{1}} + \overline{X_{3}} \cdot \overline{X_{2}} \cdot \overline{X_{0}} + \overline{X_{3}} \cdot X_{1}
> $$

- To display the digits 0-9 inclusive:
    - We only care about 10 rows vs. all 16 for 4 inputs
    - ? What if you get an input from $1010$ to $1111$?
        - Make the outputs whatever you want

## Don’t Care Values

> [!def]+ Don’t care values
> - Input values that will never happen or are not meaningful in a given design
> - Output values do not have to be defined
> - Recorded as `X` in truth-tables and K-maps

- In K-maps, think of **don’t care** values as either 0 or 1
    - Depending on what helps ==simplify== circuit
- Do not have to replace all `X` values with either 0 or 1
    - Include each `X` in groupings as needed

> [!example]+ HEX0 K-map
> ![|318](https://i.imgur.com/12csyHq.png)
>
> $$
> \texttt{HEX0} = X_{1} + X_{2} \cdot X_{0} + X_{3} + \overline{X_{2}} \cdot \overline{X_{0}}
> $$
>
> - Same number of terms, but
> - Fewer inputs → less gates

> [!example] Segment 1 (HEX1)
>
> ![|147](https://i.imgur.com/bA41Qcq.png) ![|370](https://i.imgur.com/0ok95EJ.png)
>$$
> \texttt{HEX1} = \overline{X_{1}} \cdot \overline{X_{0}} + \overline{X_{1}} \cdot \overline{X_{0}} + \overline{X_{2}}
> $$

> [!example]+ Segment 2 (HEX2)
>
> ![|245](https://i.imgur.com/7CCP5G8.png) ![|382](https://i.imgur.com/Rjwjd2Q.png)
>
> $$
> \texttt{HEX2} = X_{2} + \overline{X_{1}} + X_{0}
> $$

## The Final 7-Segment Decoder

![|348](https://i.imgur.com/Yw7QuOs.png)

- Decoders all look the same, except for ==inputs== and ==outputs==
- Implementation of decoders differ from decoder to decoder
    - Unlike other devices
