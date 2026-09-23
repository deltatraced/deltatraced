# 1 Journal

2025-09-06 Wk 36 Sat - 18:00

What did we learn last week?

### 1.1.1 Recall

2025-09-06 Wk 36 Sat - 18:00

I will try to recall here without revisiting the definitions or the book.

The [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf) started in page 1 with a definition of [Probability Space](../../../../concepts/2025/000%20Probability%20Space.md).

A probability space is a 3-tuple $(\Omega, \mathcal{F}, P)$ where

* $\Omega$ is a set of outcomes.
* ([check](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-1)) Example: A dice has the set of outcomes $\Omega = \lbrace 1, 2, 3, 4, 5, 6 \rbrace$. ^checked-3-1-1954
* $\mathcal{F}$ is a family of sets including sets up to the power set of outcomes: $\mathcal{F} \subseteq \wp({\Omega})$. This denotes our set of events.
* For our dice,  $\mathcal{F}$ can have at a maximum $2^{6} = 64$ events as per the powerset maximum count.
* This includes individual outcomes as well any combination of unique outcomes.
* ([false](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-2)) If we're modeling a problem about tossing a single dice, we may only want to consider 6 possible events. $\mathcal{F} = \lbrace \lbrace 1 \rbrace, \lbrace 2 \rbrace, \lbrace 3 \rbrace, \lbrace 4 \rbrace, \lbrace 5 \rbrace, \lbrace 6 \rbrace  \rbrace$. ^checked-3-1-2012
* ([false](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-1)) $P: \mathcal{F} \times [0,1] \to \mathbb{R}^{\pm \infty}$ ^checked-3-1-1

What we know about $P$:

* It is a [set function](../../../../../../../../../concept/math/concepts/2025/019%20set%20function.md). This means that it provides a [measure](../../../../../../../../../concept/math/concepts/2025/015%20measure.md) for our events. It satisfies [σ-additivity](../../../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md), which means that for the corresponding set, when you take the measure of the union of two sets, it corresponds to adding the measures of each:

$$
P(\mathcal{F}_1 \cup \mathcal{F}_2) \equiv P(\mathcal{F}_1) + P(\mathcal{F}_2)
$$

This also scales for any randomly selected sequence of elements $A_i \in \mathcal{F}$:

$$
P(\bigcup_i A_i) \equiv \sum_i P(A_i)
$$

([false](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-2)) $(\mathcal{F}, P)$ together form a measure space. ^checked-3-1-2003

For any [set function](../../../../../../../../../concept/math/concepts/2025/019%20set%20function.md) $\mu$,

* $\mu(\varnothing) = 0$
* $\mu$'s range yields non-negative values

**proofs**

(proof 1)

Assume that for any set $A \in \mathcal{F}$, $P(A)$  is some constant $C$. Then,

$$
\begin{aligned}
& P(A) \\
& = C\\
& = P(A \cup \varnothing) \\
& = P(A) + P(\varnothing)\\
& = C + P(\varnothing)\\\end{aligned}
$$

This yields

$$
C = C + P(\varnothing)
$$

Proving that if there is at least one finite measure, $P(\varnothing)$ must be 0.

**Some struggles I've had last week**

(1)

The book used an expression like

$$
\mu(\bigcup_i A_i) = \sum_i \mu(A_i)
$$

for some $A_i \in \mathcal{F}$ to express [σ-additivity](../../../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md).

$\mathcal{F}$ is a set of sets, and I thought $A_i$ here meant it was a particular set of those. But actually, it is short hand for writing

Given some sequence $a_1, a_2, a_3, \dots, a_n \in \mathcal{F}$. So we may sample a sequence of sets from $\mathcal{F}$ and use the shorthand $A_i \in \mathcal{F}$ instead of
$\forall i \in I, a_i \in \mathcal{F}$  where $I$ is $\{1,\dots,n\}$.

**Things I'm unsure of**

(1)

Definition of $P$. I know that we can have a [set function](../../../../../../../../../concept/math/concepts/2025/019%20set%20function.md) $\mu$, but I seem to recall $P$ assigning \[0,1\] to events in $\mathcal{F}$. If this is the case, then the domain of $P$ should be $[0, 1]$ and not $\mathbb{R}^{\pm \infty}$.

$P: \mathcal{F} \times [0, 1] \to \mathbb{R}^+$ would mean that P does not operate on the events directly, but on their probability density, and is a [set function](../../../../../../../../../concept/math/concepts/2025/019%20set%20function.md) over that density $\mathcal{F} \times [0, 1]$.

([check](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-1)) ^checked-3-1-2

(2)

I had the idea that *outcome* and *event* are separate... That our dice events would be $\{1, 2, 3, 4, 5, 6\}$ but I am more confident that $\mathcal{F}$ is a family of sets and those are sets of sets, and not individual items. I am not sure what else outcome would refer to besides just the collection of possible states.

