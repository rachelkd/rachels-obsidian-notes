---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC258]]"
Module:
  - "[[1 - Transistors]]"
Week: 1
Lecture:
  - "2"
Chapter: 
Slides/Notes:
  - "[[transistors_f2024_slides.pdf]]"
Date: 2025-01-08
aliases: []
Date created: Wed., Jan. 8, 2025, 3:32:12 pm
Date modified: Sat., Feb. 22, 2025, 8:45:46 pm
---

# Electricity Basics

- Electricity is the ==flow of charged particles== through a material
    - Usually electrons
- Charged particles come from atoms
    - Made up of *protons*, *neutrons*, *electrons*
    - Positive charge, no charge, negative charge, respectively
- Electricity stems from ==electron movement==

## Voltage and Current

- Electrical particles (e.g., electrons) want to ==flow from regions of *high electrical potential*== (many electrons) ==to regions of *low electrical potential*== (not as many electrons)
    - Similar to gravitational potential
- Potential is referred to as **voltage**
- **Current**
    - Rate of electron flow
- **Resistance** $R$
    - Relationship between voltage $V$ and current $I$
    - Often determined by material

> [!thm]+ Resistance
> $$
> R = \frac{V}{I}
> $$

> [!note]+ Even though current is caused by electrons flowing through a material, the convention is to measure current as the *movement of positive charges*.
> - Protons do not actually move
> - When electrons move from Point A to point B:
>     - B becomes more negative, and
>     - A becomes more positive
> - Scientists historically viewed current in terms of this creation of positive charge in a material

## Sources of Electricity

- Two common sources of electricity:
    - **Batteries**
        - Have a concentration of particles stored inside them
        - Will run out eventually
    - Electrical outlets
        - Constantly being supplied with electric particles that never run out
            - e.g., Waterfalls

## Path of Electricity

- Current always flows toward the zero voltage point of a circuit
    - Referred to as **ground**
- Electricity always takes the path of ==least resistance== from source to ground
- All electricity wants to make its way to the ground

> [!important]+ Remember!
> - Even though electrical current is flow of electrons through a medium,
>     - Direction is typically expressed as the *movement of positive charges*
>
> ![](https://i.imgur.com/3V4vvVb.png)

## Using Electricity

Knowing that electric particles want to travel from areas of high concentration to areas of low concentration,

- We can use this to drive **circuits**
    - A circular path
- Each of these circuits has:
    - A **source** of electrical particles
    - Some **path** between this source and the ground
    - Some **resistance** along this path that ==dissipates these electrons==

![](https://i.imgur.com/9xO8I0C.png)

- Electrons flow from negative side to positive
- Current flows from positive to negative
- Put stuff in-between, e.g., lightbulb
    - Electricity flows through lightbulb
    - Loses some of the voltage potential
        - → Transfers into light energy
        - Uses the flow of electrons to do some kind of work

## Resistance is Futile

- In the water analogy:
    - **Resistance** would be the measure of how restrictive the pipe is that connects the reservoir to the ground
    - Low resistance $\leftrightarrow$ Wide, smooth pipe
    - High resistance $\leftrightarrow$ Narrow, twisty pipe

> [!def]+ Electrical resistance
> - Indicates how well a material allows electricity to flow through it
> - Measured in ohms ($\Omega$)
>     - More ohms, more resistance

- **Insulators**
    - High resistance
    - Do not conduct electricity at all, or only under special circumstances
- **Conductors**
    - Low resistance
    - Conduct electricity well
    - Generally used for wires
- Are largely determined by the position of the element on the periodic table
- **Semiconductors**
    - Somewhere in-between conductors and insulators
