Theorem: $L(A) = L$
Proof: We show that $L(A) \subseteq L$ by showing with induction on $|w|$ that $w \in L(A) \Rightarrow w \in L$.
Actually, we show something stronger, namely
1. If $\hat{\delta}(p, w) = p$ then $w$ doesn't contain 11 and doesn't end in 1. 
2. If $\hat{\delta}(p, w) = q$ then $w$ doesn't contain 11 and ends in a single 1.

**Base:** $|w| = 0 \Rightarrow w = \epsilon \Rightarrow \hat{\delta}(p, w) = p$