(3)

I'm using $\mathbb{R}^+$ to denote the [extended real number set](../../../../../../../../../concept/math/concepts/2025/017%20RealExt.md). But some other symbol may be more standardly used. But this could also be used to mean the positive real numbers, so it's not good... Let's use $\mathbb{R}^{\pm \infty}$.  ([true](000%20Wk%2036%20Last%20Week%20Recap.md#check-3-1-3)) It might have just been a line on top of $\mathbb{R}$... ^checked-3-1-2256

### 1.1.2 Check

2025-09-06 Wk 36 Sat - 19:25

Ok, let's check!

(check 1)

2025-09-06 Wk 36 Sat - 19:25

The [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf) specifies $P: \mathcal{F} \to [0, 1]$. $P$ simply assigns probabilities for $\mathcal{F}$.

It also specifies that $\mu$ is a probability measure in the case that $\mu(\Omega) = 1$, and that it is usually denoted $P$.

So the definition $P: \mathcal{F} \times [0, 1] \to \mathbb{R}^{\pm \infty}$ is not correct. A value in $[0,1]$ would be the assigned measure.

But it seems correct to say that $P$ *is* a [set function](../../../../../../../../../concept/math/concepts/2025/019%20set%20function.md), its range is just more restricted than $\mathbb{R}^{\pm \infty}$ .

Because $\mu(\Omega) = 1$ for a probability measure, this is supporting evidence that the dice example is correct. By definition of $\mathcal{F}$, its sets cannot include any element outside of $\Omega$, and so there are no other elements to union with it, and so it gives us the highest measure of 1.

Checks:

* [x] [^checked-3-1-1](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-1), (false) definition of $P$
* [x] [^checked-3-1-2](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-2), unsure (1): definition of $P$
* [x] [^checked-3-1-1954](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-1954), Dice example for $\Omega$

^check-3-1-1

(check 2)

2025-09-06 Wk 36 Sat - 19:51

The book specifies in (pg 1, vpg 9/490) that $\mathcal{F}$ is a [σ-algebra](../../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$ and clarifies that this means means that

1. $\mathcal{F}$ is a non-empty collection
1. if $A \in \mathcal{F}$ then $A^c \in \mathcal{F}$.
1. Sample some sequence $A_i \in \mathcal{F}$, then $\bigcup_i A_i \in \mathcal{F}$.

The book specifies that $(\Omega, \mathcal{F})$ form a measure space. This is the space we can *put* a measure on. It's not correct to say it's $(\mathcal{F}, P)$.

2025-09-06 Wk 36 Sat - 20:20

I wrote:

 > 
 > If we're modeling a problem about tossing a single dice, we may only want to consider 6 possible events. $\mathcal{F} = \lbrace \lbrace 1 \rbrace, \lbrace 2 \rbrace, \lbrace 3 \rbrace, \lbrace 4 \rbrace, \lbrace 5 \rbrace, \lbrace 6 \rbrace  \rbrace$.

But does this satisfy the properties of $\mathcal{F}$?

(steps)

(1)

Consider $A = \{1\}$

(2)

$A^c = \{\varnothing, 2, 3, 4, 5, 6\}$ considering our universe of consideration only spans over items of $\Omega + \{\varnothing\}$.

(3)

Is $A^c$ in $\mathcal{F}$?

No.

(4)

So it does not satisfy property 2

2. if $A \in \mathcal{F}$ then $A^c \in \mathcal{F}$.

$\therefore$ This is a false example of $\mathcal{F}$.

(/steps)

A valid (and almost always used) example would be $\mathcal{F} = \wp(\mathcal{F})$ where $\{\varnothing\}^c$ yields $\Omega$.

2025-09-06 Wk 36 Sat - 21:38

Spawn [Drawing 2025-09-06 21.19.50.excalidraw](../../../../drawings/Drawing%202025-09-06%2021.19.50.excalidraw.md)

Spawn [6.3 How do we deal with the empty set in compliments of the sigma algebra?](000%20Wk%2036%20Last%20Week%20Recap.md#63-how-do-we-deal-with-the-empty-set-in-compliments-of-the-sigma-algebra) ^spawn-invst-7b43a3

Checks:

* [x] [^checked-3-1-2003](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-2003), (false) $(\mathcal{F}, P)$  denoting a measure space
* [x] [^checked-3-1-2012](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-2012), (false) example of $\mathcal{F}$

^check-3-1-2

(check 3)

2025-09-06 Wk 36 Sat - 22:55

Yes it's written with a line above: $\overline{\mathbb{R}}$

Checks:

* [x] [^checked-3-1-2256](000%20Wk%2036%20Last%20Week%20Recap.md#checked-3-1-2256), on notation of extended reals having a line on top

^check-3-1-3
