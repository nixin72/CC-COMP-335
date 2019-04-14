## Introduction to Theoretical Computer Science

- **Automata** = Abstract Computing Devices
- Allan Turing studied **Turing Machines** (= computers) before there were any real computers. 
- We will also look at simpler devices than Turing Machines (Finite State Automata, Pushdown Automata, ...), and specification means, such as grammars and regular expressions. 
- **NP-hardness** = What cannot be efficiently computed. 
- **Undecidability** = What cannot be computed at all 

### Finite Automata
Finite Automata are used as a model for:
- Software for designing digital circuits
- Lexical analyzer of a compiler 
- Searching for keywords in a file or on the web
- Software for verifying finite state systems, such as communication protcols 

- Example: Finite Automaton Modelling an on/off switch

```
-> OFF
:OFF -> push -> :ON
:ON -> push -> :OFF
```

- Example: Finite automaton recognizing the string "then"

```
-> ""
"" -> t -> T
T -> h -> TH
TH -> e -> THE
THE -> n -> THEN
<- THEN
```

### Structural Representations
There are alternative ways of specifying a machine
**Grammars:** A rule like $E \Rightarrow E + E$ specifies an arithmetic expression
- $Lineup \Rightarrow Person.Lineup$
  $Lineup \Rightarrow Person$

Says that a lineup is a single person or a person in fron of a lineup. 
**Regular Expressions:** Denote structure of data, e.g.
`[A-Z][a-z]*[][A-Z][A-Z]`
matches `Ithaca NY`
but does not match `Palo Alto CA`

### Central Concept
**Alphabet:** Finite, nonempty sets of symbols
**Example:** $\Sigma = \{0, 1\}$ binary alphabet
**Example:** $\Sigma = \{a,b,c,...,z\}$ the set of all lowercase letters
**Example:** The set of all lowercase letters

**Strings:** Finite *sequences* of symbols from an alphabet $\Sigma$, ex. "0011001"
**Empty String:** The string with zero occurences of symbols from $\Sigma$
  - The empty string is denoted by $\epsilon$

**Length of String:** Number of positions for symbols in the string.
- $|w|$ denotes the length of string $w$.
- $|0110| = 4, |\epsilon| = 0$
  
**Powers of an Alphabet:** $\Sigma^k = $ the set of strings of length $k$ with symbols from $\Sigma$.
- Example: $\Sigma = \{0, 1\}$
    $\Sigma^0 = \{\epsilon\}$
    $\Sigma^1 = \{0,1\}$
    $\Sigma^2 = \{00,01,10,11\}$

The set of all strings over $\Sigma$ is denoted $\Sigma^*$
$\Sigma^* = \Sigma^0 \cup \Sigma^1 \cup \Sigma^2 \cup ...$
Also:
$\Sigma^+ = \Sigma^1 \cup \Sigma^2 \cup \Sigma^3 \cup ...$
$\Sigma^* = \Sigma^+ \cup \{\epsilon\}$

**Concatenation:** If $x$ and $y$ are strings, then $xy$ is the string obtained by placing a copy of $y$ immediately after a copy of $x$.
$x=a_1a_2...a_i$
$y=b_1b_2...b_j$
$xy=a_1a_2...a_ib_1b_2...b_j$

Example: $x=01101,~ ~ y=110,~ ~ xy=01101110$
**Note:** For any string $x$, $x\epsilon = \epsilon x = x$

### Languages:
If $\Sigma$ is an alphabet, and $L \subseteq \Sigma^*$ then $L$ is a language. 
Examples of languages:
- The set of legal English words
- The set of legal C programs
- The set of strings consisting of $n$ 0's followed by $n$ 1's 
  - $\{\epsilon, 01, 0011, 000111, 00001111,...\}$
- The set of strings with equal number of 0's and 1's
  - $\{\epsilon, 01, 10, 0011, 0101, 1001, 1010, ...\}$
- $L_p =$ the set of binary numbers whole value is prime
  - $\{10, 11, 101, 111, 1011, ...\}$
- The empty language $\emptyset$
- The language $\{\epsilon\}$ consisting of the empty string 

**Note:** $\emptyset \neq \{\epsilon\}$
**Note:** The underlying alphabet $\Sigma$ is always finite.

**Problem:** Is a given string $w$ a memeber of a language $L$?
Example: Is a binary number prime = is it a memeber in $L_p$
is $11101 \in L_p$? What computational resources are needed to answer the question. 

Usually we think of problems not as a yes/no decisions, but as something that transforms an input into an output

Example: Parse a C-program = Check if the program is correct, and if it is, produce a parse tree.

Let $L_x$ be the set of all valid programs in proglang $X$. If we can show that determining membership in $L_x$ is hard, then parsing programs written in $X$ cannot be easier. 

