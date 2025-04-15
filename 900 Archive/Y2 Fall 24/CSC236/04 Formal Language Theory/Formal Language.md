---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC236]]"
Week: 11
Module:
  - "[[4 - Formal Language Theory]]"
Lecture:
  - "19"
  - "20"
  - "21"
  - "22"
Chapter:
Slides/Notes:
  - https://share.goodnotes.com/s/HXTL4KT4C0CRZkjaKkMhz7
Date: 2024-11-19
Date created: Tue., Nov. 19, 2024, 4:04:31 pm
Date modified: Sun., Dec. 8, 2024, 10:11:33 pm
---

# Formal Language

- **Alphabet**
    - ==Typically denoted by $\Sigma$==
    - A non-empty finite set of *symbols*/*characters*
- **String**
    - ==*Finite* sequence of characters==
    - We do not use quotations:
        - abc, *not* “abc”
    - $\epsilon$ is a name for the empty string “”
        - Empty string has no content; empty list; container that has nothing in it
- **String over $\Sigma$**
    - A string whose characters are all in $\Sigma$
- **$\Sigma^{*}$**
    - The ==set of all strings== over $\Sigma$
- **Language** over $\Sigma$
    - A *set* of strings over $\Sigma$
        - i.e., A subset of $\Sigma^{*}$, an element of $\mathcal{P}(\Sigma^{*})$
            - $\mathcal{P}(\Sigma^{*})$ is a *powerset*
                - The set of all subsets of a given set, including the empty set and the original set itself
                - Size is $2^{n}$, where $n$ is the number of elements in $\Sigma^{*}$
        - Empty set or not
        - Finite set or not
- **Character**
    - Is *context-dependent*
    - Define the characters that we allow in any particular context through an *alphabet*
        - Standard notation for alphabet: $\Sigma$
            - Not a sum; never going to be followed by an algebraic expression
            - → That means it is an alphabet
            - A ==non-empty, finite set== of *allowable* characters
- abc
    - is a string over $\Sigma_{1} = \{ a,b,c \}$
    - is a string over $\Sigma_{2} = \{ a,b,c,1,2 \}$
    - is ==not== a valid string over $\Sigma_{3} = \{ a, b \}$

> [!note]+ $\Sigma$ must be defined so that each string is parsed uniquely
>
> - $\Sigma = \{ a, aa, b \}$
>     - aaa is ambiguous
>         - Can mean a followed by aa
>         - Can mean aa followed by a
>         - Can mean three a’s

## Operations (on Strings, Characters, languages)

- **Concatenation**
    - Use $\cdot$ (dot)
    - If $s, t$ are strings, then $s \cdot t \equiv st$ means all the characters in $s$, followed by all the characters in $t$
    - Examples:
        - $abc \cdot bca = abcbca$
        - $abc \cdot \epsilon = abc$
        - $a \cdot \epsilon \cdot bc = abc$
            - No gap!
    - Consistent with Python:
        - `"abc" + "bca" == "abcbca"`
        - `"abc + "" == "abc"`
        - `"a" + "" + "bc" == "abc"`
- **Length**
    - If $s$ is a string, $\left| s \right| =$ number of characters in $s$
    - $|abc| = 3$
    - $\left| \epsilon \right| = 0$
        - ! $\epsilon$ is not a character
        - $\epsilon$ is the only 0-length string
- ? How do you represent spaces?
    - You don’t
    - Another disadvantage of not using quotation marks

## Notation

- For any **alphabet** $\Sigma, n \in \mathbb{N}$,
    - $\Sigma^{n} = \{ \text{all strings over } \Sigma \text{ with length exactly } n \}$
    - $\{ a, b \}^{2} = \{ aa, ab, ba, bb \}$
        - Strings are *sequences*
            - → ab and ba are different from each other
            - → aa is okay; can have same character twice in a row because it is a sequence; position matters
                - Unlike a set
        - $\{ aa, ab, ba, bb \}$ is a *language*
            - Is a set
            - Elements of set are sequences
            - Those sequences contain individual characters
                - Think of ==strings as *containers*==
            - Two layers of containers to get down to the individual characters
    - $\Sigma^{*} = \{ \text{all strings over } \Sigma \} = \bigcup_{i \in \mathbb{N}} \Sigma^{i} = \Sigma^{0} \cup \Sigma^{1} \cup \Sigma^{2} \cup \cdots$

## Languages

- $\emptyset$ is a language
    - Only way that a set cannot be a language:
        - If it contains something that is not a string
        - Nothing in $\emptyset$ that is not a string $\implies \emptyset$ is a language
    - Empty
- $\Sigma^{*}$
    - Non-empty and infinite
- $\Sigma^{236}$
    - Non-empty and finite
- $\{ a \}$ is a language
    - If $a \in \Sigma$
    - % One-character strings are *ambiguous*
        - Is this a character or a string that contains just that character?
        - Context must be provided
            - Say explicitly if $a$ is a string or character
- $\{ \epsilon \} \neq \emptyset$ is a language
    - No characters in the set, but there is a string
    - $\epsilon \in \{  \epsilon \}$
    - $\epsilon \not\in \emptyset$
    - If you have a bag, and you put a literal empty string in it, your bag is not empty
        - No beads in your bag, but there is a string
        - vs. a totally empty bag that does not even have a string in it → $\emptyset$

## Recognition Problem

> [!goal] We want to study languages, their properties, and their representations

- **The Recognition Problem**
    - Given:
        - $s \in \Sigma^{*}$
            - $s$ is a ==string== by definition of $\Sigma^{*}$
        - $L \subseteq \Sigma^{*}$
            - $L$ is a set and contains strings
            - $\implies L$ is a ==language==
    - ? Is $s \in L$?
        - i.e., Does $s$ belong in $L$?

Consider $L_{1} = \left\{ s \in \{ 0, 1 \}^{*} : s \text{ is a binary representation of an even natural number > 2 that cannot be written as a sum of two primes} \right\}$

- Binary representation → We can have leading zeroes
- Even natural number > 2:
    - Has to end with a zero if it is even
    - Has to have something else other than `10`
- Is $11010100 \in L$?
    - Need to find out what number it is
    - Need to find if there are two primes that add to this number
        - If there are, the string is not in the language
- **Goldbach’s Conjecture**
    - Open problem: Can you write every even number > 2 as a product of two primes
    - Nobody can tell exactly what this language is equal to
        - Might be the empty language, if Goldbach’s conjecture is true
        - Might be non-empty if Goldbach’s conjecture is false

## Representations

- **Finite State Automata** (FSA):
    - Describes a calculation to determine whether $s \in L$ or not
- **Regular Expressions** ([[Regular Expressions|RegEx]]):
    - Describes a pattern that matches every string in $L$, and *only* in $L$

### Example. L/R Game from PP3

> [!note] PP3 refers to [[Structural Induction]]

- There are two circles: $L$ and $R$.
- You are standing in circle $L$.
- Someone calls out a string of letters, each of which is either `a` or `b`.
- Whenever you hear `a`, you stay in or move to $L$.
- Whenever you hear `b`, you move to the circle you are not standing in.
- Let S be the set of all finite strings containing only a’s and/or b’s, which we’ll consider to be defined structurally as follows:
    - $\epsilon \in S$ (recall that $\epsilon$ is the empty string)
    - If $s \in S$, then $sa$ and $sb$ are in $S$

> [!thm]+ Claim.
>
> - You end up in $L$ $\iff$ the number of b’s at the end of the string (after the last a) is even

> [!example]+ Finite State Automata
> Define $L = \{ s \in \{ a, b \}^{*} : s \text{ ends with an even \# of b's} \}$
> (Note that we will call the states $l, r$ to differentiate between $L$ for language)
>
> ![](https://i.imgur.com/i2gnBPJ.png)
>
> - Also known as a *transition diagram*
> - Describes a *process*:
>     - Start in the initial state (L)
>     - For each character, follow the arrows
>     - Last state gives the answer if $s \in M$
>         - $l$ → Yes
>         - $r$ → No

> [!example]+ RegEx
>
> - An even number of b’s: `(bb)*`
> - `*` is an operator
>     - 0 or more repetitions
> - Strings that end in `a`:
>     - `(a + b)*a`
> - `+` is an operator
>     - Meaning: “or”, “alternation”, “this or that”
> - This gives us the Regex `(a + b)*a(bb)`
> - What about the string `bb`?
>     - `((a + b)*a + ε)(bb)*`

## Deterministic Finite Automata

- There are ==deterministic finite automata== and ==non-deterministic finite state automata==
    - FSA refers to either
    - Not being specific
- **Deterministic Finite Automata** (DFA)
    - We use finite automata to distinguish against **finite state machines** (from [[CSC258]])
    - FSM takes an input and produces an output that is made up of not more than a single boolean value
    - Our formalism *always* produces ==one boolean== output
        - More specialized than FSM

![|center|500](https://i.imgur.com/5QlyK71.png)

- \* We start at the initial state $l$ (indicated by the arrow)
- For a string `ababb`,
    - The computation of this automata on the string is:
        - ![|500](https://i.imgur.com/7iXPIYi.png)
    - The state that you are in at the end ($l$) gives you your answer
    - Some states are associated with the state “true” or “false”
        - $l$ is the only state associated with “true”
        - → The string ends with an even number of b’s
- For a string `abab`,
    - Then the processing would stop at $l \to l \to r \to l \to r$
    - Answer would be false; string does not end with an even number of b’s
- The small calculation/computation model captures an algorithm
    - That tells you the answer: Does a string belong to the language $M$, yes or no?

### In General

> [!question]+ What makes up a finite state automata?
>
> - Finite, non-empty set of **states** $Q$
>     - $Q = \{ l, r \}$
>         - $l, r$ are just labels
>         - Names that you give to the states are arbitrary and just for convenience; have no implicit meaning
> - Input **alphabet** $\Sigma$
>     - $\Sigma = \{ a, b \}$
> - **Initial**/start **state** $s \in Q$
>     - $s = l$
>     - Convention: Indicated by diagram; exactly one initial state
> - A set of **accepting states** $F \subseteq Q$
>     - $F = \{ l \}$
>     - In general, $F$ can be anything from $\emptyset$ to all of $Q$
> - **Transition function** $\delta : Q \times \Sigma \to Q$
>     - For each $q \in Q, a \in \Sigma, \delta(q, a) =$ next state after reading/processing character $a$ from state $Q$
>     - e.g., $\delta(l, a) = l$
>     - ![|300](https://i.imgur.com/2vAtTgf.png)

### Regular Expressions (RE)

From [[#Example. L/R Game from PP3|example]], we had:

- $(\epsilon + (a + b)^{*}a)(bb)^{*}$
    - To guarantee that we have an ==even number of b’s at the end==,
        - Need to make sure that is in front of it does *not* end with b
        - It can be ==empty==, or it can ==end with a==, which is what we have in the expression

> [!info]+ We define regular expressions *recursively*
>
> - Formally, for each alphabet $\Sigma$, define $RE_{\Sigma}$ as follows:
>     - $\emptyset \in RE_{\Sigma}, \epsilon \in RE_{\Sigma}$
>         - $\emptyset$ describes the (empty) language with no strings: $\{  \}$ (see [Piazza@621](https://piazza.com/class/lwkvixjjc4f2ew/post/621))
>     - $a \in RE_{\Sigma}$ for each $a \in \Sigma$
>     - For each $R, S \in RE_{\Sigma}$,
>         - $(R \oplus S) \in RE_{\Sigma}$
>         - $(R \odot S) \in RE_{\Sigma}$, or just $(RS)$
>         - $R^{*} \in RE_{\Sigma}$
>     - Nothing else

> [!tip]+ Think of $RE_{\Sigma}$ as a set of strings over $\{ \emptyset, \epsilon, a, b, *, \oplus, \odot \}$ (assuming $\Sigma = \{ a, b \}$)
>
> - Note that these are Regex operators, *not* string operators

- Note: *precedence*
    - $* > \cdot > +$
    - e.g., ==$a + b \cdot c = a + (b \cdot c)$== *not* $(a + b) \cdot c$
- e.g., $RE_{\{ a, b \}} = \{ \emptyset, \epsilon, a, b, \emptyset^{*}, \epsilon^{*}, a^{*}, b^{*}, \emptyset^{***}, \dots, (\emptyset + \emptyset)\cdot(a^{*} + b \epsilon), \dots \}$

### Relationship with Languages

- For each regex $R \in RE_{\Sigma}$, define the language of $R$ as follows:
    - $R = \emptyset : L(R) = \emptyset$
    - $R = \epsilon : L(R) = \{ \epsilon \}$
        - Non-empty set that contains the string that contains no characters
    - $R = a \in \Sigma : L(R) = \{ a \}$
    - $R = (S \oplus T) : L(R) = L(S) \cup L(T)$
    - $R = (S \odot T) : L(R) = \{ s \cdot t : s \in L(S) \text{ and } t \in L(T) \}$
    - $R = S^{*} : L(R) = \{ s_{1} \cdot \dots \cdot s_{k} : k \in \mathbb{N} \text{ and } s_{1}, \dots, s_{k} \in L(S) \}$
        - Note: When $k = 0, s_{1} \cdots s_{k} = \epsilon$
            - $\epsilon$ is the *identity element* for concatenation
- For each DFA $A = (Q, \Sigma, \delta, s, F)$,
    - $L(A) = \{ x \in \Sigma^{*} : \text{processing each character of }x \text{, one-by-one, left-to-right, starting from state }s \text{, according to } \delta \text{, reaches a state in } F \}$
        - Take your string → Start at the start state → Look at the characters one-by-one and follow the arrows → Done; no more characters to read → Look at the state you’re currently in
            - If it is a state that is accepting ($\in F$), the string is in the language $L(A)$
            - If the state is not accepting ($\not\in F$), the string is ==not== in the language $L(A)$
            - DFA $\implies$ There is exactly one possible state you could be in after processing each string

### Example

$$
\begin{align*}
L &= \{ s \in \{ a, b \}^{*} : \text{the second-last character of } s \text{ (exists and) is b} \} \\
&= \{ ba, bb, aba, abb, bba, bbb, \dots \}
\end{align*}
$$

> [!info] When we say “the second-last character of $s$ is b,” we implicitly assume it *exists*

- Regular expression of $L$
    - $(a+b)^{*}(ba + bb)$
    - $(a + b)^{*}b(a + b)$
    - These two regexs are *not* the same, but have the ==same value== $\iff$ **equivalent** (same language)
        - e.g., $2(n + 1)$ and $2n + 2$ in algebra; are not the same, but have the same value
- DFA
    - $\to \bigcirc$
        - Initial state: accepting state or not?
            - ? Is there a string that ends in this state?
                - Empty string $\epsilon$
                - & Want output of DFA to match whether $\epsilon$ is in $L$
        - $\epsilon \not\in L$ → Initial state is not accepting ($\not\in F$)
    - Say that you’ve processed an `a` and processed a `b`
        - Is there another sequence of characters to process where the result should be different at the end?
        - Add an `a` after each one:
            - `aa` → No
            - `ba` → Yes
            - $\implies$ `a` and `b` cannot go to the same state
                - Otherwise, transition on a would go to the same next state → Cannot get two different answers
            - `b` is the one where we need something different, so add a state for reading `b`
    - $\to \bigcirc \overbrace{\longrightarrow}^{b} \bigcirc$
        - Then, at the initial state, when you read an `a`, you can stay at the state
    - ![|300](https://i.imgur.com/MlMNDiy.png)
        - When you read an `a`, no chance that next character is going to make the second-last character `b`
        - Last thing you have read can only be an `a`
        - Adding one thing next to it → second-last character can only be an `a`

![](https://i.imgur.com/0bmZdkG.png)

> [!question]+ How did we know that when we were in state $q_{0}$ and read an `a` that we could stay in state $q_{0}$
>
> - One thing you can do after coming up with DFA:
>     - Trace DFA on a bunch of strings → See if you end up where you should be
> - When you read `a` and you’re in $q_{0}$,
>     - No way that the second-last character is a `b`
> - When you read `b` in $q_{0}$
>     - Also does not make second last character a `b`
>
> | Strings in $q_{0}$ |
> | ------------------ |
> | $\epsilon$         |
> | $a$                |
> | $aa$               |
>
> - Look at two of these strings: $\epsilon$ and $a$
>     - Let $t \in \{ a, b \}^{*}$ → $t = \epsilon \vee t = a$
>     - & Is there $t$ such that $\epsilon \cdot t \in L, a \cdot t \not\in L$ *or* $\epsilon \cdot t \not\in L, a \cdot t \in L$
>     - If there is a way to follow them by the same sequence of characters, so that the result in one case is in your language and the other case is not in the language
>         - & Cannot have these two strings be in the same state
> - This is what happens when you are in state $q_{0}$ and read a `b`:
>
> | Strings in $q_{0}$ | Operation on character with $t = a \in \{ a, b \}^{*}$ |
> | ------------------ | ------------------------------------------------------ |
> | $\epsilon$         | $\epsilon \cdot a \not\in L$                           |
> | $b$                | $b \cdot a \in L$                                      |
>
> - If you stay in the same state after reading `b` from state $q_{0}$
>     - Behaviour after reading one more character has to be the same in DFA
>     - Cannot make a difference between the two outcomes $\epsilon \cdot a$ and $b \cdot a$
>     - $\implies$ New state required
>     - This never happens with $\epsilon$ and $a$
>         - Either they are both in the language or both not in the language

> [!def]+ Distinguishable
> Strings $u, v \in \Sigma^{*}$ are **distinguishable** *with respect to* $L \subseteq \Sigma^{*}$
> $$\iff \exists t \in \Sigma^{*} s.t. (u \cdot t \in L \wedge v \cdot t \not\in L) \vee (u \cdot t \in L \wedge v \cdot t \in L)$$

- For $L$ above:
    - $\epsilon, b$ are **distinguishable**
    - If you follow both by the same string (single character with `a`) → Result of one is not in the language and other one is in the language
    - $\epsilon, a$ are **indistinguishable**
        - No matter what string you put after both of them → Two results are either in the language or outside of the language
        - Answer never changes

> [!tip]+ When designing DFAs
>
> - & Use **distinguishable** to decide whether to go to a new state or not
> - Ask yourself: What are the strings that take you to the initial state
>     - Very few strings when you start: $\epsilon$ and single characters
>         - If you can distinguish → Need new state
>         - If not → can stay in state where you were previously

> [!important]+ In any DFA for $L$, ==**distinguishable** strings must end up in different states==
>
> - Strings $u, v \in \Sigma$ are distinguishable $\implies$ must be in different states
> - ! Not an if and only if — just an implication
>     - Nothing that says the DFA has to take all indistinguishable strings to the same state

### State Invariant

> [!def]+ State invariant
>
> - Complete list of all strings that end up in each state
> - The lists cover $\Sigma^{*}$ completely

For our example, we have state invariant(s):

- $q_{0} : \dots aa$ or $a$ or $\epsilon$
- $q_{1} : \dots ab$ or $b$
- $q_{2} : \dots ba$
- $q_{3} : \dots bb$

> [!obs] Come up with (or pick) one string per state: $\{ aa, ab, ba, bb \}$

- Pick something nice, simple, and uniform

> [!thm]+ Claim. All these strings are **pairwise distinguishable** with respect to $L$
>
> - Any two strings in the set can be distinguished

- There are 4 strings → $4C2 = \begin{pmatrix}4 \\ 2 \end{pmatrix} = \frac{24}{(4-2)!2!} = 6$ possible pairs
- Some of the pairings are easy to distinguish:
    - Pick any string in $L$ and string not in $L$:
        - $$\begin{align*} aa &\not\in L \\ ba &\in L \end{align*}$$
    - Think of following them by empty string $\epsilon$
    - $\epsilon$ is a common suffix that results in one string in $L$ and one string not in $L$
- For the others, pick any character suffix:
    - $$
        \begin{align*}
        aa \cdot a &\not\in L \\
        ab \cdot a &\in L \\
        \\
        ba \cdot a &\not\in L \\
        bb \cdot a &\in L \\
        \\
        \vdots
        \end{align*}
        $$

> [!obs]+ Consequence. Every DFA for $L$ needs at least 4 states.
>
> - One for each of these strings
> - Any DFA that uses fewer than 4 states would have to send 2 of these strings to the same state, but
>     - Those two strings are distinguishable → There is a suffix that takes one outside of $L$ and one inside of $L$
>     - $\implies$ DFA would be making an error on one of the two strings
> - Part of the **Myhill-Nerode Theorem**

- **Myhill-Nerode Theorem** also says:
    - If you find a way to split up all the strings into equivalence classes (e.g., $\{ aa, ab, ba, bb \}$ are our 4 equivalence classes),
        - Then there is a DFA with number of equivalence classes’ states (e.g., 4)
        - Equivalence classes:
            - If you take the set of all strings $\Sigma^{*}$, and
            - Can split it into all strings indistinguishable from $aa$, from $ab$, and so on, and
            - That covers everything
            - $\implies$ There exists a DFA with that many states
- $\implies$ We know that our DFA is optimal
    - No smaller DFA for language $L$

## Non-Deterministic Finite Automata (NFA)

### Example

Given one possible regex that matches our language $L$: $(a + b)^{*}b(a + b)$

- Given a string, does the string match the regular expression?
- Given a (long) string, one character at a time:
    - ? What do you keep track of?
    - ? How do you keep track of it?

![](https://i.imgur.com/3aJWdtV.png)

Trace the string `abbaa`:

![](https://i.imgur.com/I0cNSvM.png)

> [!info]+ In general
>
> - At each step, NFA is some *subset* of states
>     - No restriction
>         - Could be $\emptyset, Q$, or anything in between
> - Includes start as one step
>     - We allow NFA to start at some subset of states at start
>         - We might see more than one arrow indicating start → That is ok
>     - Instead of one in DFA

- We still have **transitions**, and (non)**accepting states**
- $\delta : Q \times \Sigma \to \mathcal{P}(Q)$
    - *Power set*
    - For each $(q, c) \in Q \times \Sigma$, $\delta(q, c) =$ *set* of next states
- Starting from $S \subseteq Q$ (**initial set**), next set of states = union of $\delta(q, c)$ for each $q \in$ current set of states
    - At any point in time, including the beginning:
        - Think of where we are at as a *set* of states
        - For each state in set, we apply $\delta$ to find our next states
        - Look at resulting set and union all results
        - → Set of states that we are in
        - Sometimes, $\delta$ says next state is empty
            - → Not contributing anything to the next set of states
- ? What is the output?
    - Accept $\iff$ last set of states contains at least one accepting state

## Subset Construction

> [!thm]+ Subset construction
>
> - Every NFA can be converted to a DFA for the same language, but
> - It may need many more states

> [!obs]+ Intuition. Each state of the DFA corresponds to one *subset* of the states of the NFA

- How many more states might we need for DFA?
    - If every subset of NFA could be a different state for DFA, you might need as many as $2^{n}$ states, where $n$ is the number of states in the NFA

## Product Construction

### Example

Define language $L = \{ s \in \{ a, b \}^{*} : \textcolor{DarkSeaGreen}{\underbrace{s \text{ has an even \# of a's}}_{L_2}} \text{ AND } \textcolor{LightBlue}{\underbrace{\text{second-last character is b}}_{L_1}} \}$

- We have a DFA and NFA for all strings whose “second-last character is b”
- Now, adding that strings have an even number of a’s
- ? How do we do that?
- Condition in language is made up of two *sub-conditions*
    - Define $A_{1}$ to be the DFA for “second-last character is b”
        - ![|495](https://i.imgur.com/PRSx031.png)
        - $\mathcal{L}(A_{1}) = L_{1}$
    - Define $A_{2}$ to be the DFA for “even # a’s”:
        - ![|400](https://i.imgur.com/5RZOiVT.png)
        - $\mathcal{L}(A_{2}) = L_{2}$
- Make a DFA where every state of the DFA ==corresponds to a pair of states== — one in DFA $A_{1}$ and one in DFA $A_{2}$
    - ![|500](https://i.imgur.com/4T0f2qk.png)
    - Each state in our product construction DFA matches a pair between each state of the original DFAs

> [!question]+ Which is the initial state?
>
> - The product state of the one that tracks the initial states of $A_{1}, A_{2}$

> [!question]+ Which states should be accepting?
>
> - Want to accept when the state has the accept property in both DFA

![|center|500](https://i.imgur.com/9DvS489.png)

> [!question]+ How do we decide the transitions?
>
> - Start at a state and see what happens in each DFA when you read an `a` or `b`
> - Transition to the intersection of these two states
> ![|500](https://i.imgur.com/iiG7drD.png)

- Define $A$ to be our product construction DFA of $A_{1}$ and $A_{2}$
- $L(A) = L$

> [!obs]+ Observation.
> $$L = L_{1} \cap L_{2}$$

> [!def]+ Regular
>
> - Language $L \subseteq \Sigma^{*}$ is **regular**
>     - iff $L = \mathcal{L}(R)$ for some $R \in RE_{\Sigma}$
>     - iff $L = \mathcal{L}(A)$ for some DFA $A$
>     - iff $L = \mathcal{L}(A')$ for some NFA $A'$

- Can interpret this definition in many ways:
    - Definition of a language being *regular* can be done based on any one of these iff’s
        - → Will get same class of languages being labelled as regular
    - Anytime you have a regular expression, there is a DFA and NFA for the same language
    - Anytime you have a DFA, there is a regular expression and NFA for that same language
    - Anytime you have an NFA, there is a regular expression and DFA for the same language
    - Equivalent in terms of their ability to *define* languages
        - Not equivalent in terms of *how* they define the same language
            - Already seen that in NFA and DFA in subset construction
            - If you start from a language with a simple NFA, you might need exponentially more states to get DFA for same language
            - Will always be able to get a DFA for the same language, but more efficient with NFA for certain languages

## Language Operations (Closure Properties for Regular Languages)

- **Closure properties for regular languages**
    - What are the kinds of things we can do to regular languages that keep them regular?

> [!summary] So far…
>
> - We have already seen that if we have two languages that have DFAs — meaning they are *regular* — and take their intersection,
>     - The result is regular
> - & Intersection of *regular* languages is *regular*

For each regular languages $L_{1}, L_{2} \subseteq \Sigma^{*}$,

- **Product construction** $L_{1} \cap L_{2}$ is regular
- **Union** $L_{1} \cup L_{2}$
    - If you replace the *and* with *or* in $L$, we can make any state that has one state in the initial DFA that is accepting an accepting state in the product construction DFA
        - There is a much simpler, direct way to do union
    - $$L_{1} \text{ is regular} \implies \exists R_{1} \in RE_{\Sigma} s.t. \mathcal{L}(R_{1}) = L_{1}$$
    - $$L_{2} \text{ is regular} \implies \exists R_{2} \in RE_{\Sigma} s.t. \mathcal{L}(R_{2}) = L_{2}$$
    - $\therefore \mathcal{L}(R_{1} + R_{2}) = L_{1} \cup L_{2}$
- **Complement** $\bar{L} = \Sigma^{*} - L$
    - Every string that is *not* in $L$
    - If you have a regular expression (pattern) for a language, is there a way to take that pattern and turn it to match only the strings in the complement?
    - Use **deterministic finite automata**:
        - $L_{1}$ is regular → $\exists$ DFA $A_{1} s.t. \mathcal{L}(A_{1}) = L_{1}$
        - DFA $A_{1}$ has $(Q, \Sigma, \delta_{1}, q_{\text{start}}, F)$
        - Construct $A_{1}' = (Q, \Sigma, \delta, q_{\text{start}}, Q - F)$
            - Every state that used to accept will now reject; every state that used to reject will now accept
        - Then, $A_{1}'$ rejects inputs $\iff$ $A_{1}$ accepts
        - $A_{1}'$ accepts $\iff$ $A_{1}$ rejects
    - $\therefore \mathcal{L}(A_{1}') = \overline{L_{1}}$
    - This idea would *not* work with an NFA

These are all set operations; there are lots of other non-set language operations that also preserve regularity.

- e.g., Doubling a string

## “Equivalence” For Regular Expressions, DFAs, NFAs

- *Equivalent* in the way that
    - Any language defined by one of these can also be defined by the other two
    - Not saying specific relationships between two or three of these
        - e.g., Not saying that DFA has to be the size of $x$ because of of the regular expression or NFA, etc.

<!-- break -->
1. NFA → DFA:
    - **Subset construction**
2. DFA → RE:
    - **State-elimination construction**
3. RE → NFA:
    - **Recursive definition**

### State-Elimination Construction Example

![|495](https://i.imgur.com/5H07aeI.png)

#### Aside: Every DFA is an NFA

- ? Is this a DFA?
    - Exactly one initial state
    - Every state and character has exactly one transition
- & Just looking at the picture, you cannot tell if it is a DFA or NFA:
    - Could be NFA with an initial set of states that contains that one state
    - Transition for each state and character is to a (power) set of states that contains always one next state
    - Perfectly-valid NFA
- This gives the argument that ==every DFA is an NFA==

#### State-Elimination Construction

> [!thm]+ State-Elimination Construction
> For *each* accepting state, one-by-one, begin with the whole DFA, and remove every state *except* the initial and the target accepting state.

- Remove $q_{1}, q_{2}$ in example
- Say we pick $q_{1}$ to remove first
- List all other states that have a transition coming into $q_{1}$ except for maybe $q_{1}$
    - $q_{0}, q_{2}$
- List all the states that $q_{1}$ has a transition going out to.
    - $q_{3}, q_{2}$

![|465](https://i.imgur.com/pqT3UMa.png)

- Keep track of all the ways the picked accepting state $q_{1}$ interacts with the rest of the DFA
    - To get rid, you want these interactions to be preserved
        - Can use $q_{1}$ to go from $q_{0}$ to $q_{2}$, or
        - Start at $q_{0}$, and go to $q_{3}$, or
        - …
- Draw a transition diagram that looks similar to a DFA or NFA, but
    - ==is not a DFA or NFA!==
    - ![|370](https://i.imgur.com/lwhNd6c.png)

- To get from $q_{0}$ to $q_{2}$:
    - Can read just a 1, or a 01 from above diagram
    - ![|430](https://i.imgur.com/ak9LaVB.png)
    - Interpret labels as regular expressions

![|529](https://i.imgur.com/sZeGuVi.png)

- Remove $q_{2}$: ![|242](https://i.imgur.com/uLDBJTJ.png)
- Final regular expression $RE: \Big( 00 + (01 + 1)(01)^{*}(1+00) \Big)\Big(0 + 1 \Big)^{*}$
    - Get to the accepting state, and stay there
    - Staying at $q_{3}$ involves looping in $q_{3}$
        - If you have transitions going back, you have to account that as well
        - Might loop 0 or more times at $q_{0}$, followed by regular expression to get from $q_{0}\to q_{3}$
            - $00 + (01 + 1)(01)^{*}(1 + 00)$
        - Once you get to $q_{3}$, you might loop at $q_{3}$ by reading $(0 + 1)$, or going back to $q_{0}$, and so on
        - Covers all possibilities
    - Here, it is simpler

> [!obs] If we look at our original DFA, we can describe the language more simple than the RE is.

### RE → NFA: Recursive Definition

![|452](https://i.imgur.com/mZLnq4p.png)

- In each case, $\mathcal{L}(R) = \mathcal{L}(A)$
    - i.e., In each case, the NFA on the right has a language that is equal to the language of the regular expression on the left
- For the empty set, there is no initial set of states → $S = \emptyset \subseteq Q$
    - NFA starts in no state and processes all of the characters
    - Every character it processes is not in any state at all → Stays in no state
    - At end: no state at all; valid set of states that it can be in
    - → Certainly not in an accepting state, so it rejects the string
        - Even though there is one accepting state

#### Construct NFAs Based on NFAs for These

Let $R_{1}, R_{2} \in RE_{\Sigma}$.
Assume $A_{1}, A_{2}$ are NFA such that $\mathcal{L}(A_{1}) = \mathcal{L}(R_{1}), \mathcal{L}(A_{2}) = \mathcal{L}(R_{2})$.

> [!note]+ Each NFA we construct has ==exactly one== accepting state, with ==no outgoing transition==
> ![|500](https://i.imgur.com/pY1HBjI.png)
>
> - Special case:
>     - Treat initial state “arrow” like incoming transition
> - We see that all our basic NFAs in the example satisfy this property

- **“Or”/Union: $R_{1} + R_{2}$**
    - ![|500](https://i.imgur.com/RoQqYvF.png)
    - Put the two NFAs together with all of their incoming transitions
    - Create a bigger NFA with both NFAs together for the union
        - Will start in the set of states that contains all the start states of both $A_{1}, A_{2}$
        - Calculate on both NFAs at the same time
        - At the end: Will be in one of these two accepting states, if the string should be accepted by one or the other
    - Drawback: Not respecting the fact that you want the NFAs to have exactly one accepting state
        - → *Merge* the two accepting states into *one* accepting state
        - ![|500](https://i.imgur.com/4TEGU14.png)
    - Only way that this merge will cause trouble is if:
        - Had transitions going out of the accepting state into one of the NFAs
            - Could get to the state from the other NFA and move back and forth between the two
            - No outgoing transition from assumption, so it is okay to merge
- **Concatenation $R_{1} \cdot R_{2}$**
    - Get rid of the accepting state in $A_{1}$
        - ![|500](https://i.imgur.com/y7c3Vor.png)
        - If you get to the accepting state in $A_{1}$, you have processed some sequence of characters that is in $\mathcal{L}(A_{1}) = \mathcal{L}(R_{1})$
    - When you reach a string that is accepted by $A_{1}$, you want to keep processing them in $A_{1}$, *and also* continue processing them in $A_{2}$
        - ![|500](https://i.imgur.com/ndEAPVL.png)
    - Have to think about the initial states: Are these the initial states of the new NFA or not?
        - Will be an initial state if the crossed-out state was an initial state
            - If $A_{1}$ could start in its accepting state, you want to be able to start in any of the start states (incoming transitions) into $A_{2}$
            - If $A_{1}$ could *not* start in its accepting state, then none of the start states (incoming transitions) into $A_{2}$ are initial states
                - Have to first process something in $A_{1}$
        - & $A$‘s start states remain start states iff $A_{1}$’s accepting state is a start state for $A_{1}$
        - ![|700](https://i.imgur.com/y6bCqir.png)
- **Kleene-star $R_{1}^{*}$**
    - If not already the case, make ==accepting state be a start state==
        - ![|500](https://i.imgur.com/zhr2pUg.png)
        - $R_{1}^{*}$ includes 0 repetitions → Want to start in accepting state, so $\epsilon$ is accepted
        - Starting at the accepting state, any character you read takes you out of the state directly
        - Only accepts $\epsilon$ and nothing else at this starting accepting state
    - Every transition that takes you to the accepting state:
        - Duplicate and send it back in a loop into each of the initial states
        - ![|500](https://i.imgur.com/2UyJ15D.png)

#### Example. $(a^{*} + b)c$

![|500](https://i.imgur.com/FHbqePX.png)

- What states do you start in?
    - Start in both states
    - Accepts because it contains the accepting state
    - If you read an `a`:
        - From the accepting state, you go nowhere
        - From the left state, you go to both states
        - → Every time you read an `a`, you stay in both states which is accepting
        - If you read anything other than an `a`, you go nowhere → No longer accepts
    - NFA accepts any repetitions of `a` (including zero) of the character `a`

![|300](https://i.imgur.com/0IxpOYk.png)

Then, the NFA for $a^{*} + b$ is:

![|500](https://i.imgur.com/2DcVL7u.png)

- Merge the accepting state into a single accepting state at the end
- Starts in every state
    - Can trace what happens when you read an `a`:
        - Leave the state at the bottom state and accepting state → Go nowhere
        - But also in both of the top state and accepting state when starting at the top state
    - If you read a single `b`:
        - Exit the top initial state and accepting initial state
        - But, starting at the bottom state: Single `b` takes you to the accepting state

Then, the NFA for $(a^{*} + b)c$ is:

![|448](https://i.imgur.com/xTx6K01.png)

- Since the accepting state was a start state for $R_{1}$, then the start state for $R_{2} = c$ is *still* a start state!
    - Recall: “$A$‘s start states remain start states iff $A_{1}$’s accepting state is a start state for $A_{1}$”

> [!abstract]+ This process works for every regular expression; this is a ==general process==
>
> - We have described how to deal with all the basic regular expressions
> - Described how to deal with regular expressions that are *not* basic
>     - Union, concatenation, Kleene-star
>     - Described how to take arbitrary NFAs and combine them into new NFAs for the same language (in [[#Construct NFAs Based on NFAs for These]])

If we change the order to construct the NFA for $c(a^{*} + b)$:

![|447](https://i.imgur.com/u09Wol1.png)

- From the definition of $R_{1} \cdot R_{2}$, take every transition from the first regular expression $R_{1}$ that would have gone into its accepting state; send it to each of the initial states of the second regular expression $R_{2}$ instead
    - There are 3 initial states for $R_{2} = (a^{*} + b)$

---

## Non-Regular Languages

Consider $L = \{ a^{n}b^{n} : n \in \mathbb{N} \} = \{ \text{some number of a's followed by the same number of b's} \} = \{ \epsilon, ab, aabb, aaabbb, \dots \}$.

> [!thm]+ Claim.
> This language $L$ is *not* regular.

- $a^{n}b^{n}$ is a perfectly-fine notation to describe strings
    - It is *not* a regular expression
- Isn’t the language just $\mathcal{L}(a^{*}b^{*})$?
    - It does not have to be the same number here
    - $a^{*}b^{*}$ matches strings like `aaab`

> [!question]+ How do we prove that this is not regular?
>
> - Already know how to do this to know that a DFA has the smallest number of states
> - Notion of **distinguishability**

> [!question]+ What are some strings that are distinguishable with respect to $L$?
>
> - Start with two strings where you can put a suffix and the result is that one is in $L$ and the other is not:
>     - `ab` $\in L$
>     - `a`$\not\in L$
>     - These two are not going to generalize, though
> - Try $a, aa$:
>     - $a \cdot b \in L$
>     - $aa \cdot b \not\in L$
>     - → Every NFA for $L$ needs at least two states
>     - Keep adding a’s:
>         - $aaa$
>         - $aaaa$
>         - These strings are all distinguishable from $a$
> - Add two b’s as the suffix:
>     - $aabb \in L$
>     - $aaabb \not\in L$
>     - $aaaabb \not\in L$
>     - This distinguishes $aa$ from the others
> - If you add 3 b’s, it distinguishes the string with 3 a’s from the others
> - If you follow it with 4 b’s → Distinguishes string with 4 a’s from the others
> - → So far, minimum DFA states = 4
>     - Have 4 strings that are distinguishable from each other
> - Problem: If we keep adding a’s, we never run out of distinguishable strings
>     - Can keep adding more and more strings

- Here, the set of strings $\{ \epsilon, a, aa, aaa, \dots \}$ contains strings that are *all* **pairwise distinguishable** from each other
    - Pick any two strings $i \neq j$:
        - $a^{i} \cdot b ^{i} \in L$
        - $a^{j} \cdot b^{i} \not\in L$
- $\implies \therefore$ Every DFA needs $\infty$ many states
- $\therefore$ No DFA for $L$

For contrast and understanding, consider a set of *indistinguishable* strings with respect to $L$:

- $\{ b, ba, abb, \dots \}$
    - None of these strings are in the language
    - Adding anything to the end will still not be in the language

### Prove that a Language is not Regular

> [!tip]+ To show that a language $L$ is not regular:
>
> - Come up with an infinite set of strings
> - Show that any two strings in that set are *distinguishable* from each other with respect to $L$
> - Conclude that there is no DFA for $L$
>     - If there is no DFA $\implies$ no NFA and regular expression for $L$
