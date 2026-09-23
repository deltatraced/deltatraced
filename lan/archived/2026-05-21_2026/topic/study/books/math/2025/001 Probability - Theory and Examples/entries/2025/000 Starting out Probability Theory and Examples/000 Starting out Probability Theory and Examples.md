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

Putting probability space definition in [000 Probability Space](../../../concepts/2025/000%20Probability%20Space.md)

2025-09-06 Wk 36 Sat - 22:53

Last we reached is defining the properties continuity from below and continuity from above for [000 Probability Space](../../../concepts/2025/000%20Probability%20Space.md)

# 3 Errata

## 3.1 non-empty collection not missing empty subset

In [^errata-e14827](000%20Starting%20out%20Probability%20Theory%20and%20Examples.md#errata-e14827)

I wrote

 > 
 > $\mathcal{F}$ is a [σ-algebra](../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can holds elements up to $\wp(\Omega) - \lbrace \varnothing \rbrace$ where $\wp$ denotes the powerset.

but this is not true. This is because I misinterpreted the definition of σ-algebra. It reads:

 > 
 > A σ-algebra on a set $X$ is a non-empty collection $\Sigma$ of [subsets](../../../../../../../../concept/math/concepts/2025/005%20subset.md) of $X$ closed under [compliment](../../../../../../../../concept/math/concepts/2025/006%20compliment.md), [countable](../../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [union](../../../../../../../../concept/math/concepts/2025/008%20union.md), and [countable](../../../../../../../../concept/math/concepts/2025/010%20Countable%20set.md) [intersections](../../../../../../../../concept/math/concepts/2025/014%20intersection.md).

The algebra itself must be a *non-empty collection*. I interpreted it as *not having the empty set*.

So as correction,

$\mathcal{F}$ is a [σ-algebra](../../../../../../../../concept/math/concepts/2025/007%20%CF%83-algebra.md) over $\Omega$, so it can hold elements up to $\wp(\Omega)$ where $\wp$ denotes the powerset.

# 4 LaTeX Config

$$
\newcommand{\defeq}{{\ \stackrel {\scriptscriptstyle{\text{def}}}{=}}\ }
$$
