# 1 Definition

A probability space is a 3-tuple $(\Omega, \mathcal{F}, P)$ where

* $\Omega$ is the set of outcomes
* $\mathcal{F}$ is set of events.
* It is also a [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$.
* $P:\mathcal{F} \to [0,1]$ is a function assigning probabilities to events in $\mathcal{F}$

Being a σ-algebra over $\Omega$, $\mathcal{F}$ includes all non-empty [subsets](../../../../../../../concept/math/concepts/2025/005%20subset.md) of $\Omega$ and satisfies the conditions:

1. if $A \in \mathcal{F}$, then $A^c \in \mathcal{F}$
1. if $\forall i \in I, A_i \in \mathcal{F}$, then $\bigcup_{i \in \mathbb{I}} A_i \in \mathcal{F}$ where $I$ is $[1..|A_i|]$ and $|A_i|$ is the size of the sequence $A_i$.

# 2 Consequences

### 2.1.1 Closure under intersection

As a consequence of closure under union, the condition $\bigcap_{i \in \mathbb{I}} A_i \in \mathcal{F}$ can be rewritten as as a union $(\bigcup_{i \in I} A_i^c )^c \in \mathcal{F}$

This is by application of [De Morgan's laws](https://en.wikipedia.org/wiki/De_Morgan%27s_laws) which is also mentioned in wiki for [compliment](../../../../../../../concept/math/concepts/2025/006%20compliment.md).

# 3 Theorems

## 3.1 Theorem 1

This matches book theorem 1.1.1 on pg 2, vpg 10/490.

(1)

Let $\mu$ be a [measure](../../../../../../../concept/math/concepts/2025/015%20measure.md) on $(\Omega, \mathcal{F})$

(2)

$\mu$ satisfies monotonicity, if $A \subset B$ then $\mu(A) \le \mu(B)$

(3)

$\mu$ satisfies subadditivity, if $A \subset \bigcup_{m=1}^\infty A_m$ then $\mu(A) \le \sum_{m=1}^\infty \mu(A_m)$

(4)

$A_i \uparrow A$ denotes continuity from above, and translates to $A_1 \subset A_2 \subset A_3 \subset \dots \subset A_i$ and $\bigcup_i A_i = A$

(5)

$A_i \downarrow A$ denotes continuity from below, and translates to $A1 \supset A_2 \supset A_3 \supset \dots \supset A_i$  and $\bigcap_i A_i = A$

(6)

if $A_i \uparrow A$,

# 4 Related Entries

* [000 Starting out Probability Theory and Examples](../../entries/2025/000%20Starting%20out%20Probability%20Theory%20and%20Examples.md)

# 5 Notes

## 5.1 On Sequence Notation

The notation $A_i \in \mathcal{F}$  used in the book is shorthand for $\forall i \in I, A_i \in \mathcal{F}$ , where $I$ is the length of the sequence in consideration.

## 5.2 Wiki naming scheme

In [wiki Probability space](https://en.wikipedia.org/wiki/Probability_space),

they call $\Omega$ the *sample space* which is a set of outcomes, and $\mathcal{F}$ the *event space*.

Their explanation that we can think of an event under $\mathcal{F}$ as a query which may yield a zero-or-more subset of outcomes in $\Omega$. For example, "die landing on 6" is an event $\{6\}$, and "die landing on a number less than 4" is an event $\{1, 2, 3\}$.

# 6 Resources

# 7 Config

$$

\\newcommand{\arw}{\rightarrow}

$$
