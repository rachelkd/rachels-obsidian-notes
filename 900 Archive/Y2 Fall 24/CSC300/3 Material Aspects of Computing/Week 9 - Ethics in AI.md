---
dg-publish: false
tags: [ethics, lecture, note, university]
Course Code:
  - "[[CSC300]]"
Week: 9
Module:
  - "[[3 - Material Aspects of Computing]]"
Lecture:
  - "9"
Chapter:
Slides/Notes:
Date: 2024-11-05
Date created: Tue., Nov. 12, 2024, 1:00:45 am
Date modified: Wed., Nov. 13, 2024, 11:48:59 pm
---

# So Far…

- [[Week 2 - Ethical Reasoning and its Relevance to Computer and Society|Theories of Ethics]]
- [[2 - Data, Privacy, and Surveillance|Political frameworks]] for analyzing technological systems
- [[Week 4 - Politics of Data|Knowledge policies]] in data classification
- Issues with data collection
    - [[Week 5 - Privacy Theory|Privacy breaches]]
    - [[Week 6 - Surveillance|Surveillance]]
- Environmental impacts of computing technologies
    - [[Week 7 - Computing and the Environment|Material extraction]]
    - Power consumption

> [!important]+ AI is not separate from any other computing technology
>
> - Is an example of computer technology
> - What we have learned already applies to AI as well
>     - e.g., Political, environmental issues

> [!goal]+ Learning Objectives
>
> - **Labour** issues in AI
>     - **Data annotation**
>     - AI replacing workers
> - **Data colonialism**
>     - Data **appropriation**
>     - Data **relations**
> - AI regulation
>     - Policies
>     - Applying existing laws

## Ethical Considerations in AI

- **Labour issues involved in AI**
    - **Data annotation**
        - AI tech we use today relies on available huge amounts of data and annotated data
        - Ethical issues:
            - Who labels what data and how?
            - How much are they paid?
            - How about their workplace safety
    - **Future of work**
        - Is AI going to take our jobs?
        - Which jobs? Why?
        - What kind of “re-skilling” is needed?
- **Data colonialism**
    - Tied to how AI is supposedly coupled with data
    - **Data appropriation**
        - Ethical issues:
            - Is data a resource? (Data access)
            - If a data source is available, can anyone use it? (Data extraction)
            - Who is creating the data and who is benefiting from it?
    - **Data relations**
        - How do AI technologies control data production?
        - How is the data used to train AI managing our relationships (to each other and the world)?
- **AI Governance**
    - The way we regulate ethics in today’s world is closely tied with governance
    - **AI policy**
        - What regulation is needed to prepare for AI?
        - What kind of enforcing mechanism is needed?
    - **Audits and impact assessments**
        - How do we evaluate AI tech?
        - How should existing regulations be applied?

---

# Overview of a Typical (Data-Drive) AI System Pipeline

