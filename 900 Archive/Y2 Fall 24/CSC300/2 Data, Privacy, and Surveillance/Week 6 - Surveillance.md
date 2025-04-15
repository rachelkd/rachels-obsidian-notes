---
dg-publish: true
tags: [cs, ethics, lecture, note, university]
Course Code:
  - "[[CSC300]]"
Week: 6
Module:
  - "[[2 - Data, Privacy, and Surveillance]]"
Date: 2024-10-08
Lecture:
  - "6"
Quercus Page: https://q.utoronto.ca/courses/354160/pages/lecture-6-surveillance?module_item_id=5960389
Slides:
  - "[[Lec6_Surveillance_Ahmed.pdf]]"
Date created: Sat., Oct. 12, 2024, 12:06:09 am
Date modified: Thu., Nov. 28, 2024, 4:14:09 pm
---

> [!tldr]- Where we are
>
> - Politics in data
>     - How **infrastructure inversion reveals the politics of collection, analysis, and applications of data**
> - Importance of privacy
>     - How privacy is defined by the contextual norms and boundaries

> [!goal]- Learning Objectives
>
> - What is **surveillance**?
> - How surveillance can become problematic
> - What is **dynamic normalization**?
> - What is **surveillance capitalism**?
> - How can we resist surveillance

---

# Surveillance

- Monitoring behaviour for ==information gathering==
    - Widely used today
    - People are capturing info everything
    - e.g., CCTV cameras (on roads to determine speeding, important places to keep eyes on law breakers)
    - e.g., Smartphones, smartwatches, other wearables are constantly taking data while you are using it
        - Location data, who you contact, etc.

<!-- break -->
- We convert data into **models**
- Model is used in different applications to give people different applications

> [!important] The core work that many computer scientists, data scientists, and AI experts today do is ==making a model out of data==

- Commercial companies will tell you that they are capturing data to give you better services

# Surveillance as Social Control

> [!abstract] Surveillance creates a social control

- When you are under surveillance, you act in a way that is different from how you would act freely
    - When you know that people are looking at you (i.e., under surveillance), you check what you are doing
    - e.g., Make sure you are not breaking any laws
- A powerful entity can make sure that you are following all the rules, regulations, and laws that are set by this powerful entity
    - e.g., Government: Put surveillance to make sure every citizen follows the laws set by the government
- ==Surveillance as social control== is the main mechanism for why surveillance took place

## Bentham’s Panopticon

- **Panopticon**
    - Type of ==institutional building== and a ==system of control==
    - Designed by English philosopher and social theorist ==Jeremy Bentham== in 18th century
    - Disciplinary concept brought to life in the form of a central observation tower placed within a circle of prison cells
        - From the tower, guard can see every cell and inmate
        - Inmates cannot see into the tower
        - Prisoners will never know whether or not they are being watched
- Concept of design:
    - Allow all prisoners of an institution to be *observed* by a single security guard
    - Without inmates being able to tell whether they are being watched

