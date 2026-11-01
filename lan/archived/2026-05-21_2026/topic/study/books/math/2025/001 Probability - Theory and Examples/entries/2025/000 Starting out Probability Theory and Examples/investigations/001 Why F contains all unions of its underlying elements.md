# 1 Journal

* [x] 

So in pg1 (vpg 9/490) of the [book](https://sites.math.duke.edu/~rtd/PTE/PTE5_011119.pdf),

They say

 > 
 > if $A_i \in \mathcal{F}$ is a countable sequence of sets then $\bigcup_iA_i \in \mathcal{F}$

Why would this be true?

2025-08-30 Wk 35 Sat - 17:54

(1)

$\mathcal{F}$ is a [σ-algebra](../../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can hold elements up to $\wp(\Omega)$ where $\wp$ denotes the powerset.

^errata-e14827

2025-08-30 Wk 35 Sat - 18:22

Spawn [Drawing 2025-08-30 18.25.14.excalidraw](../../../../drawings/Drawing%202025-08-30%2018.25.14.excalidraw.md) (sketch)

$\mathcal{F}$'s elements are possible *subsets* of $\Omega$, yet here the book says that an element $A_i$ is a countable *sequence*?

An element $A$ of $\mathcal{F}$ should be a subset of $\Omega$. An element $A_i$ of $A$ should be considered an individual item.

2025-08-30 Wk 35 Sat - 18:58

In [σ-additive defn premise 3](../../../../../../../../../concept/math/concepts/2025/018%20%CF%83-additive.md#199261-premise-3),

We also mention sequences, and this might be where this comes from.

 > 
 > Let $S$ be the set of all [sequences](../../../../../../../../../concept/math/concepts/2025/020%20Seq.md) of pairwise disjoint \[^1\] sets in $\mathcal{F}$

What motivated this is the writing in the [σ-additive set function wiki](https://en.wikipedia.org/wiki/Sigma-additive_set_function),

 > 
 > Suppose that A ![{\displaystyle \scriptstyle {\mathcal {A}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d586ece9308bf4ef901494079b434c71aac7d41) is a [σ-algebra](https://en.wikipedia.org/wiki/Sigma_algebra "Sigma algebra"). If for every [sequence](https://en.wikipedia.org/wiki/Sequence "Sequence") A 1 , A 2 , … , A n , … ![{\displaystyle A\_{1},A\_{2},\ldots ,A\_{n},\ldots }](https://wikimedia.org/api/rest_v1/media/math/render/svg/7a121200e0c558612beb99e748a738814d788c3f) of pairwise disjoint sets in A ,

I think the point here is because you could take the union of any number of subsets, and the result will hold, not just 2. If you pick two subsets at random and form a pair, you find that they are disjoint, so it follows any randomly sampled sequence too will be disjoint.

The mentioning of sequence here is likely to relax the constraints of repetition and order of [sets](../../../../../../../../../concept/math/concepts/2025/001%20Set.md), and these are relaxed because the property holds with or without them.

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

I think it is just a restatement of the requirement that [σ-algebra](../../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) is closed under [countable](../../../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [unions](../../../../../../../../../concept/math/concepts/2025/008%20union.md).