![](https://i.imgur.com/tdTzmQu.png)

- We focus on **data curation** in this lecture

# Recap of the Course So Far

## Politics of Data Classification

See [[Week 3 - The Politics of Technology]], [[Week 4 - Politics of Data]]

- Data is
    - ==Never objective and neutral== (specifically, data **categorization**)
        - e.g., It may be inspired by Botanical Taxonomic Approach
    - A ==tool to understand the world==
    - Always ==dependent on human judgement==
- **Data collection, organization, and use** are always based on ==historical and political judgements==
    - e.g., The poor, unemployed, Black people. unmarried mother, illegitimate child, etc. are historically delineated as “deviant categories” in order the design ‘solutions’ (interventions) with which to *control* these groups
- **Data categorization**
    - Who is included and who is left out?
    - Which categories are created and which are left out?
    - Whose interests guide category formation?
    - **Recognition of categories** can be a ==slow, politicized process==
        - e.g., AIDS status as a disease achieved through activism
    - Some categories are ==deliberately misrepresented/misused== based on cultural conetxts
        - e.g., Differing statistics of heart attack in USA and Japan

> [!important]+ Connection to AI
>
> - A lot of what AI does is classification
> - e.g., iPhone might auto-tag photos; separate you, friends, pets into different objects

## Surveillance

- See [[Week 6 - Surveillance]]
    - **Bentham’s Panopticon**
    - **Foucault’s Expansion**
    - **Surveillance as social control**
    - Dynamic normalization (through the *panopticon* and *behavioural control*) and surveillance capitalism (where data is extracted and analyzed for surveillance capitalism) have a bi-directional relationship!

> [!important]+ Connection to AI
>
> - A lot of technology is now being powered by AI
> - Surveillance and AI are also tightly coupled
>     - Process of data extraction and analysis with AI becomes more scalable
>     - The presence of a camera that can now recognize your face is much more powerful

- **Government and corporation** and surveillance
    - Idea of the **revolving door**
        - A lot of tech executives end up in the US government and vice versa
        - An exchange between government positions and those in corporations

## Computing and Environmental Impacts

- **Technology life cycle**
    - Raw material extraction → Use of technology → Repair → Disposal
- **Raw material extraction**
    - e.g., Cobalt
    - Data centers require this mineral much more
    - Also increasing ==emissions==
    - Increasing use of AI is increasing the requirement for data centers and the resources that data centers take
- **E-waste**
    - Tied to ==social and environmental justice==
        - ==Toxic colonialism==
    - ==Environmental toxicology== and ==occupational health==
    - ==Material energy flows in electronic production== and ==ways to manage the disposal of post-consumption electronics==

# AI and Labour

## Data Annotation

> [!def]+ Data annotation
>
> - The process of labeling data for AI applications

- Involves adding categories, labels, and other contextual elements to raw data
    - e.g., text, audio, image, video
    - → So machines can read and act upon the information
- Data annotation can be done by humans, machines, or a combination of both
    - Depending on type, quality, and quantity of data

![](https://i.imgur.com/3OuwLWx.png)

- Examples of data annotation:
    - Auto-transcription, auto-captioning, translation, semantic classification, content moderation

### Who Annotates Data?

- Freelance/contract labelers
- Mostly young, low-income, low-educated
- Primarily from developing countries
- Harms:
    - Precarious, exploitative conditions
    - Low pay, long hours, harmful content

#### Wages of Data Annotators

- As of 2023:
    - Average hourly pay for data annotator reported by an annotation company was $25.23
    - Lower than median hourly wage for all occupations in US ($28.18 in 2020)
- Majority of data annotators in US and Canada earn below living wage
- Data annotators in other countries (e.g., India, China, Kenya, Philippines) earn less than US annotators
    - Ranges from $2 to $10 an hour

### Data Annotation and the Voices of the Workers

- Workers have no or limited voice over how they annotate data
- Not treated as respected human beings whose opinions are important
- What they are doing is actually *skilled labour*
    - Have to do their work fast

### Data, Classification, and Power

- ==Annotation is **culturally and historically specific**==
    - Annotation classifies data
    - Classifications are *subjective* choices
    - Hidden ethical/political implications
    - Constructs and reflects social reality
- Example of how politics of data gets embedded into politics of AI
- ==Annotation is *always subjective*==
    - Reflects labelers’ worldview
    - Manual, unaware, or third-party labeling
    - Includes Mechanical Turk biases (platform/software used to annotate)
    - If annotator is not aware of such biases, then it is embedded
- ==Annotation process is *power-laden*==
    - Relation between annotators, data, and corporate structures
        - Employers who employ these annotators shape how the annotation is done
        - e.g., Time constraints for annotations
            - Slow → Get fired → Cannot give all the attention it needs

### What Constitutes Power in Data Annotation

- **Standardization**
    - Annotation companies ==prioritize requirements== and ==expectations of commissioning clients==
    - Annotation guidelines generally tailored to meet ==requirements of the product== that would be trained on the annotated datasets, its desired ==outcome(s)==, and its ==revenue== plans
    - Quality = doing what the client expects
        - e.g., Creating more categories might be equated to high quality work
    - Non-objective nature comes from *power*; this is a question of power
- **Layering**
    - Annotation process has many layers of individuals and departments involved
    - Many roles and departments participate in annotation assignments
    - Annotators are in the ==lower layer== of the hierarchical structure where the actual labeling of data is carried out
        - Client company → Outsources to data annotators
- **Naturalization**
    - Annotators and managers assume that clients know exactly how data was supposed to be labeled
        - They just follow instructions; ==do not see entire dataset==
        - e.g., Might hear 30s of an audio clip to categorize
        - → Might make decisions based on quality assumptions
    - Annotators usually do not perceive the meaning of labels imposed in the top-down approach of data labeling
    - → Annotators cannot make any judgements about data
        - e.g., If they bring up biases to client companies, they might not get paid or will get fired
        - Annotators do not have *ownership* of the data
- **Profit Orientation**
    - All of this is driven by profit
    - Annotation companies are trying to reduce cost to remain competitive
    - → Seek to optimize the speed and homogeneity (i.e., annotated the same way) of annotations
    - Commissioning clients and annotation companies are primarily concerned with product and revenue plans vs. wellbeing of workers and workers’ judgements

## Workers’ Safety Issues

### Case Study: Labeling Inappropriate Content on Social Media

- Labelling hateful content can ==affect the mental health of data annotators==
    - e.g., **content moderation** in social media
- Content moderators have suffered from psychological distress, trauma, burnout
- Often do not have adequate support or protection
- A lot of these people are precarious and vulnerable
    - Have to expose themselves to violent and hateful content to make money
- Avoiding the public from suffering from violent content has made content moderators suffer instead

## AI and Employment

- Many current applications of AI are ==designed to replace human workers==
    - e.g., When we think of “automation,” we think of not having to deal with humans
- Generative AI is ==taking white-collar jobs== and wages in the online freelancing world
    - Rather than just blue-collar jobs that one might assume
    - Not just factory workers, but people who do intellectual work (e.g., office work)
- While we are not sure how much of generative AI is useful, we do know that many people/companies want to ==adopt it everywhere== as fast as they can

> [!warning]+ Ethical concern:
>
> - We might be adopting technology without understanding its impact properly

### Concerns

- AI and automation could affect or eliminate ¼ of US jobs
    - Especially in sectors such as manufacturing, retail, transportation, hospitality
        - Where tasks can be easily automated
- AI and automation could also ==widen the income and wealth inequality== in the US and other countries
    - By creating a new wave of billionaire tech barons at the same time that pushes many workers out of better paid jobs

> [!important] The excitement to adopt AI everywhere is usually driven by people in the management who ==want to reduce labour costs==

- AI might not be useful, but it *will* be adopted to reduce jobs

### Nuance

- Biggest performance gains came among the less highly skilled workers

![](https://i.imgur.com/0eaXjLQ.png)

- The more multifaceted your job is → Less risk of it being automated out of existence
    - e.g., Human creativity less likely to be automated v.s. mundane tasks
- **Gig-worker model** of performing one fairly narrow task for multiple clients (even if you perform it exceptionally well) is exposed

### Counterpoints

- ==AI will *not* replace human capabilities and values==
- New technologies will *complement* rather than substitute human skills and attributes that are essential for many tasks, industries, markets
- e.g., AI will not replace human capabilities and values such as empathy, creativity, and ethics

## AI and Reskilling

To be ethical and responsible, what we need to do is **reskilling**

> [!def]+ Reskilling
>
> - The process of learning new skills
>     - So you can do a different job, or of training people to do a different job

- Can help workers adapt to new roles and opportunities
- Can help organizations meet their staffing needs and goals
- Sort of like creating a platform so that people can transition more smoothly
    - e.g., Instead of being fired because AI is replacing your job, you will instead be placed in a training program so you do not have to deal with economic fragility

> [!important] Reskilling is needed to survive AI

- **Digital literacy**
    - The ability to use, create, and evaluate digital technologies and media
    - Important for workers to adapt to changing nature of work and communication in the digital age
- **Data literacy**
    - Ability to understand, analyze, and communicate data effectively
    - Essential for workers to leverage power of AI and make ==informed== decisions based on data
    - Can be enhanced by learning data science, statistics, visualization, and storytelling skills
    - Somebody needs to interpret (maybe even modify) AI outputs
        - Cannot do that without understanding data
- **Critical thinking**
    - Ability to reason, analyze, and evaluate information and arguments logically and objectively
    - Crucial for workers to ==cope with the complexity and uncertainty of AI and its impacts==
        - When the future is uncertain, critical thinking becomes even more important
    - Can be developed by learning problem-solving, decision-making, and creativity skills
    - AI cannot do this right now
        - It can ==recreate patterns, but it cannot *build* new patterns==
- **Emotional intelligence**
    - Ability to recognize, understand, and manage one’s own and others’ emotions
    - Vital for workers to ==collaborate and communicate effectively with diverse people and cultures==
    - Vital to deal with stress and challenges of AI and automation
        - As more things become automated, emotions and embodied learning becomes more valuable
    - Can be cultivated by learning empathy, self-awareness, and self-regulation skills
- **Lifelong learning**
    - Ability and willingness to learn new skills and knowledge continuously and independently
    - Necessary for workers to keep up with rapid pace of technological change and innovation, and to seize new opportunities and roles created by AI
        - We don’t know what will be adopted, what something will be used with
        - → Must be flexible and adapt to new technology and also be critical and not use new technology if it is not good
    - Can be fostered by learning learning strategies, motivation, and curiosity skills

> [!important]+ While all of these are important at the individual level, it cannot actually solve the problem because a lot of the problem is **institutional**
>
> - Need to do reskilling at a societal and structural level
>     - e.g., Add curriculum at universities,
>     - Require companies to do reskilling before adopting AI (and consequentially, firing their workers),
>     - Introduce government programs to do reskilling needed to survive AI
> - Question about AI and ethics and responsible AI is ==not just about using/not using AI==, but also about ==building structures==
>     - At the national, societal, community, provincial, institutional, individual level
>     - ==Adapt== to its use and ==evaluate== its use and outcomes

# Data Colonialism

- Extracting and exploiting data without consent

### Examples of Data Colonialism

> [!example]+ Arizona State University Research
>
> - ASU used Havasupai DNA without consent
> - Researched beyond original agreement
> - Violated Havasupai rights/interests
> - Indigenous tribe sued and won in 2010

> [!example]+ Facebook and Cambridge Analytica
>
> - Facebook allowed Cambridge Analytica access
> - Exploited user data ==without consent==
> - Used for political manipulation
> - Exposed transparency/accountability issues
> - Sparked global outcry in 2018

> [!example]+ Google: Toronto Smart City
>
> - Data collection without public consent
> - Raised privacy/security concerns
> - Public opposition (and COVID) led to cancellation in 2020

## Definition

> [!def]+ Data colonialism
>
> - Governments, organizations, and corporations claim ownership and privatize data to extract and exploit

- Concept:
    - Data is seen as ==valuable for profit and power==
    - Producers lack control
        - Cannot consent meaningfully
        - Most of the times, unaware that data is captured
- Impacts:
    - Can harm rights, interests, and well-being
    - Particularly for marginalized groups
        - Partly because a lot of these experiments first happened in poorer areas
        - Marginalized groups treated as guinea pigs
        - Often lack ability to opt-out (e.g., financial and time costs)
        - Lack power most of the time anyways

## Data Appropriation

> [!def]+ Data Appropriation
>
> - Data is extracted from human life and turned into a resource for capitalism

- **Data appropriation** involves the claim that ==data is “just there” and available== for anyone to take
    - Without regard for the ==rights, interests, and well-being of data producers==
- Closely tied with **data colonialism** and **data capitalism**
- Data appropriation involves the **commodification and monetization of data**
    - As well as the creation of **new markets and industries** based on data
    - e.g., Social media platforms sell data about their users; application is free, but created advertising market as a result
        - We get free services, but these companies are making millions off data
- Most AI models were trained on freely-available information on the web and copyrighted data without artists’ consent
    - Generative AI companies (e.g., OpenAI) assume that data is just there and available for them to monetize
    - People are pushing back

## Data Relations

- Data is *not* just there to be taken
- Data is used to ==shape and control human life and society==
    - Tool to understand the world ([[Week 4 - Politics of Data]])
- Data relations involve the **normalization** and **systematization** of data collection
    - As well as the widespread adoption of data-driven knowledge and narratives
    - e.g., We have used data to understand our heartbeats, steps you take daily, etc. → Measure health
- Data relations also involves the ==creation and enforcement of new forms of power== and authority based on data
    - e.g., If you wear an Apple Watch, Apple has more control over you
    - → Usually ==leads to exclusion of those who are not represented or valued by data==
        - e.g., If everybody has fitness watches and the watch directly reports to doctors, then the person who cannot afford a watch will get excluded
    - This is a form of power; an ethical concern
- Data relations can also be positive
    - Examples of better data relations: Using data to highlight erased narratives, empowering people, and building communities
        - e.g., “Society has always been highly racist and sexist, and here is the data that shows that”

> [!important]+ Data should ==not be thought of a static object you can capture==
>
> - Data itself does not mean anything
> - Data is only valuable when it is used to make decisions
>     - e.g., Targeted advertising
> - These decisions shape our understanding of the world and our relationship with each other, society, politics, etc.

## To Prevent Data Colonization

- Respect rights of data producers
    - e.g., Take in account of groups that are excluded
- Obtain consent, participation, and benefit-sharing
    - e.g., Have a way to opt-out easily
- Develop governance frameworks with values in mind
- Ensure protection, privacy, and ownership
- Promote data literacy and empowerment
    - You should not be an expert in the industry to understand the data
- Challenge data colonialism and advocate for justice

# AI Regulation

- A lot we can do at individual level, but to really control AI processes, we need **AI regulation**
    - AI being deployed by large corporations with money and power

> [!info] One way to control is with ==state and regulation==

- Establishing and enforcing rules and standards for the ==collection, storage, processing, use, and sharing of data and algorithms== by governments, organizations, or individuals
    - Enforce these rules for each stage of the [[#Overview of a Typical (Data-Drive) AI System Pipeline|AI System Pipeline]]
- Main purpose should be to ==protect the rights, interests, and well-being of citizens==
- AI regulation should also seek to ==balance the benefits and risks of data for society, economy, and innovation==
    - e.g., Trying to balance economic growth — which AI is supposedly going to lead — and well-being of society
        - Maybe sometimes the biases against marginalized groups, who already have less power
- AI regulation is the ==lowest bar==
    - ==Just because something is legal, it does not mean that it is ethical==
    - When you are making decisions for governance (in a democratic society) → Need to come to a consensus
        - Will probably only regulate the things that have low opposition
- → A proper, ethical, and responsible way to do AI (and to do it in a good way) would require something ==beyond those laws==

> [!example]+ Examples of AI Regulation already
>
> - **AI Bill of Rights**
>     - Introduced by Biden administration
>     - Guidelines; not enforced, but a standard
> - **Artificial Intelligence Act**
>     - by the EU
> - **General Data Protection Regulation (GDPR)**
>     - Law in EU

- When we talk about regulation, there are:
    - ==Laws==, but also
    - ==Guidelines and principles==
        - which have developed rapidly

## Globally

- Legislative bodies in 127 countries passed AI-related laws in 2022

![|center](https://i.imgur.com/2oEsR8T.png)

- Laws often lag behind society
    - Don’t pass laws until you *see* something and its impacts on society
- While laws specific to AI are being passed, as AI is being integrated into other decision-making systems in other parts of our lives, there are other laws already existing, which should be governing AI
    - Just because AI is used for banking does not mean existing financial laws do not apply
- Recent deployments of AI actually do violate many of the existing laws
    - People are trying to build frameworks to impose those laws into new algorithms
    - e.g., AI violating housing laws, equal employment laws, etc.

## Data Principles for AI Regulations

- A lot of data principles are being drawn from **FAIR Principles**

### <mark style="background: #D2B3FFA6;">FAIR Principles</mark>

- Published in 2016 in Scientific Data
- FAIR Guiding Principles for scientific data management and stewardship
- Make your data:
    - **Findable**
    - **Accessible**
    - **Interoperable**
    - **Reusable**

![](https://i.imgur.com/tXa6OPM.png)

#### <mark style="background: #D2B3FFA6;">Findable</mark>

- (Meta)data are assigned a globally unique and persistent identifier
- Data are described with rich metadata
- Metadata clearly and explicitly include the identifier of the data they describe
- (Meta)data are registered or indexed in a searchable resource

#### <mark style="background: #D2B3FFA6;">Accessible</mark>

- (Meta)data are retrievable by their identifier using a standardized communications protocol
    - Protocol is open, free, and universally implementable
    - Protocol allows for an authentication and authorization procedure, where necessary
- Metadata are accessible, even when the data are no longer available
    - Even if you can’t access the data, you should know what’s in the data

#### <mark style="background: #D2B3FFA6;">Interpretable</mark>

- (Meta)data use a formal, accessible, shared, and broadly applicable language for knowledge representation
- (Meta)data use vocabularies that follow FAIR principles
    - Jargon and standards that are not common sense should have handbooks so that people can refer to it
- (Meta)data include qualified references to other (meta)data

#### <mark style="background: #D2B3FFA6;">Reusuable</mark>

- Data should be able to be downloaded and repurposed
- (Meta)data are richly described with a plurality of accurate and relevant attributes
    - (Meta)data are released with a clear and accessible data usage license
    - (Meta)data are associated with detailed provenance
        - Know where it is coming from, the limits of the data
    - (Meta)data meet domain-relevant community standards
        - Needs to adhere to what is expected in industries and domains

> [!important] FAIR Principles provide guidelines for *data access*, but not for *data impact*

- FAIR Principle that data is findable, accessible, interpretable, and reusable underlines the idea that data is objective and neutral
    - ==Does not consider the politics== of data and judgement embedded
    - ==Just because data can be accessed does not mean that data is good==
    - [p] Can have information about exclusions, but
        - [c] If you can still use the data, the exclusions persist
- ==Does not talk about impacts== both in using the data and repercussions in re-using the data
- Does not cover consent, privacy, or protection
    - Discussed earlier in notes

> [!important] FAIR Principles is a good step, but not sufficient

### <mark style="background: #D2B3FFA6;">CARE Principles</mark>

- As an alternative, Indigenous communities have developed the **CARE Principles** for Indigenous Data Governance
    - Data governance principles to explicitly challenge **data colonialism**
- Built on top of FAIR Principles, but
    - Also address some of the issues with FAIR Principles aforementioned

<!-- break -->
![](https://i.imgur.com/kdPeLqm.png)

- **Collective Benefits**
- **Authority to Control**
- **Responsibility**
- **Ethics**
- Is about data and also *data impact*
    - Shifts framework from thinking just about data → thinking about data reasons and people involved in gathering, processing, storing, and re-use

#### <mark style="background: #D2B3FFA6;">Collective Benefit</mark>

- **For inclusive development and innovation**
    - Governments must support Indigenous data use, fostering innovation and self-determined development
- **For improved governance and citizen engagement**
    - Data improves planning, engagement, and decision-making for Indigenous communities, enhancing transparency and understanding of resources and third-party impacts
- **For equitable outcomes**
    - Indigenous data should reflect community values and ensure equitable benefits that support Indigenous well-being and aspirations

#### <mark style="background: #D2B3FFA6;">Authority Of Control</mark>

- **Recognizing rights and interests**
    - Indigenous Peoples have rights to Indigenous Knowledge and data, including the right to ==free, prior, and informed consent== in data collection and policy development
        - i.e., People whom data is collected on have the (ownership) rights to the data
- **Data for governance**
    - Indigenous Peoples have the right to data that aligns with their worldviews and supports self-determination, and such data should be accessible to aid Indigenous governance.
- **Governance of data**
    - Indigenous Peoples have the right to create cultural governance protocols for Indigenous data and lead its stewardship and access, especially concerning Indigenous Knowledge

#### <mark style="background: #D2B3FFA6;">Responsibility</mark>

- **For positive relationships**
    - Indigenous data use requires relationships based on respect, reciprocity, trust, and mutual understanding, as defined by Indigenous Peoples.
    - Those handling Indigenous data must ensure its creation, interpretation, and use respect Indigenous dignity.
    - Related to *data impact*
- **For expanding capability and capacity**
    - Using Indigenous data entails a reciprocal duty to boost data literacy, support an Indigenous data workforce, and develop digital infrastructure for data management and security.
- **For Indigenous languages and worldviews**
    - Resources must support data generation based on the languages, worldviews, and lived experiences of Indigenous Peoples.
    - Pushes back against AI’s homogenizing tendency

#### <mark style="background: #D2B3FFA6;">Ethics</mark>

- Tied to *data impact*
- **For minimizing harm and maximizing benefit**
    - Ethical data avoid stigmatizing Indigenous Peoples and align with Indigenous ethical frameworks and UNDRIP rights. Benefits and harms should be assessed from the Indigenous perspective.
- **For justice**
    - Ethical processes address power imbalances and must include representation from relevant Indigenous communities to respect Indigenous and human rights.
- **For future use**
    - Data governance should consider future uses and harms based on ethical frameworks of relevant Indigenous communities.
    - Metadata should reflect provenance, purpose, limitations, and consent issues.

> [!tldr]+ Summary of CARE Principles
>
> - Built on top of FAIR principles, but
> - Center ==Indigenous ways of thinking== and ==impact==
> - Good way to think about how we challenge colonialism
>     - So many of our default assumptions are based on data colonialism

# Summary

- **Labour issues involved in AI**
    - Labour politics in data annotation
    - The unemployment debate
- **Data colonialism**
    - Extraction
    - Control
- **AI Regulation**
    - What we should do to ensure an ethical development and use of AI technologies