**Question:** Why?

### Finite Automata Informally
Protocol for e-commerce using e-money
**Allowed events:** 
1. The customer can *pay* the store (= send the money file to the store)
2. The customer can *cancel* the money (like putting a stop on a check)
3. The store can *ship* the goods to the customer
4. The store can *redeem* the money (= cash the check)
5. The bank can *transfer* the money to the store

#### e-commerce
The protocol for each participant:

<img src="https://cdn.discordapp.com/attachments/517047603867811899/533439809402830899/unknown.png">

Completed protocols:

<img src="https://cdn.discordapp.com/attachments/517047603867811899/533439982044708864/unknown.png">

The entire system as an Automaton:

<img src="https://cdn.discordapp.com/attachments/517047603867811899/533440121958301705/unknown.png">


### Deterministic Finite Automata
A **DFA** is a quintuple
$A=(Q, \Sigma, \delta, q_0, F)$
- $Q$ is a finite set of *states*
- $\Sigma$ is a *finite* alphabet (= input symbols)
- $\delta$ is a *transition* function $(q, a) \rightarrowtail p$
- $q_0 \in Q$ is the *start state*
- $F \subseteq Q$ is a set of *final states*

**Example:** An automaton $A$ that accepts $L = \{x01y~:x,y\in \{0,1\}*\}$
The automaton $A=(\{q_0,q_1,q_2\}, \{0, 1\}, \delta, q_0, \{q_1\})$ as a *transition table*:
| $\delta$  |   0   |   1   |
| :-------: | :---: | :---: |
| $\to q_0$ | $q_2$ | $q_0$ |
|  $*q_1$   | $q_1$ | $q_1$ |
|   $q_2$   | $q_2$ | $q_1$ |

The automaton $A$ as a *transition diagram*:
<img src="https://cdn.discordapp.com/attachments/517047603867811899/533436461832208405/unknown.png">

An FA *accepts* a string $w = a_1a_2...a_n$ if there is a path in the transition diagram that
1. Begins at the **start state**
2. Ends at an **accepting state**
3. Has sequence of labels $a_1a_2...a_n$

Example: The FA
<img src="https://cdn.discordapp.com/attachments/517047603867811899/533437042902958092/unknown.png">
Accepts the string "1100101"

- The transition function $\delta$ can be extended to $\hat{\delta}$ that operates on states and strings (as opposed to states and symbols)

**Base:** $\hat{\delta}(a, \epsilon) = q$
**Recusrive Step:** $\hat{\delta}(q, xa) = \delta(\hat{\delta}(q,x), a)$
- Now, formally, the *language* acceepted by $A$ is 
  - $L(A) = \{w~:~\hat{\delta}(q_0, w) \in F\}$
- The languages accepted by FAs are called *regular languages*.

**Example:** DFA accepting all and only strings with and even number of 0s and an even number of 1s. 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/533438585391546378/unknown.png">
Tabular representation of the Automaton
|  $\delta$  |   0   |   1   |
| :--------: | :---: | :---: |
| $*\to q_0$ | $q_2$ | $q_1$ |
|   $q_1$    | $q_3$ | $q_0$ |
|   $q_2$    | $q_0$ | $q_3$ |
|   $q_3$    | $q_1$ | $q_2$ |


**Example:** Graph of a DFA
Accepts all strings without two consecutive 1s.
<img src="https://cdn.discordapp.com/attachments/517047603867811899/533481174732111872/unknown.png">

Alternative representation: Transition Table
<img src="https://cdn.discordapp.com/attachments/517047603867811899/533481540273831953/unknown.png">

Extended Transition Function
- We describe the effect of a string of inputs on a DFA by extended $\delta$ to a state and a string.
- Induction on length of string
- **Basis:** $\hat{\delta}(q, \epsilon) = q$
- **Induction:** $\hat{\delta}(q, wa) = \delta(\hat{\delta}(q,w),a)$
  - $w$ is a string; a is an input symbol.

Extended $\delta$: Intuition
- Convention:
  - ...x, x, y, z are strings
  - a, b, c, ... are single symbols
- Extended $\hat{\delta}$ is computed for state $q$ and inputs $a_1a_2...a_n$ by following a path in the transition graph, starting at $q$ and selecting the arcs with labels $a_1, a_2, ... , a_n$ in turn.

<img src="https://cdn.discordapp.com/attachments/517047603867811899/534162887359594496/unknown.png">

### Language of a DFA
- Automata of all kinds define languages 
- If A is an automaton, L(A) is it's language 
- For a DFA, A, L(A) is the set of strings labelling paths from start state to a final state
- Formally: L(A) = the set of strings $w$ such that $\hat{\delta}(q_0, w)$ is in F. 

