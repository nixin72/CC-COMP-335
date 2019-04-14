## Nondeterministic Finite Automata
An NFA can be several states at once, or, viewed anpother way, it can "guesS" which state to go to next. 

**Example:** An automaton that accepts all and only string ending in 01.
<img src="https://cdn.discordapp.com/attachments/517047603867811899/534779005946298388/unknown.png">

Here is what happens when the NFA processes the input 00101:
<img src="https://cdn.discordapp.com/attachments/517047603867811899/534779172346789893/unknown.png">

Formally, an NFA is a quintuple
$A = (Q, \Sigma, \delta, q_0, F)$
- $Q$ is a finite set of symbols 
- $\Sigma$ is a finite alphabet
- $\delta$ is a transition function from $Q x \Sigma$ to the *powerset* of $Q$
- $q_0 \in Q$ is the *start state*
- $F \subseteq Q$ is the set of *final states*

**Example:** The NFA from slide 1 is: $(\{q_0,q_1,q_2\},\{0,1\}, \delta, q_0, \{q_2\})$
where $\delta$ is the transition function
| $\delta$  |       0        |     1     |
| :-------: | :------------: | :-------: |
| $\to q_0$ | $\{q_0, q_1\}$ | $\{q_0\}$ |
|   $q_1$   |     $\phi$     | $\{q_2\}$ |
|  $*q_2$   |     $\phi$     |  $\phi$   |

Extended transition function $\hat{\delta}$
**Basis:** $\hat\delta(q,\epsilon) = \{q\}$

**Induction:** $\displaystyle\hat\delta(q,xa)={\cup\atop{p\in\hat\delta(q,x)}}\delta(p,a)$
**Example:** Let's compute $\hat\delta(q_0, 00101)$ on the blackboard. 

- Now, formally, the *language accepted* by $A$ is $L(A)=\{w:\hat\delta(q_0,w)\cap F\neq\emptyset\}$
Let's prove formally that the NFA $A$
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537319356103917568/unknown.png">
accepts the language $L=\{x01:x\in\Sigma^*\}$

To show that $L(A) \subseteq L$ we do a mutual induction on the three statements below:
0. $w\in\Sigma^*\Rightarrow q_0\in\hat\delta(q_0,w)$
1. $q_1\in\hat\delta(q_0,w)\iff w=x0$
2. $q_2\in\hat\delta(q_0,w)\iff w=x01$

**Basis:** if $|w| =0$ then $w=\epsilon$. Then statement (0) follows from def. For (1) and (2) both sides are false for $\epsilon$
**Inductive hypothesis:** Assume statements (0)-(2) when $|w|=k$, for some $k \geq 0$.

**Inductive step:** Let $|w|=k+1$. Then $w=xa$, where $a\in\{0,1\}, |x|=n$ and statements (0)-(2) hold for $x$. We now show that statements (0)-(2) hold for $xa$.

**Statement (0):**
IH(0) $\Rightarrow q_0\in\hat\delta(q_0,x),a\in\{0,1\}$
$\Rightarrow q_0\in\hat\delta(q_0,a)\Rightarrow q_0\in\hat\delta(q_0,xa)$

**Statement (1) <= direction:**
$w=x0\text{ IH}(0)\Rightarrow q_0\in\hat\delta(q_0,x)$
$\Rightarrow q_1\in\hat\delta(q_0,x0)$

**Statement (1) => direction:**
$q_1\in\hat\delta(q_0,xa)\Rightarrow q_1\in\delta(\hat\delta(q_0,x),a)$
This is possible only if $a=0$ and $q_0\in\hat\delta(q_0,x)$
$\text{IH}(0)\Rightarrow q_0\in\hat\delta(q_0,x)$

**Statement (2) <= direction:**
Let $w=x01\text{ IH}(1,"\Leftarrow")\Rightarrow q_1\in\hat\delta(q_0,x0)\Rightarrow\hat\delta(q_0,x01)=\delta(\hat\delta(q_0,x0),1)\ni\delta(q_1,1)\ni q_2$

