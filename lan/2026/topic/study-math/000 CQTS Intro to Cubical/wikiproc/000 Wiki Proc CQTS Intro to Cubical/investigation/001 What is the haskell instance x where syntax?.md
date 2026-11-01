---
context_type: investigation
status: done
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/issue/000 unequal terms for partial pattern matching CQTS](../issue/000%20unequal%20terms%20for%20partial%20pattern%20matching%20CQTS.md)

Spawned in: [^spawn-invst-6f5d8f](../issue/000%20unequal%20terms%20for%20partial%20pattern%20matching%20CQTS.md#spawn-invst-6f5d8f)

# Solution

`instance C T` is a `C-T instance decleration` which specifies that type `T` satisfies bound `C` where `C` is some class.

# Journal

2026-06-23 Wk 26 Tue - 14:02 +03:00

What is the haskell `instance X where` syntax?

1. https://downloads.haskell.org/ghc/latest/docs/users_guide/exts/typeclasses.html
1. https://www.haskell.org/onlinereport/haskell2010/haskellch10.html#x17-17700010.2
   * in
     1. `topdecl` $\to$ `type simpletype = type`
     1. `instance [scontext =>] qtycls inst [where idecls]`
1. https://www.haskell.org/onlinereport/haskell2010/haskellch4.html#x10-750004.3
   * in
     * `4.3.2 Instance Declerations`

We've seen in the docs that classes are like bounds on types, for example `Eq a` and `Ord a`, and we can constraint types by them: `Eq a => a -> a -> a` with the =>.

In the example

````haskell
class Foo a => Bar a where ...
instance (Eq a, Show a) => Foo [a] where ...
````

We can see that they are defining a new class (a bound; a constraint; a type class) `Bar` that applies only when the bound `Foo` applies for all types `a`.

Then `instance` specifies that, for all `a` which satisfy the bounds `Eq a` and `Show a`, type `[a]` satisfies the bound `Foo`.

`instance` is similar to an `impl` block in Rust traits that implement sharable named functionality for some type.
