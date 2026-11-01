---
parent: '[[000 Experiment with Rocq]]'
spawned_by: '[[002 Looking into Software Foundations tutorial]]'
context_type: task
status: pend
---

Parent: [000 Experiment with Rocq](../000%20Experiment%20with%20Rocq.md)

Spawned by: [002 Looking into Software Foundations tutorial](../entries/002%20Looking%20into%20Software%20Foundations%20tutorial.md)

Spawned in: [^spawn-task-aa36aa](../entries/002%20Looking%20into%20Software%20Foundations%20tutorial.md#spawn-task-aa36aa)

# 1 Journal

2026-02-06 Wk 6 Fri - 12:40 +03:00

While working on this and analysis I exercises,

Not sure how to prove `mult_cancel` $\forall a\ b\ c \in \mathbb{N}, a \cdot b = a \cdot c \implies a = 0 \lor b = c$.

[Rocq stdlib index](https://docs.rocq-prover.org/V8.9.1/stdlib/index_lemma_N.html) seems to point to some `mul_cancel_l`

[NZMulOrderProp.mul_cancel_l](https://docs.rocq-prover.org/V8.9.1/stdlib/Coq.Numbers.NatInt.NZMulOrder.html#NZMulOrderProp.mul_cancel_l) doesn't seem quite right with its use of double equals, and I don't see the proof directly here.

2026-02-06 Wk 6 Fri - 12:40 +03:00

2026-02-06 Wk 6 Fri - 16:05 +03:00

After generalizing the dependent c, `IHb'` is as we expect: `IHb' : forall c : nat, S a' * b' = S a' * c -> b' = c \/ S a' = 0`. This really should apply no matter your choice of `c`.

2026-02-06 Wk 6 Fri - 16:33 +03:00

The `destruct (IHb' c' H0)` then fits `c'` to the generalized $\forall c : nat$ in the inductive hypothesis, and in doing that, `H0` fits its form exactly, so then we only need to prove the right hand side of its implication.