**Statement (3) => direction:**
Let $w=xa,a\in\{0,1\}\text{ and }q_2\in\hat\delta(q_0,xa\\\Rightarrow$
$q_2\in\delta(\hat\delta(q_0,x),a)\Rightarrow a=1\text{ and }q_1\in\hat\delta(q_0,x)\\\text{ IH}(1, "\Rightarrow")\\x=y0\Rightarrow w=y01.$

To show that $L \subseteq L(A)$ we show the contrapositive
$w\notin L(A)\Rightarrow w\notin L$
Suppose $w\notin L(A)$
$\Rightarrow\hat\delta(q_0,w)\subseteq\{q_0,q_1\}$

From first part, point 2, we get

$q_2\notin\hat\delta(q_0,w)\Rightarrow w\neq x01\Rightarrow w\notin L$

### Equivalence of DFA and NFA
- NFAs are usually easier to "program" in. 
- Surprisingly, for any NFA $N$ there exists a DFA $D$, such that $L(D)=L(N)$, and vice versa. 
- This involves the subset construction, an important example how an automaton $B$ can be generically constructed from another automaton $A$. 
- Given an NFA:
  - $N=(Q_n,\Sigma,\delta_N,q_0,F_N)$
- We will construct a DFA
  - $D=(Q_D,\Sigma,\delta_D,\{q_0\},F_D)$
- Such that
  - $L(D)=L(N)$
- The details of the subset construction:
  - $Q_D=\{S:S\subseteq Q_N\}$

Note: $|Q_D|=2^{|Q_N|}$, although most states in $Q_D$ are likely to be garbage. 

- $F_D = \{S\subseteq Q_N:S\cap F_N \neq\emptyset\}$
- For every $S\subseteq Q_N \text{ and } a\in\Sigma$,
  - $\delta_D(S,a)={\cup\atop{p\in S}}\delta_N(p,a)$

Let's construct $\delta_D$ from the NFA on slide 1.
|                    | 0              | 1             |
| -----------------: | :------------- | :------------ |
|        $\emptyset$ | $\emptyset$    | $\emptyset$   |
|      $\to \{q_0\}$ | $\{q_0, q_1\}$ | $\{q_0\}$     |
|          $\{q_1\}$ | $\emptyset$    | $\{q_2\}$     |
|         $*\{q_2\}$ | $\emptyset$    | $\emptyset$   |
|      $\{q_0,q_1\}$ | $\{q_0,q_1\}$  | $\{q_0,q_2\}$ |
|     $*\{q_0,q_1\}$ | $\{q_0,q_1\}$  | $\{q_0\}$     |
|     $*\{q_1,q_2\}$ | $\emptyset$    | $\{q_2\}$     |
| $*\{q_0,q_1,q_2\}$ | $\{q_0,q_1\}$  | $\{q_0,q_2\}$ |

Note: The states of $D$ correspond to subsets of states of $N$, but we could have denoted the states of $D$ by say, $A-F$ just as well. 

|         | 0    | 1    |
| ------: | :--- | :--- |
|     $A$ | $A$  | $A$  |
| $\to B$ | $E$  | $B$  |
|     $C$ | $A$  | $D$  |
|    $*D$ | $A$  | $A$  |
|     $E$ | $E$  | $F$  |
|    $*F$ | $E$  | $B$  |
|    $*G$ | $A$  | $D$  |
|    $*H$ | $E$  | $F$  |

We can often avoid the exponential blow-up by constructing the transition table for $D$ only for accessible states $S$ as follows:

**Basis:** $S=\{q_0\}$ is accessible in $D$
**Induction:** If state $S$ is accessible, so are the states in $\displaystyle\cup_{a\in\Sigma^{\delta}D^{(S,a)}}$

Example: The "subset" DFA with accessible states only.
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537332395435753512/unknown.png">

**Theorem 2.11:** Let $D$ be the "subset" DFA of an NFA $N$. Then $L(D) = L(N)$

**Proof:** We show on the blackboard in class by an induction on $|w|$ that
$\hat\delta_D(\{q_0\},w)=\hat\delta_N(q_0,w)$
The claim then follows. **Why?**

**Theorem 2.12:** A language $L$ is accepted by some DFA iff $L$ is accepted by some NFA. 

**Proof:** The "if" part is theorem 2.11.
For the "only if" part we note that any DFA can be converted to an equivalent NFA by modifying the $\delta_D$ to $\delta_N$ by the rule
- If $\delta_D(q,a)=p$, then $\hat\delta_N(q_0,w)=\{p\}$

By induction on $|w|$ it will be shown on blackboard that if 
$~~~~\hat\delta_D(q_0,w)=p$, then $\hat\delta_N(q_0,w)=\{p\}$
The claim of the theorem follows. 

---

### Exponential blow up

- There is an NFA $N$ with $n+1$ states that has no equivalent DFA with fewer than $2^n$ states. 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537334258432213004/unknown.png">
$L(N)=\{x1c_2c_3...c_n:x\in\{0,1\}^*, c_i\in\{0,1\}\}$
suppose an equivalent DFA D with fewer than $2^n$ states exists. 

$D$ must remember the last $n$ symbols it has read. There are $2^n$ bitsequences $a_1a_2...a_n$. Since $D$ has fewer than $2^n$ states

$\exists q, a_1a_2...a_n, b_1b_2...b_2:$
$~~~~~~a_1a_2...a_n\neq b_1b_2...b_n$
$~~~~~~\hat\delta(q_0,a_1a_2...a_n)=\hat\delta_D(q_0,b_1b_2...b_n)=q$

Since $a_1a_2...a_n\neq b_1b_2...b_n$ they must differ in at least one position.

**Case 1:**
$1a_n...a_n$
$0b_2...b_n$

Then $q$ has to be both an accepting and a nonaccepting state. 

**Case 2:**
$a_1...a_{i-1}1a_{a+1...a_n}$
$b_1...b_{i-1}0b_{i+i}..._{b_n}$

Now $\hat\delta_D(q_0,a_1...a_{i-1}1a_{a+1...}a_n0^{i-1}) =$
$~~~~~~~\hat\delta_D(q_0,b_1...b_{i-1}0b_{i+i}..._{b_n}0^{i-1})$

And $\hat\delta_D(q_0,a_1...a_{i-1}1a_{a+1...}a_n0^{i-1})\in F_D$
$~~~~~~~\hat\delta_D(q_0,b_1...b_{i-1}0b_{i+i}..._{b_n}0^{i-1})\in F_D$

---

### FA's with Epsilon-Transitions
An $\epsilon$-NFA accepting decimal numbers consisting of:
1. An optional + or - sign
2. A string of digits
3. A decimal point
4. Another string of digits

One of the two strings (2) and (4) are optional. 
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537337877017788416/unknown.png">
An $\epsilon$-NFA is a quintuple $(Q,\Sigma,\delta,q_0,F)$ where $\delta$ is a function from $Q \times \Sigma\cup\{\epsilon\}$ to the powerset of $Q$.

**Example:** The $\epsilon$-NFA from the previous image. 

$E=(\{q_0,q_1,...,q_5\},\{.,+,-,0,1,...,9\},\delta,q_0,\{q_5\})$
Where the transition table for $\delta$ is 

|           | $\epsilon$  | $+,-$       | $.$         | $0,...,9$     |
| --------: | :---------- | :---------- | :---------- | :------------ |
| $\to q_0$ | $\{q_1\}$   | $\{q_1\}$   | $\emptyset$ | $\emptyset$   |
|     $q_1$ | $\emptyset$ | $\emptyset$ | $\{q_2\}$   | $\{q_1,q_4\}$ |
|     $q_2$ | $\emptyset$ | $\emptyset$ | $\emptyset$ | $\{q_3\}$     |
|     $q_3$ | $\{q_5\}$   | $\emptyset$ | $\emptyset$ | $\{q_3\}$     |
|    $*q_5$ | $\emptyset$ | $\emptyset$ | $\emptyset$ | $\emptyset$   |

--- 

### ECLOSE

We close a state by adding all states reachable by a sequence $\epsilon\epsilon...\epsilon$

Inductive definition of ECLOSE($q$)

**Basis:** $q\in$ ECLOSE($q$)

**Induction:**
$p\in$ ECLOSE($q$) and $r\in\delta(p,\epsilon)\Rightarrow$
$r\in$ ECLOSE($q$)

Example of $\epsilon$-closure
<img src="https://cdn.discordapp.com/attachments/517047603867811899/537340487141556225/unknown.png">

For instance, 

$\text{ECLOSE}(1)= \{1,2,3,4,5,6\}$

- Inductive definition of $\hat\delta$ for $\epsilon$-NFA's

**Basis:**
$\hat\delta(q,\epsilon)=\text{ ECLOSE}(q)$

**Induction:**

$\hat\delta(q,xa)= {\cup\atop{p\in\hat\delta(q,x)}}\text{ ECLOSE}(\delta(p,a))$

Let's compute on the blackboard in class $\hat\delta(q_0,5.6)$ for the NFA.