![|center|400](https://i.imgur.com/R6rRl12.png)

- Introduced as a model of prison reform
- Design helps prison authority make sure that all prisoners can always be watched by only one guard
- Prisoners also know that they are being watched
- $\implies$ Became a very famous model for controlling the prisoners
    - A lot of other institutions also borrowed model and put a surveillance on their employees
    - e.g., Metal industry

# Foucault’s Expansion

- ==Michel Foucault==
    - French philosopher, historian of ideas, writer, political activist, literary critic
- Expanded on the conception of the panopticon:
    - ==Building’s material capacity enables order and social control==
    - **Visible**:
        - $ The inmate will constantly have before his eyes the tall outline of the central tower from which he is spied upon
    - **Surveillance is unverifiable**:
        - $ The inmate must never know whether he is being looked at at any one moment; but he must be sure that he may always be so

> [!quote] “The Panopticon is a machine for dissociating ==the seeing/being seen dyad==: in the perpheric ring, one is totally seen, without ever seeing; in the central tower, one sees everything without ever being seen.”

![|center|300](https://i.imgur.com/QvvYuWQ.png)

> [!important]+ Foucault fundamentally is concerned about the idea of power and the relationship between power and knowledge.
>
> - **Central thesis:** Clear power imbalance between two parties if one person can see someone all the time, and the other person cannot see the first person
>     - May not also mean visually see, but getting information and knowledge

- ==Asymmetry of knowledge → Power imbalance between people==

<!-- break -->
- This panopticon has already been installed in society in different ways
    - Government, religion, family puts surveillance on us
    - We often don’t understand how much surveillance is on us
- Model has *extended* beyond prison structure
- Foucault proposes that we now live in a “**panoptic machine**”
    - Two perspectives are developed in analyzing the nature of this surveillance:
        - That of the “supervisor”
        - That of the “inmate”

## The Inmate

- Aware of the signs of surveillance
    - Tower, CCTV cameras, satellites, etc.
    - ==Sign of the presence of a supervisor rather than actual presence== is critical
- Uncertain whether they are being seen
    - → **Internalizes** the watching
    - → Conforms to explicit and implicit rules of the frameworks/institutions they live in
- Abstractly, power strives to be panoptic despite resistance
    - Actual panopticon may be static and material, but
    - When power functions through **panopticism**, it is *mobile*
        - An enclosure on the psyche rather than something spatial
        - Need to instil in people’s mind that they are always being observed
        - e.g., God is always looking at them → Cannot do something considered bad in their religion

## The Supervisor

- ==Enforces internalized rules==
- CCTV influences behaviour unknowingly
- Supervisors reinforce panopticon efficiency
    - Facebook moderation shapes behaviour
    - Reporting tools encourage compliance

<!-- break -->
- What does Foucault say?
    - Supervisor increases the capacity of individuals/states/institutions to know about social groups and populations
    - Does not simply maintain control, but
    - Also plays role of administrator/bureaucrat/scientist that connects general individuals to authority

## Surveillance as Social Control: Why?

- For governments:
    - Surveillance supposedly for prevention and investigation of crime
    - Also used as a tool of social control:
        - To adjust and influence how a population behaves
        - e.g., Find out who does not align with ideology
- Behaviour that is not in line with the ideas of the group that is conducting surveillance can be *targeted for intervention*
- Mere presence of surveillance devices/buildings can serve as a reminder/symbol for the act of surveillance leading people to ==*modify* their behaviours in accordance with implicit or explicit rules==

# Dynamic Normalization

- **Normalization**
    - Refers to social processes through which ideas and actions come to be seen as *normal*
    - Taken-for-granted or natural in everyday life
- **Dynamic normalization**
    - Happens when such normalization process changes according to the will of the ruling power
    - e.g., Slurs were allowed on TV and now, it is not appropriate
        - Using Teams to send an invite over calendar technology
        - Before phones, people did not expect an instant response from you → Technology made people have this expectation

> [!important] Dynamic normalization is connected to panopticon and behavioural control

- When something is normalized in society, society wants you to follow that custom
    - → Custom becomes your behaviour
    - → Cannot deviate from that behaviour
- → There has to be some mechanism where people will know what you are doing

![|400](https://i.imgur.com/dQl1F8o.png)

### Example

- University students are expected to have access to computers and internet
    - Most communication through email
    - Internet technology → People will have email, text, phone → Become part of the norm
- Street directions are no longer provided
    - When you ask someone for an address, they no longer also provide you with directions
    - Norm that you can use map technology to find the address

# Surveillance for Profit

- Companies are making money out of surveillance

## Surveillance Capitalism

- **Informational capitalism**
    - Contemporary political, economic, and cultural tendency to perceive personal information as a *basic resource* (just like energy), the most reliable element on which to build safety enhancement and efficiency strategies, and as a commodity, exchangeable on the “information market”
    - ==Value of information==
    - This is what social media companies are doing:
        - Gathering data → Selling data/information to companies
        - Get this information by ==imposing surveillance== on users
- **Surveillance capitalism**
    - Involves information ==collected by means of surveillance==, and
    - How this information about people can be used as a resource

![](https://i.imgur.com/y9J9H7n.png)

- Surveillance capitalism involves three parts:
    - **Data**
        - Produced by general public and people
    - **Extraction**
    - **Analysis**
- Collected from the presence of *ubiquitous computing* architectures
    - i.e., sensors and data collection in every aspect of life and use of technology in the course of it

### Data

- *Mechanisms* are put into place to ==collect large scale information== about people, for the benefit of power-holding organizations or entities
    - Mechanisms e.g., Surveys, sensors, usage log, etc.
    - Organizations e.g., government, corporations, universities, etc.
    - → Threatens individual privacy
    - → Intervenes personal behaviour

### Extraction

- Techniques are applied on raw data to *extract* information relevant to the profit of power-holding entities
    - One-way process
        - *Not* transactional
    - Data is made valuable to corporations (e.g., advertisers)

### Analysis

- Models are made to provide insights into people’s behaviours → Can be leveraged by power-holding entities
    - Models based on data aggregation sidelines individual priorities
    - Models are dominated by majorities

## Surveillance Capital

> [!def]- Capital (in surveillance capitalism)
>
> - Development of these data-driven models that attract *investment* is called **surveillance capital**

> [!def]- Surveillance capitalism
>
> - This dis-embedded and extractive variant of information capitalism is **surveillance capitalism**
> - The process of developing data-driven models to make money through information extraction

- Facebook sells users’ behavioural data to apparel industries
    - Your information → their money

## Concerns About Surveillance Capitalism

- Concerns are **privacy** and **trust**
    - Companies often taking private information (with or without consent)
        - Consent is technically there, but it is a long document (Terms of Conditions) and changes frequently
    - Have blind trust that they will not be sending our data to a third party
- Surveillance capitalists often violate our *[[Week 5 - Privacy Theory|privacy rights]]*
    - Users do not have much choice over what is maintained as secret or private
- **Trust-based relationships** between people and products
    - (In which professionals are trusted and held accountable by mutual dependencies)
    - Are eroded by the formal indifference to users:
        - Should I trust my phone? TV? Home assistant?
        - → Cannot trust anything in today’s world because so many tech companies are hungry for data

> [!info]+ Trust-based relationships are needed, but there is…
>
> - **Asymmetry in power** between corporations and people using their services
>     - Can one abandon Google even if one wants to?
>         - You do not hold the power to ignore them
> - **Asymmetries in knowledge**
>     - We do not know what data these companies are collecting, but they know a lot about us
>     - Sustained by the asymmetry in power
>     - i.e., You do not know the extent to which Google has information about you and how it is being used vs. the array of information Google has on you

- Another concern: **experimental objects**
    - Powerful entities collect data *and* ==conduct experiments on us== to see changes in our behaviour
    - Such experiments make more “profitable” models
- e.g., Facebook conducted a large-scale emotion contagion study in 2013-14
    - Half of Facebook users saw only one kind of information (happy)
    - Others saw the opposite (sad)
    - Experimented which kind of information is more shared
- Experiments are developed to modify behaviour in these domains
    - → See what is more profitable without necessitating the consent of the user population
- →==Reality is commodified and monetized==

## Applications Today: Social Media

- Profit from engagement and clicks
- No incentive for information integrity
- Fake news appeals to advertisers
- Data used for political purposes
    - Cambridge Analytica influenced 2016 election
    - Political parties are also spending money to make business out of information

## Government and Corporation

- The idea of the **revolving door**
    - A lot of tech executives end up in the US government and vice versa
    - An exchange between government positions and those in corporations

![|center|300](https://i.imgur.com/QBGcuTg.png)

# Capital and Control

- Surveillance capitalism collects behavioural data
    - Not only to promote/sell products according to people’s behaviour/taste
    - Also *changes* and *controls* people’s behaviours/taste for their profit
    - e.g., Netflix:
        - Presents movies that we like
        - Makes us like movies to generate profit

![](https://i.imgur.com/WVQQ8fP.png)

- Suppose there is a new drink.
    - Company that is selling this drink wants people to drink this
    - → Go to social media company → Collect data, do analysis, let drink company know whether people like this product or not
- Suppose they find that people do not like this drink
    - → Want to make this normalize in society so that people start drinking
    - → Media campaign, start showing advertisements to target users
        - “All your peers are drinking this thing; if you’re not drinking this, you’re not cool”
    - → Social media platforms target users with these ads: e.g., Are you a person in your 20s with friends and partying? Discounts, available nearby
- → Normalized in society

> [!important]- Social media surveillance and dynamic normalization work together to help large corporations make money out of the public

- Extract data from us → Do the analysis → Get the information which they sell to companies
- Information is important for these companies (surveillance capital)
- Also run ads on social media to change people’s minds

# Broader Concerns

- Also applies to governments and other powerful entities
- Loss of individuality: controlled humanity
    - e.g., Border control: Forced to do this informational labour; take off shoes, full body scan to give this information about yourself to the government

# Resisting Surveillance

## Individual

- Given that surveillance itself is **contextual**, **relational**, and **dependent on power dynamics**, ==resistance will also take on these characteristics==
- Hard to resist on a personal level
    - However, education on digital media and digital data protection needs should be more common
    - i.e., ==Technological literacy==
        - Understanding technology, its roles, and the importance of rights such as privacy

## Collective

- Can start building a resistance at the community level
- Collective resistance can *petition* for ==transparency== from agents of surveillance
    - Require governments and corporations to disclose the information collected and the ways in which it is used
        - Kind of like a protest
- Petition agents to ==give users more control== over personal data
- Petition big tech to be ==regulated== as public utilities
    - Keep sensitive data away from market pressures
    - e.g., Health data
- ==Use technology as a means, not an end==
    - Build communities or “smart cities” in an empowered way
- Introduce supportive groups, meaningful community participation, and mutual resource mobilization and aid
    - e.g., Baugruppen model in Germany
- Technology can then be developed to streamline processes within this developmental model

> [!important] We cannot resist all the surveillance, but it is important to understand how we are under different kinds of surveillance

- What can we do? What is the right thing to do? How can we minimize the data that is being extracted from us? What are the ways we can regulate surveillance
