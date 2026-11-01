# 1 Journal

* [ ] 

2025-09-07 Wk 36 Sun - 03:42

So in page 2 they're trying to write a proof for continuity from above and from below.

We investigated the meaning of the notation in [6.4 Union continuity operations applied to set functions](001%20Writing%20Continuity%20from%20above%20and%20below%20theorems%20for%20probability%20space.md#64-union-continuity-operations-applied-to-set-functions).

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

### 1.1.1 References

1. [stackexchange answer](https://tex.stackexchange.com/questions/365953/how-do-i-define-a-piecewise-function-in-latex) for piecewise definitions in LaTeX.

### 1.1.2 Pend
