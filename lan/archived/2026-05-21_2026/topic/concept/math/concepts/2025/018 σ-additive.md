# 1 Definition

## 1.1 Wiki

See the wiki: [σ-additive set function](https://en.wikipedia.org/wiki/Sigma-additive_set_function)

(1)

Let $\mathcal{F}$ be a [σ-algebra](007%20%CF%83-algebra.md) over some [set](001%20Set.md) $\Omega$

(2)

Let $\mu$ be a [set function](019%20set%20function.md) $\mu : \mathcal{F} \to \overline{\mathbb{R}}$

(3) ^199261-premise-3

Let $S$ be the set of all [sequences](020%20Seq.md) of pairwise disjoint [^1] sets in $\mathcal{F}$

(3)

$\mu$ is σ-additive if for every sequence $A \in S$,

$$

\\mu \left(\bigcup\_{n=1}^{\infty} A\_{n} \right) = \sum\_{n=1}^{\infty} \mu (A\_{n})

$$

So the measure of the union of all sets in sequence $A_n$ is equivalent to the measure of its component parts.

# 2 Properties

## 2.1 Empty Set Value

If there is at least one set $A$ has a finite measure, then $\mu(\varnothing) = 0$.

(1)

Let $\mu(A)$ = C, where $C$ is some finite number in $\overline{\mathbb{R}}$.

(1)

$$

\\mu(A) = \mu(A \cup \varnothing)

$$

This is satisfied by definition of union with $\varnothing$.

(2)

$$

\\mu(A) = \mu(A \cup \varnothing) = \mu(A) + \mu(\varnothing)

$$

This is satisfied by definition of σ-additivity

(3)

$$
\begin{aligned}
& \mu(A) \\
& = \mu(A) + \mu(\varnothing) \\
& = C + \mu(\varnothing) \\
& = C
\end{aligned}
$$

$\therefore$  So because $\mu(A)$  is finite,  $\mu(\varnothing)$ must be 0.

Note that if $\mu(A) = \pm \infty$, then $\mu(\varnothing)$ could be any value other than $-\infty$ here. That's because the result would produce $\infty - \infty$ which is an [undefined form](017%20RealExt.md#undefined-forms).

[^1]: pairwise disjoint means if you pick any random pair of sets, you find they are disjoint (share no elements)

# 3 Also called

countably additive.
