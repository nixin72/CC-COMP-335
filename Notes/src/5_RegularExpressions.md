### Regular Expressions 
A FA (NFA or DFA) is a "blueprint" for constructing a machine to recognize a regular language. 

A *regular expression* is a "user-friendly", declarative way of describing a language. 
**Example:** `01*+10*`
Regular expressions are used in e.g
1. UNIX `grep` command
2. UNIX Lex (Lexical analyzer generator) and Flex (Fast Lex)

#### Operations on languages
*Union*: $L\cup M = \{w:w\in L \lor w\in M\}$

*Concatenation:* $L.M = \{w:w=xy,x\in L,y\in M\}$

*Powers:* $L^0=\{\epsilon\}, L^1=L,L^{j+1}=L.L^k$

*Kleen Closure:* $L^*=\cup_{i=0}^\infty L^i$

**Question:** What are $\emptyset^0, \emptyset^i,$ and $\emptyset^*$

#### Building Regex's
Inductive definition of regex's:

**Basis:** $\epsilon$ is a regex and $\emptyset$ is a regex. 
$L(\epsilon)=\{\epsilon\}$, and $L(\emptyset)=\emptyset$

If $a\in\Sigma$, then $a$ is a regex. 
$L(a)=\{a\}$

**Induction:**
If $E$ is a regex's, then $(E)$ is a regex. 
$L((E)) = L(E)$

If $E$ and $F$ are regex's, then $E+F$ is a regex. 
$L(E+F)=L(E)\cup L(F)$

If $E$ and $F$ are regex's, then $E.F$ is a regex. 
$L(E.F)=L(E).L(F)$

If $E$ is a regex's, then $E^*$ is a regex. 
$L(E^*)=(L(E))^*$

**Example:** Regex for
$L=\{w\in\{0,1\}^*:0$ and $1$ alternate in $w\}$

$(01)^*+(10)^*+0(10)^*+1(01)^*$
or, equivalently,
$(\epsilon+1)(01)^*(\epsilon+0)$

Order of precedence for operators:
1. Star
2. Dot
3. Plus

**Example:** $01^*+1$ is grouped $(0(1)^*)+1$

#### Equivalence of FA's and regex's 
We have already shown that DFA's, NFA's and $\epsilon$-NFA's are all equivalent. 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537847847387660313/unknown.png">
To show FA's equivalent to regex's we need to establish that
1. For every DFA $A$ we can find (construct in this case) a regex $R$, such that $L(R)=L(A)$
2. For every regex $R$ there is a $\epsilon$-NFA $A$ such that $L(A)=L(R)$

#### The state elimination technique
Let's label the edges with regex's instead of symbols 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537849289431318538/unknown.png">

Now let's eliminate state $s$:

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537849463797055509/unknown.png">

For each accpeting state $q$ eliminate from the original automaton all states except $q_0$ and $q$.

For each $q\in F$ we'll be left with an $A_q$ that looks like: 

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537850068758298645/unknown.png">

that corresponds to the regex $E_q=(R+SU^*T)^*SU^*$
or with $A_q$ looking like 

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537850433922662421/unknown.png">

Corresponding to the regex $E_q=R^*$
- The final expression is 
  - <img src="https://cdn.discordapp.com/attachments/517047603867811899/537853099335876619/unknown.png">

**Example:** $A$, where $L(A)=\{W:w=x1b,$ or $w=x1bc, x\in\{0,1\}^*,\{b,c\}\subseteq\{0,1\}\}$

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537853693622747139/unknown.png">

We turn this into an automaton with regex labels

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537853952772014080/unknown.png">

Let's eliminate state $B$

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537854158498299925/unknown.png">

Then we eliminate state $C$ and obtain $A_D$

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537854365847912448/unknown.png">

with regex $(0+1)^*1(0+1)(0+1)$

From 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537854916618878992/unknown.png">

We can eliminate $D$ to obtain $A_C$

<img src="https://cdn.discordapp.com/attachments/517047603867811899/537855057602019338/unknown.png">

With regex $(0+1)^*(0+1)$

- The final expression is the sum of the previous two regex's:
  $(0+1)^*1(0+1)(0+1)+(0+1)^*1(0+1)$