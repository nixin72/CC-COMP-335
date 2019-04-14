kiababakashi

- Study induction
- Study mathematical recursion

Logicians 
Functional programming 
Software 

$a^R = a$
$(w,~a)^R = a~w^R$
$(hell,~o)^R = o(hell)^R = ol(hel)^R = oll(he)^R = olle(h)^R = olleh$

Prove that $(u^R)^R = u$
$u = w~a$
Proof by induction on the length of $u$
Base case: $u = \text{"a"}$
$|u| = 1$
$(a^R)^R = a^R = a$

$|u| = n$
IH: $((u^R)^R) = u$

IS: Let $u = w~a$ such that $|w| = n$
$((w,~a)^R)^R = (a,~w^R))^R$

let w = $w_1~w_2~...~w_n$
So $w^R = w_n~...~w_1$
$((a, w_n...w_2) w_1)^R = (T, w_1)^R = w_1 T^R = w_1,w_2,...,w_n~a$


Question:
Let L = {ab, aa, baa}, $\Sigma$ = {a, b}
Which of the following are in L* and which are in L4
If it is in L4, it is also in L*

Lambda is the empty string 

- $S_1 = abaabaaabaa$
  - $ab + aa + baa + ab + aa$
  - 5 long, so it belongs to L5 and L*
- $S_2 = aaaabaaaa$
  - $aa + aa + baa + aa$
  - 4 long, so it belongs to L4 and L*
- $S_3 = baaaaabaaaab$
  - $baa + aa + ab + aa + aa + b$
  - Belongs to the alphabet, but *not* to the language. Not in L* 
- $S_4 = baaaaabaa$
  - $baa + aa + ab + aa$
  - 4 long, so it belongs to L4 and L* 

Total and Partial functions 
```
| a |---->| 1 |
| b |---->| 2 |
| c |---->| 3 |
DFA
```

```
| a |---->| 1 |
| b |---->| 2 |
| c |     | 3 |
Partial NFA
```
A DFA is defined a tuple 
- Set of states, 
- Alphabet
- Transition functions 
- Start state
- End state(s)
DFA = $(Q, \Sigma, \delta, q_0, F)$
$\delta$ is a transition function. 
$\delta = Q \times \Sigma \to Q$
$|Q| = m$
$|\Sigma| = n$

A DFA validates whether a string belongs to a language or not. It's a yes or no problem

$\Sigma = \{a,b\}$ , give DFAs for the following languages
- L_1 = {w|w ends with the string ab}
    ```haskell
            ^b
        ^a  <-
    -> |q0| -> |q1| -> ||q2||
             a       b
    ```

$\Sigma = \{a,~b\}$
$L_2 = \{a^n b^m | n+m \text{ is even}\}$
$n \in O \land m\in O$
OR 
$n\in E \land m\in E$
n/2 = k
2|n