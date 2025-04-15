---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC207]]"
Week: 11
Module:
  - "[[3 - Professional and Miscellaneous Topics]]"
Lecture:
  - "22"
Chapter: 
Slides/Notes:
  - "[[22-CodeSmells.pdf]]"
  - "[[22-GenAI-prompting.pptx]]"
Date: 2024-11-21
Date created: Fri., Nov. 22, 2024, 3:24:56 am
Date modified: Mon., Dec. 2, 2024, 3:36:28 am
---

# Generative AI

- Type of AI that creates new data based on existing data
- What can generative AI do for you?
    - Text generation (ChatGPT)
    - Image generation (DALL-E)
    - Code generation
        - e.g., Write test code, do a code review, etc.
    - Generate PlantUML code for a UML diagram
    - Help you do a pro/con analysis

## What is Prompt Engineering?

> [!def]+ Prompt
> - Instructions telling someone, or something, what to do

- e.g., “Write me a computer program to play Tic-Tac-Toe”
- “Write me a Java Swing program for X’s and O’s”
- “Design a Java program for playing noughts and crosses”

> [!tip] You can expect better results when prompting a GenAI if your instructions are more precise!

## Why is Prompt Engineering Important

- **Prompt engineering** is the skill of crafting effective inputs (prompts) to
    - Get specific, accurate, or creative outputs from an AI model
    - Is an **iterative process**, which
        - Involves testing different prompts

| Basic Prompt Example           | Advanced Prompt Example                                                                                                                                                       |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| “Help me with a Java for-loop” | “Write a Java for-loop that iterates through an array of integers and prints each element that is an even number. Provide comments explaining each step for a beginner code.” |

- Largely just adding details and context to prompts

# Some Prompt Engineering Techniques

- **Zero-shot prompting**
    - Provides the ML model with a task it has not explicitly been trained on
    - Tests model’s ability to produce relevant outputs without relying on priors examples
- **Few-shot prompting** or **in-context learning**
    - Gives model a few sample outputs (i.e., shots) to
        - Help it learn what the requestor wants it to do
    - Learning model can better understand the desired output if it has context to draw on
    - Opposed to zero-shot prompting
- **Chain-of-thought prompting** (CoT)
    - Advanced technique that provides step-by-step reasoning for the model to follow
    - Breaking down a complex task into intermediate steps or “chains of reasoning”
        - Helps the model achieve better language understanding
        - → Create more accurate outputs
    - You try to be more systematic about your prompts
        - i.e., Give it a sequence of prompts that ==build up the context== of the overall question in mind
    - Allows you to *see* the reasoning behind the model’s output

# How Might We Engineer Our Prompts?

- Could ask a GenAI how best to prompt it
    - Apply  ==Golden Rule==:
        - Treat others as you wish to be treated, so
        - Prompt GenAI as we would another person
- **Layered prompts**
- **Ask the model to adopt a persona**
- **Provide examples**
- Many “frameworks” have been recently proposed for how to engineer effective prompts:
    - e.g., RICCE

![](https://i.imgur.com/92vDGp2.png)

## RICCE

- **Role**
    - Specify the role you want the AI to play
    - Helps set tone and perspective of response
    - e.g., “You are a friendly nutritionist advising someone with a nut allergy”
- **Instructions**
    - Clearly state what you need from the AI
    - Directs the AI on what exactly to do
    - e.g., “List safe food options and explain why they are suitable”
- **Context**
    - Provide relevant background information
    - Shapes the response by giving AI necessary details
    - e.g., “The person is a vegetarian and prefers organic food”
- **Constraints**
    - set boundaries or specify limitations in your request
    - Ensure AI’s response is focused and within desired parameters
    - e.g., “Avoid suggesting any high-calorie foods”
- **Examples**
    - Few-shot prompting
    - Give examples of what you consider a good response
    - Guides AI by demonstrating expected response or content
    - e.g., “Example of a good response: Almonds are unsafe because they are a type of nut. However, you can enjoy sunflower seeds, which are nutritious and nut-free”

## Technique 1: Layered Prompts

- Complex queries often overwhelm AI
- Layered prompts solve this by breaking questions into ==smaller, sequential parts==
- Encourages structured and detailed responses
- Helps guide AI to deeper insights

> [!example]+ Example prompts
> Step 1: “What is photosynthesis”
> Step 2: “How does sunlight contribute to photosynthesis?”
> Step 3: “Explain this process step-by-step at a high-school level”

## Technique 2: Ask the Model to Adopt a Person

- Assign a role to AI to tailor its response style and depth
- Model adapts its **tone**, **knowledge**, and **expertise** to the role you specify

> [!example]+ Example prompts
> - “Explain black holes in a simple and fun way for a 5-year-old”

## Technique 3: Provide Examples

- **Few-shot programming**
- Requires fewer examples than **many-shot** approaches, which might overwhelm the context
- Providing AI with a few examples of desired output format/structure within the prompt itself
- By showing examples, you *teach* the AI how to respond
- Useful for:
    - When you need responses tailored to specific formats or creative styles
    - Tasks like text generation, problem-solving, or summarizing

> [!example]+ Example
> **Prompt:** “I will provide a sentence, and you will summarize it in three words. Examples:
> Sentence: ‘The sun rises in the east.’ Summary: ‘Sun rises east.’
> Sentence: ‘Water boils at 100 degrees Celsius.’ Summary: ‘Water boils 100C.’
> Now, summarize:
> Sentence: ‘Humans need oxygen to survive.’”
> **AI Output:**
> “Humans need oxygen”

> [!summary]+ You get what you ask for
> - Getting quality output from AI tools depends heavily on the ==quality of your prompts==
> - “==Garbage in, garbage out==”
>     - Idea that a system with bad input will produce bad output

# Prompt Tuning

![](https://i.imgur.com/7ijY4E9.jpeg)

- Prompt engineering is a *manual* task
- Cohere AI uses this idea of **prompt tuning**
    - ==Automated prompt engineering==
- **Prompt tuning**:
    - Give an initial prompt
    - Take that prompt and generate completions
    - Evaluate the output (**evaluation service**)
        - Asks the Cohere LLM to score the output given some specification that you’ve provided
        - Specification is the **prompt evaluation criteria**
    - Once you have a **criteria score**, output can be formed as an ==optimization problem==
        - Produces a new prompt
    - **Meta prompt** is going iterate on the prompt that we started on
