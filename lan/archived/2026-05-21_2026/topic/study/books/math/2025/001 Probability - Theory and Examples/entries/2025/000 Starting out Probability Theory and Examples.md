# 1 Objective

Go through the first chapter.

# 2 Journal

2025-08-30 Wk 35 Sat - 08:42

Measure theory has a hidden section Appendix A at the end of the book in pg 455 (vpg 463/490)

2025-08-30 Wk 35 Sat - 08:47

How do you write "(Ω, F, P )"

2025-08-30 Wk 35 Sat - 14:11

So I found that $\mathcal{F}$ is notation for family of sets! You can learn more about it in this wiki [wiki Set function](https://en.wikipedia.org/wiki/Set_function) which talks about the same notation also with $\Omega$!

(update)
So to say that $\mathcal{F}$ is a family of sets over $\Omega$ is to say that $\mathcal{F} \subseteq \wp(\Omega)$ where $\wp(\Omega)$ is the powerset of $\Omega$.

2025-09-06 Wk 36 Sat - 22:46

In [wiki Probability space](https://en.wikipedia.org/wiki/Probability_space), they mention that $\mathcal{F}$ is almost always $2^\Omega$ which they use as the notation for the powerset, instead of $\wp(\Omega)$. But they mention that if $\Omega$ is uncountable, then $2^{\Omega}$ would be "too large". So this explains $\mathcal{F} \subseteq 2^{\Omega}$.  
(/update)

2025-08-30 Wk 35 Sat - 08:45

Putting probability space definition in [000 Probability Space](../../concepts/2025/000%20Probability%20Space.md)

2025-09-06 Wk 36 Sat - 22:53

Last we reached is defining the properties continuity from below and continuity from above for [000 Probability Space](../../concepts/2025/000%20Probability%20Space.md)

# 3 Tasks

## 3.1 Wk 36 Last Week Recap

2025-09-06 Wk 36 Sat - 18:00

What did we learn last week?

### 3.1.1 Recall

2025-09-06 Wk 36 Sat - 18:00

I will try to recall here without revisiting the definitions or the book.

The [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf) started in page 1 with a definition of [Probability Space](../../concepts/2025/000%20Probability%20Space.md).

A probability space is a 3-tuple $(\Omega, \mathcal{F}, P)$ where

* $\Omega$ is a set of outcomes.
* ([check](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-1)) Example: A dice has the set of outcomes $\Omega = \lbrace 1, 2, 3, 4, 5, 6 \rbrace$. ^checked-3-1-1954
* $\mathcal{F}$ is a family of sets including sets up to the power set of outcomes: $\mathcal{F} \subseteq \wp({\Omega})$. This denotes our set of events.
* For our dice,  $\mathcal{F}$ can have at a maximum $2^{6} = 64$ events as per the powerset maximum count.
* This includes individual outcomes as well any combination of unique outcomes.
* ([false](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-2)) If we're modeling a problem about tossing a single dice, we may only want to consider 6 possible events. $\mathcal{F} = \lbrace \lbrace 1 \rbrace, \lbrace 2 \rbrace, \lbrace 3 \rbrace, \lbrace 4 \rbrace, \lbrace 5 \rbrace, \lbrace 6 \rbrace  \rbrace$. ^checked-3-1-2012
* ([false](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-1)) $P: \mathcal{F} \times [0,1] \to \mathbb{R}^{\pm \infty}$ ^checked-3-1-1

What we know about $P$:

* It is a [set function](../../../../../../../concept/math/concepts/2025/019%20set%20function.md). This means that it provides a [measure](../../../../../../../concept/math/concepts/2025/015%20measure.md) for our events. It satisfies [σ-additivity](../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md), which means that for the corresponding set, when you take the measure of the union of two sets, it corresponds to adding the measures of each:

$$
P(\mathcal{F}_1 \cup \mathcal{F}_2) \equiv P(\mathcal{F}_1) + P(\mathcal{F}_2)
$$

This also scales for any randomly selected sequence of elements $A_i \in \mathcal{F}$:

$$
P(\bigcup_i A_i) \equiv \sum_i P(A_i)
$$

([false](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-2)) $(\mathcal{F}, P)$ together form a measure space. ^checked-3-1-2003

For any [set function](../../../../../../../concept/math/concepts/2025/019%20set%20function.md) $\mu$,

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

for some $A_i \in \mathcal{F}$ to express [σ-additivity](../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md).

$\mathcal{F}$ is a set of sets, and I thought $A_i$ here meant it was a particular set of those. But actually, it is short hand for writing

Given some sequence $a_1, a_2, a_3, \dots, a_n \in \mathcal{F}$. So we may sample a sequence of sets from $\mathcal{F}$ and use the shorthand $A_i \in \mathcal{F}$ instead of
$\forall i \in I, a_i \in \mathcal{F}$  where $I$ is $\{1,\dots,n\}$.

**Things I'm unsure of**

(1)

Definition of $P$. I know that we can have a [set function](../../../../../../../concept/math/concepts/2025/019%20set%20function.md) $\mu$, but I seem to recall $P$ assigning \[0,1\] to events in $\mathcal{F}$. If this is the case, then the domain of $P$ should be $[0, 1]$ and not $\mathbb{R}^{\pm \infty}$.

$P: \mathcal{F} \times [0, 1] \to \mathbb{R}^+$ would mean that P does not operate on the events directly, but on their probability density, and is a [set function](../../../../../../../concept/math/concepts/2025/019%20set%20function.md) over that density $\mathcal{F} \times [0, 1]$.

([check](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-1)) ^checked-3-1-2

(2)

I had the idea that *outcome* and *event* are separate... That our dice events would be $\{1, 2, 3, 4, 5, 6\}$ but I am more confident that $\mathcal{F}$ is a family of sets and those are sets of sets, and not individual items. I am not sure what else outcome would refer to besides just the collection of possible states.

(3)

I'm using $\mathbb{R}^+$ to denote the [extended real number set](../../../../../../../concept/math/concepts/2025/017%20RealExt.md). But some other symbol may be more standardly used. But this could also be used to mean the positive real numbers, so it's not good... Let's use $\mathbb{R}^{\pm \infty}$.  ([true](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#check-3-1-3)) It might have just been a line on top of $\mathbb{R}$... ^checked-3-1-2256

### 3.1.2 Check

2025-09-06 Wk 36 Sat - 19:25

Ok, let's check!

(check 1)

2025-09-06 Wk 36 Sat - 19:25

The [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf) specifies $P: \mathcal{F} \to [0, 1]$. $P$ simply assigns probabilities for $\mathcal{F}$.

It also specifies that $\mu$ is a probability measure in the case that $\mu(\Omega) = 1$, and that it is usually denoted $P$.

So the definition $P: \mathcal{F} \times [0, 1] \to \mathbb{R}^{\pm \infty}$ is not correct. A value in $[0,1]$ would be the assigned measure.

But it seems correct to say that $P$ *is* a [set function](../../../../../../../concept/math/concepts/2025/019%20set%20function.md), its range is just more restricted than $\mathbb{R}^{\pm \infty}$ .

Because $\mu(\Omega) = 1$ for a probability measure, this is supporting evidence that the dice example is correct. By definition of $\mathcal{F}$, its sets cannot include any element outside of $\Omega$, and so there are no other elements to union with it, and so it gives us the highest measure of 1.

Checks:

* [x] [^checked-3-1-1](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-1), (false) definition of $P$
* [x] [^checked-3-1-2](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-2), unsure (1): definition of $P$
* [x] [^checked-3-1-1954](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-1954), Dice example for $\Omega$

^check-3-1-1

(check 2)

2025-09-06 Wk 36 Sat - 19:51

The book specifies in (pg 1, vpg 9/490) that $\mathcal{F}$ is a [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$ and clarifies that this means means that

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

Spawn [Drawing 2025-09-06 21.19.50.excalidraw](../../drawings/Drawing%202025-09-06%2021.19.50.excalidraw.md)

Spawn [6.3 How do we deal with the empty set in compliments of the sigma algebra?](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#63-how-do-we-deal-with-the-empty-set-in-compliments-of-the-sigma-algebra) ^spawn-invst-7b43a3

Checks:

* [x] [^checked-3-1-2003](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-2003), (false) $(\mathcal{F}, P)$  denoting a measure space
* [x] [^checked-3-1-2012](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-2012), (false) example of $\mathcal{F}$

^check-3-1-2

(check 3)

2025-09-06 Wk 36 Sat - 22:55

Yes it's written with a line above: $\overline{\mathbb{R}}$

Checks:

* [x] [^checked-3-1-2256](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#checked-3-1-2256), on notation of extended reals having a line on top

^check-3-1-3

## 3.2 Writing Continuity from above and below theorems for probability space

* [ ] 

2025-09-07 Wk 36 Sun - 03:42

So in page 2 they're trying to write a proof for continuity from above and from below.

We investigated the meaning of the notation in [6.4 Union continuity operations applied to set functions](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#64-union-continuity-operations-applied-to-set-functions).

2025-09-07 Wk 36 Sun - 03:46

So this is our interpretation:

* $\mu(A_i) \uparrow \mu(A)$ means the value of $\mu(A_i)$ approaches $\mu(A)$ from the left.
  -$\mu(A_i) \downarrow \mu(A)$ means the value of $\mu(A_i)$ approaches $\mu(A)$ from the right (edited)

Let's write and attempt to explain the proof.

(steps)

(1)

Let two sets $A$ and $B$ be subsets of $\Omega$.

Define the difference of two sets $A$ and $B$ as $B - A \defeq B \cap A^c$.

So we know that $B \cap A^c$ will not contain anything $A$ since it must be the part of $B$ that intersects $A^c$.

(2)

Use $+$ to denote disjoint union. Then,

We can rewrite $B$ as $B = A + (B - A)$. Then,

$$
\mu(B) = \mu(A) + \mu(B - A)
$$

By finding the measure for all terms in the equation.

(3)

$$
\mu(B) = \mu(A) + \mu(B - A) \ge \mu(A)
$$

Given

1. A and B are non-empty subsets of $\Omega$. This means $\mu(A)$ and $\mu(B)$ are not $0$.
1. There exists a probability measure $P$ that assigns a value in $[0, 1]$ for all subsets of $\Omega$.
1. So $\mu(B - A)$ is not equal to values in $\{\infty, -\infty\}$.
   $\therefore$  $\mu(A) + C \ge \mu(A)$ follows since $C = \mu(B - A)$ is a finite non-negative constant.

(4)

Let

$$
A_n' = A_n \cap A,
$$

$$
B_n =
\begin{cases}
A_1' & n = 1 \\
A_n' - \bigcup_{m=1}^{n-1} A_m' & \text{else}
\end{cases}
$$

Pending...

(/steps)

### 3.2.1 References

1. [stackexchange answer](https://tex.stackexchange.com/questions/365953/how-do-i-define-a-piecewise-function-in-latex) for piecewise definitions in LaTeX.

### 3.2.2 Pend

# 4 Issues

# 5 HowTos

## 5.1 Translating some symbols to LaTeX

2025-08-30 Wk 35 Sat - 08:47

How do you write this in LaTeX?

![Pasted image 20250830084751.png](../../../../../../../../../../../attachments/Pasted%20image%2020250830084751.png)

I know the omega is just `\Omega`: $\Omega$.

P just looks like a regular P, $P$.

But what about the F? that's not $F$.

# 6 Investigations

## 6.1 What does pairwise mean in pairwise disjoint sets?

* [x] 

2025-08-30 Wk 35 Sat - 15:18

I know disjoin sets share no elements, but pairwise? The context is the description of sigma additivity in the [wiki](https://en.wikipedia.org/wiki/Sigma-additive_set_function):

 > 
 > Suppose that A ![{\displaystyle \scriptstyle {\mathcal {A}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d586ece9308bf4ef901494079b434c71aac7d41) is a [σ-algebra](https://en.wikipedia.org/wiki/Sigma_algebra "Sigma algebra"). If for every [sequence](https://en.wikipedia.org/wiki/Sequence "Sequence") A 1 , A 2 , … , A n , … ![{\displaystyle A\_{1},A\_{2},\ldots ,A\_{n},\ldots }](https://wikimedia.org/api/rest_v1/media/math/render/svg/7a121200e0c558612beb99e748a738814d788c3f) of pairwise disjoint sets in A ,

[statisticshowto pairwise-disjoint](https://www.statisticshowto.com/pairwise-disjoint/) talks about this, but they keep mentioning "pairwise disjoint" together, without decomposing them into individual concepts!

But in [What is Pairwise?](https://www.statisticshowto.com/pairwise-independent-mutually/#PW) they explain:

 > 
 > Pairwise means to **form all possible pairs** — two items at a time — from a set. For example, in the set {1,2,3} all possible pairs are (1,2), (2,3), (1,3).

Okay, that's intuitive.

So I think pairwise + disjoint then would mean, if you take all possible pairs from a family of sets, and then we expect that all pairs of sets are disjoint.

## 6.2 Why F contains all unions of its underlying elements

* [x] 

So in pg1 (vpg 9/490) of the [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf),

They say

 > 
 > if $A_i \in \mathcal{F}$ is a countable sequence of sets then $\bigcup_iA_i \in \mathcal{F}$

Why would this be true?

2025-08-30 Wk 35 Sat - 17:54

(1)

$\mathcal{F}$ is a [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can hold elements up to $\wp(\Omega)$ where $\wp$ denotes the powerset.

^errata-e14827

2025-08-30 Wk 35 Sat - 18:22

Spawn [Drawing 2025-08-30 18.25.14.excalidraw](../../drawings/Drawing%202025-08-30%2018.25.14.excalidraw.md) (sketch)

$\mathcal{F}$'s elements are possible *subsets* of $\Omega$, yet here the book says that an element $A_i$ is a countable *sequence*?

An element $A$ of $\mathcal{F}$ should be a subset of $\Omega$. An element $A_i$ of $A$ should be considered an individual item.

2025-08-30 Wk 35 Sat - 18:58

In [σ-additive defn premise 3](../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md#199261-premise-3),

We also mention sequences, and this might be where this comes from.

 > 
 > Let $S$ be the set of all [sequences](../../../../../../../concept/math/concepts/2025/020%20Seq.md) of pairwise disjoint \[^1\] sets in $\mathcal{F}$

What motivated this is the writing in the [σ-additive set function wiki](https://en.wikipedia.org/wiki/Sigma-additive_set_function),

 > 
 > Suppose that A ![{\displaystyle \scriptstyle {\mathcal {A}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d586ece9308bf4ef901494079b434c71aac7d41) is a [σ-algebra](https://en.wikipedia.org/wiki/Sigma_algebra "Sigma algebra"). If for every [sequence](https://en.wikipedia.org/wiki/Sequence "Sequence") A 1 , A 2 , … , A n , … ![{\displaystyle A\_{1},A\_{2},\ldots ,A\_{n},\ldots }](https://wikimedia.org/api/rest_v1/media/math/render/svg/7a121200e0c558612beb99e748a738814d788c3f) of pairwise disjoint sets in A ,

I think the point here is because you could take the union of any number of subsets, and the result will hold, not just 2. If you pick two subsets at random and form a pair, you find that they are disjoint, so it follows any randomly sampled sequence too will be disjoint.

The mentioning of sequence here is likely to relax the constraints of repetition and order of [sets](../../../../../../../concept/math/concepts/2025/001%20Set.md), and these are relaxed because the property holds with or without them.

But notice here that the sequence is of the *sets* in $\mathcal{F}$, we are still not saying as we interpret the book to say that $\mathcal{F}$ has within it anywhere some sequence!

2025-08-30 Wk 35 Sat - 21:02

OK I external help/correction on this.

$A_i$ *is* the sequence. I misinterpreted it to be an element. or "$A$ being indexed".

They gave an instructive concrete example like $A, B, C \in \mathcal{F} \implies A \cup B \cup C \in \mathcal{F}$

So I guess it is shorthand for something like $A, B, ...$

I also thought $A_i \subseteq \mathcal{F}$, but this also doesn't make sense since $A_i$ is a sequence $A, B, ... \in \mathcal{F}$. Since it is a sequence, it can always exist outside the bounds of any subset of $\mathcal{F}$.

2025-08-30 Wk 35 Sat - 21:15

Anyway going back to the proposition

 > 
 > if $A_i \in \mathcal{F}$ is a countable sequence of sets then $\bigcup_iA_i \in \mathcal{F}$

Why should this be true?

2025-08-30 Wk 35 Sat - 21:36

I think it is just a restatement of the requirement that [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) is closed under [countable](../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [unions](../../../../../../../concept/math/concepts/2025/008%20union.md).

## 6.3 How do we deal with the empty set in compliments of the sigma algebra?

* [x] 

From [^spawn-invst-7b43a3](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#spawn-invst-7b43a3) in [3.1 Wk 36 Last Week Recap](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#31-wk-36-last-week-recap)

2025-09-06 Wk 36 Sat - 21:40

Let's say we have some Polyhedral d3 Dice:

![Pasted image 20250906214311.png](../../../../../../../../../../../attachments/Pasted%20image%2020250906214311.png)
([image source](https://brycesdice.com/collections/polyhedral-d3-dice-3-sided-dice-numbered-1-2-3/products/translucent-polyhedral-teal-white-d3-pt0315-3-sided-dice))

We're interested in just rolling this dice once. Now we have $\Omega = \{1, 2, 3\}$ .

We want to check that the powerset $\wp(\Omega) \equiv \{\varnothing, \{1\}, \{2\}, \{3\}, \{1, 2\}, \{1, 3\}, \{2, 3\}, \{1, 2, 3\} \}$ with $|\wp(\Omega)| = 2^{|\Omega|} = 2^3 = 8$  is a valid [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$.

Let's check closure under [compliment](../../../../../../../concept/math/concepts/2025/006%20compliment.md), this must satisfy:

(p1)

if $A \in \wp(\Omega)$, then $A^c \in \wp(\Omega)$.

(/p1)

Let's try it.

1. $\varnothing \in \wp(\Omega) \land p_1 \to \{1,2,3\} \in \wp(\Omega)$

Here's the problem:

What's $\{1\}^c$?

I think most likely $\{2, 3\}$ but could it be $\{\varnothing, 2, 3\}$?

I think maybe the universe if we only consider elements of $\Omega$ would be $\{1, 2, 3\}$, and we should treat $\varnothing$ as the set with no elements *in the universe* and not an element of the universe itself.

We know that $A \cup \varnothing = A$.

It should be the case that for distinct elements $a, b \in U$, then $\{a\} \cup \{b\} \ne \{a\} \land \{a\} \cup \{b\} \ne \{b\}$. Meaning that union of sets of distinct universal elements always gives a different set from the inputs.

but with $\varnothing$, $A \cup \varnothing = A$.

2025-09-06 Wk 36 Sat - 22:19

Other than that $\varnothing$ should satisfy compliment under [countable](../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [union](../../../../../../../concept/math/concepts/2025/008%20union.md):

if $A, \varnothing \in \wp(\Omega)$, then $A \cup \varnothing \in \wp(\Omega)$.

## 6.4 Union continuity operations applied to set functions

* [ ] 

2025-09-06 Wk 36 Sat - 23:03

So the book in (pg 2, vpg 10/490) mentions that if $A_i \uparrow A$ (meaning $A_1 \subset A_2 \subset \ldots \subset A$ and $\bigcup_i A_i = A$  ) then it is also the case that $\mu(A_i) \uparrow \mu(A)$.

But what would $\mu(A_i) \uparrow \mu(A)$ mean?

There is info on this in [wiki Measure (mathematics)](https://en.wikipedia.org/wiki/Measure_(mathematics)).

They express a different relation which may relate:

$$
\mu \left(\bigcup _{i=1}^{\infty }E_{i}\right)~=~\lim _{i\to \infty }\mu (E_{i})=\sup _{i\geq 1}\mu (E_{i})
$$

$sup$ here may refer to the [supremum](https://en.wikipedia.org/wiki/Infimum_and_supremum) of a set, but $\mu(E_i)$ is not a set. It's a single value in $\overline{\mathbb{R}}$.

But maybe the minimum value across the set $\{i \in \mathbb{N} | \mu(E_i) \}$

But across all of them, if you find the minimum... then that would be $\mu(E_1)$?

Also some more info on supremum and infimum from [wikibooks supremum and infimum properties](https://en.wikibooks.org/wiki/Math_for_Non-Geeks/Properties_of_supremum_and_infimum).

2025-09-07 Wk 36 Sun - 00:10

The notation is also used in [wiki limit inferior and limit superior](https://en.wikipedia.org/wiki/Limit_inferior_and_limit_superior)...

They called it the limit superior,

$$
\limsup _{n\to \infty }x_{n}:=\lim _{n\to \infty } {\Big (}\sup _{m\geq n}x_{m}{\Big )}
$$

We want to know what this $\sup _{m\geq n}$ mean.

I found this defined in a textbook in Prof John Hunter's website under [m125b Real Analysis](https://www.math.ucdavis.edu/~hunter/m125b/m125b.html) course.

Captured in [000 Mn 09 Resources](../../../../../../../../../2026-05-21-pre/entries-monthly/2025/001%20Resources/entries/000%20Mn%2009%20Resources.md)

Spawn [022 supremum and infinimum](../../../../../../../concept/math/concepts/2025/022%20supremum%20and%20infinimum.md)

2025-09-07 Wk 36 Sun - 01:31

Okay so returning to this:

What does $\mu(A_i) \uparrow \mu(A)$ mean?

And does it relate to this?

$$
\mu \left(\bigcup _{i=1}^{\infty }E_{i}\right)~=~\lim _{i\to \infty }\mu (E_{i})=\sup _{i\geq 1}\mu (E_{i})
$$

It seems that the measure on continuity from below $\mu \left( \bigcup_{i=1}^\infty E_i \right)$ is equal to the least upper bound $\sup_{i \ge 1} \mu(E_i)$ or equivalently, the measure of $E_i$ at $\infty$.

So $\mu(A_i) \uparrow \mu(A)$ might mean that $\mu(A_i)$ holds continuity from below for $\mu(A)$ and is equivalent to that statement?

2025-09-07 Wk 36 Sun - 01:44

In this resource for [calculus analysis symbols](https://mathvault.ca/hub/higher-math/math-symbols/calculus-analysis-symbols/), they mention,

In $\lim_{x \uparrow a}$ $x \uparrow a$ means "x tends to a from the left" and it can also be written as $\lim_{x \to a^-}$.

In $\lim_{x \downarrow a}$, $x \downarrow a$ means "x tends to a from the right" and it can also be written as $\lim_{x \to a^+}$.

Captured to [000 Mn 09 Resources](../../../../../../../../../2026-05-21-pre/entries-monthly/2025/001%20Resources/entries/000%20Mn%2009%20Resources.md)

We can also think "from above" for "from the left" and "from below" for "from the right".

So is this what is meant here? $\mu(A_i) \uparrow \mu(A)$ means that $\mu(A_i)$ will tend from below towards $\mu(A)$ for increasing values of $i$?

I think then, maybe we can write explicitly:

$$
\mu \left(\lim_{i \to \infty^-} A_i \right) = \mu \left( A \right)
$$

But how could this achieve parity with  $\mu \left( A_i \right) \downarrow A$ and $\mu(A_1) < \infty$ ?

We cannot approach $\infty$ from the right or above!

But I would think the general idea still applies, that

$$
\mu \left(\lim_{i \to \infty^-} A_i \right) = \mu \left( A \right)
$$

It would just be that we're coming from the other direction?

2025-09-07 Wk 36 Sun - 02:21

Actually, instead of tending to infinity from the left $\infty^-$, This might be simpler:

Interpret $\mu(A_i) \uparrow A$ as

$$
\mu \left(\lim_{i \to \infty} A_i \right) \le \mu \left( A \right)
$$

and $\mu(A_i) \downarrow A$ as

$$
\mu \left(\lim_{i \to \infty} A_i \right) \ge \mu \left( A \right)
$$

2025-09-07 Wk 36 Sun - 02:27

Seeking confirmation for math help volunteers in discord.

Nothing right now.

2025-09-07 Wk 36 Sun - 03:45

Maybe we will get a textbook confirmation on this interpretation as we go through the proof. See [3.2 Writing Continuity from above and below theorems for probability space](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#32-writing-continuity-from-above-and-below-theorems-for-probability-space)

### 6.4.1 Pend

# 7 Errata

## 7.1 non-empty collection not missing empty subset

In [^errata-e14827](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#errata-e14827)

I wrote

 > 
 > $\mathcal{F}$ is a [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can holds elements up to $\wp(\Omega) - \lbrace \varnothing \rbrace$ where $\wp$ denotes the powerset.

but this is not true. This is because I misinterpreted the definition of σ-algebra. It reads:

 > 
 > A σ-algebra on a set $X$ is a non-empty collection $\Sigma$ of [subsets](../../../../../../../concept/math/concepts/2025/005%20subset.md) of $X$ closed under [compliment](../../../../../../../concept/math/concepts/2025/006%20compliment.md), [countable](../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [union](../../../../../../../concept/math/concepts/2025/008%20union.md), and [countable](../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [intersections](../../../../../../../concept/math/concepts/2025/014%20intersection.md).

The algebra itself must be a *non-empty collection*. I interpreted it as *not having the empty set*.

So as correction,

$\mathcal{F}$ is a [σ-algebra](../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can hold elements up to $\wp(\Omega)$ where $\wp$ denotes the powerset.

# 8 LaTeX Config

$$
\newcommand{\defeq}{{\ \stackrel {\scriptscriptstyle{\text{def}}}{=}}\ }
$$
