---
context_type: investigation
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [000 Rust Can we process a compile-time serial parallel DAG of tokens?](000%20Rust%20Can%20we%20process%20a%20compile-time%20serial%20parallel%20DAG%20of%20tokens%3F.md)

Spawned in: [^spawn-invst-11da35](000%20Rust%20Can%20we%20process%20a%20compile-time%20serial%20parallel%20DAG%20of%20tokens%3F.md#spawn-invst-11da35)

# Journal

2026-06-05 Wk 23 Fri - 10:41 +03:00

For example,

* `(Input<5>, (Add<2>, Mult<9>))` $\to$ 63
* `(Input<5>, (OpenParen, (Add<2>, (Mult<9>, CloseParen))) ` $\to$ 23

Code in `/home/lan/src/cloned/gh/LanHikari22/rs_repro/examples/2026_expt000_tuple_simple_calc.rs`.

2026-06-05 Wk 23 Fri - 11:25 +03:00

For a first iteration, `subexpt000`, we can see from this example that we are able to easily parse the recursive tuples.

`subexpt001` shows some expression reduction as well. So this is possible to do some expression simplifications with.
