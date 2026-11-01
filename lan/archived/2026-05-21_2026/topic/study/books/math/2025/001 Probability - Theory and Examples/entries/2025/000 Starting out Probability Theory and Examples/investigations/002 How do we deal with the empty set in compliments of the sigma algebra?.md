# 1 Journal

* [x] 

From [^spawn-invst-7b43a3](002%20How%20do%20we%20deal%20with%20the%20empty%20set%20in%20compliments%20of%20the%20sigma%20algebra%3F.md#spawn-invst-7b43a3) in [3.1 Wk 36 Last Week Recap](002%20How%20do%20we%20deal%20with%20the%20empty%20set%20in%20compliments%20of%20the%20sigma%20algebra%3F.md#31-wk-36-last-week-recap)

2025-09-06 Wk 36 Sat - 21:40

Let's say we have some Polyhedral d3 Dice:

![Pasted image 20250906214311.png](../../../../../../../../../../../../../attachments/Pasted%20image%2020250906214311.png)
([image source](https://brycesdice.com/collections/polyhedral-d3-dice-3-sided-dice-numbered-1-2-3/products/translucent-polyhedral-teal-white-d3-pt0315-3-sided-dice))

We're interested in just rolling this dice once. Now we have $\Omega = \{1, 2, 3\}$ .

We want to check that the powerset $\wp(\Omega) \equiv \{\varnothing, \{1\}, \{2\}, \{3\}, \{1, 2\}, \{1, 3\}, \{2, 3\}, \{1, 2, 3\} \}$ with $|\wp(\Omega)| = 2^{|\Omega|} = 2^3 = 8$  is a valid [σ-algebra](../../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$.

Let's check closure under [compliment](../../../../../../../../../concept/math/concepts/2025/006%20compliment.md), this must satisfy:

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

Other than that $\varnothing$ should satisfy compliment under [countable](../../../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [union](../../../../../../../../../concept/math/concepts/2025/008%20union.md):

if $A, \varnothing \in \wp(\Omega)$, then $A \cup \varnothing \in \wp(\Omega)$.
