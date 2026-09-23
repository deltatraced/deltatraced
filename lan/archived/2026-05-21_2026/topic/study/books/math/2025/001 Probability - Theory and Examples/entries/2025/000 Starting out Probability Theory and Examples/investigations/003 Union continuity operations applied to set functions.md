# 1 Journal

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

Captured in [000 Mn 09 Resources](../../../../../../../../../../../2026-05-21-pre/entries-monthly/2025/001%20Resources/entries/000%20Mn%2009%20Resources.md)

Spawn [022 supremum and infinimum](../../../../../../../../../concept/math/concepts/2025/022%20supremum%20and%20infinimum.md)

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

Captured to [000 Mn 09 Resources](../../../../../../../../../../../2026-05-21-pre/entries-monthly/2025/001%20Resources/entries/000%20Mn%2009%20Resources.md)

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

Maybe we will get a textbook confirmation on this interpretation as we go through the proof. See [3.2 Writing Continuity from above and below theorems for probability space](003%20Union%20continuity%20operations%20applied%20to%20set%20functions.md#32-writing-continuity-from-above-and-below-theorems-for-probability-space)

### 1.1.1 Pend
