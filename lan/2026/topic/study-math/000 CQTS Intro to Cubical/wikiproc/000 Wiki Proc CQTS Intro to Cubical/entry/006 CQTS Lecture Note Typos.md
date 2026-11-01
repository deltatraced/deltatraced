---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical#^spawn-entry-56c6b9|^spawn-entry-56c6b9]]

# What?

Tracking typos that I read in the lecture notes, to hopefully go correct them all at once at some point.

# Journal

2026-09-10 Wk 37 Thu - 14:21 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-3--Universes-and-More-Inductive-Types.lagda.md`,

```
## Type Arithmetic

There is a sense in which ``×`` of types acts like ordinary
multiplication of natural numbers. Because ``Bool`` has 2 elements and
``Day`` has 7, the product should have should have 14, which we can
check by case-splitting `Bool × Day` into all its possibilities.
```

should be 

```
## Type Arithmetic

There is a sense in which ``×`` of types acts like ordinary
multiplication of natural numbers. Because ``Bool`` has 2 elements and
``Day`` has 7, the product ~should have~ should have 14, which we can
check by case-splitting `Bool × Day` into all its possibilities.
```

2026-09-10 Wk 37 Thu - 16:54 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-4--Record-Types-and-Copatterns.lagda.md`,

```
Definition clauses like this are called *copatterns*. Ordinary pattern
matching explains what a function does when its input is a particular
constructor. Copattern matching explains what a function does its
output used by a particular *eliminator*.
```

should be

```
Definition clauses like this are called *copatterns*. Ordinary pattern
matching explains what a function does when its input is a particular
constructor. Copattern matching explains what a function does [when] its
output [is] used by a particular *eliminator*.
```

2026-09-10 Wk 37 Thu - 22:42 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-5--Propositions-as-TyIn `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/2--Paths-and-Identifications/2-9--Contractible-Maps.lagda.md`,
pes.lagda.md`,

```
It would be tedious if we had to define the specific notion of
equality we wanted for every type that we ever define. It's also not
entirely exactly how to do it in more difficult cases.
```

might be

```
It would be tedious if we had to define the specific notion of
equality we wanted for every type that we ever define. It's also not
entirely [clear?] exactly how to do it in more difficult cases.
```

2026-09-14 Wk 38 Mon - 18:01 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-1--Paths.lagda.md`,

```
Now we for paths with specified endpoints. For `x` and `y`, Agda
provides a built-in type `x ≡ y` which is like a function `I → A`, but
where the endpoints are known to be exactly `x` and `y`. That is,
starting with `p : x ≡ y`, evaluating `p i0` gives *exactly* `x`, and
evaluating `p i1` gives *exactly* `y`, regardless of what the
definition of the path `p` actually is.
```

might be

```
Now we [have?] paths with specified endpoints. For `x` and `y`, Agda
provides a built-in type `x ≡ y` which is like a function `I → A`, but
where the endpoints are known to be exactly `x` and `y`. That is,
starting with `p : x ≡ y`, evaluating `p i0` gives *exactly* `x`, andIn `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/2--Paths-and-Identifications/2-9--Contractible-Maps.lagda.md`,

evaluating `p i1` gives *exactly* `y`, regardless of what the
definition of the path `p` actually is.
```

2026-09-16 Wk 38 Wed - 08:51 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-2--Equivalences-and-Path-Algebra.lagda.md`,

```
For most of the types we've seen so far, we have a obvious candidate
for what paths *should* be for that type. For ``Bool`` we have
``≡Bool``, for ``ℕ`` we have ``≡ℕ``, and for pairs and functions we
saw ``×≡→≡×`` and ``funext`` respectively.
```

should be

```
For most of the types we've seen so far, we have [an] obvious candidate
for what paths *should* be for that type. For ``Bool`` we have
``≡Bool``, for ``ℕ`` we have ``≡ℕ``, and for pairs and functions we
saw ``×≡→≡×`` and ``funext`` respectively.
```

2026-09-22 Wk 39 Tue - 01:47 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-4--Composition-and-Filling.lagda.md`,

```
We can ask whether a partial element `p` *extends* to an fully defined
element `x`. That is, is there an `x : A` so that `p ≡ just x`? In
this case, we say that "`x` *extends* `p`". For ``zeroOrOne-Partial``
we can say the answer is yes.
```

should be

```
We can ask whether a partial element `p` *extends* to [a] fully defined
element `x`. That is, is there an `x : A` so that `p ≡ just x`? In
this case, we say that "`x` *extends* `p`". For ``zeroOrOne-Partial``[,]
we can say the answer is yes.
```


2026-09-22 Wk 39 Tue - 01:47 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-4--Composition-and-Filling.lagda.md`,

```
::: Aside:
Every element of a type universe `Type ℓ` is supports ``hcomp``, and
that encompasses almost every type that we use in practice. For
technical reasons, some of Agda's back-end plumbing types do not
support ``hcomp``. The only ones that get mentioned in these notes are
the interval ``I`` itself, the ``IsOne`` predicate, and partial
elements ``PartialP``. The types for which ``hcomp`` works are called
*fibrant* types, taking a name from homotopy theory. The types for
which ``hcomp`` fails live in their own universe hierarchy ``SSet``.
:::
```

should be 

```
::: Aside:
Every element of a type universe `Type ℓ` ~is~ supports ``hcomp``, and
that encompasses almost every type that we use in practice. For
technical reasons, some of Agda's back-end plumbing types do not
support ``hcomp``. The only ones that get mentioned in these notes are
the interval ``I`` itself, the ``IsOne`` predicate, and partial
elements ``PartialP``. The types for which ``hcomp`` works are called
*fibrant* types, taking a name from homotopy theory. The types for
which ``hcomp`` fails live in their own universe hierarchy ``SSet``.
:::
```

2026-09-22 Wk 39 Tue - 06:43 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-4--Composition-and-Filling.lagda.md`,

```
  For the double composite, we are specifying the left and right sides
  of the square, that is, the places where where `∂ i` holds.
```

should be

```
  For the double composite, we are specifying the left and right sides
  of the square, that is, the places where ~where~ `∂ i` holds.
```

2026-09-23 Wk 39 Wed - 06:42 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-5--Transport.lagda.md`

```
path→equiv : A ≡ B → A ≃ B
-- ExercisE:
```

should be

```
path→equiv : A ≡ B → A ≃ B
-- Exercis[e]:
```

2026-09-23 Wk 39 Wed - 06:48 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-5--Transport.lagda.md`

```
There is a second way that ``PathP`` and ``transport`` relate. Recall
that an element of `PathP A a₀ a₁` connects two elements `a₀ : A i0`
and `a₁ : A i1` of types at either end of a line `A : I → Type`, so
bthat the type is allowed to vary as we travel from `a₀` to `a₁`.
```

should be

```
There is a second way that ``PathP`` and ``transport`` relate. Recall
that an element of `PathP A a₀ a₁` connects two elements `a₀ : A i0`
and `a₁ : A i1` of types at either end of a line `A : I → Type`, so
~b~that the type is allowed to vary as we travel from `a₀` to `a₁`.
```

2026-09-23 Wk 39 Wed - 07:16 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-5--Transport.lagda.md`

This typo has to be inferred conceptually, so we need to ask the authors for confirmation.

```
To go back the other way, we will use ``transport-fixing`` again, but
in a new way. When `i = i0` we want `fromPathP p i0 = transport (λ i →
B i) b1` and when `i = i1` we want `fromPathP p i1 = b2`. So we will
ask for ``transport-fixing`` to be constant when `i = i1`.
```

They mention `b1` and `b2` but there are no such values. That said, we do find these which were talked about before, and this passes the test;

```haskell
_ = λ (p : PathP A a₀ a₁)
    → test-identical (PathP→Path p i0) (transport (λ j → A j) a₀)
_ = λ (p : PathP A a₀ a₁)
    → test-identical (PathP→Path p i1) a₁
```

So this is likely what they meant. a₀ and a₁, not b1 and b2. Similarly we're working with `A`, not `B` as in `B i`.

2026-09-23 Wk 39 Wed - 11:51 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-6--Univalence.lagda.md`,

In the references,

https://martinescardo.github.io/HoTT-UF-in-Agda-Lecture-Notes/index.htmlure-Notes/HoTT-UF-Agda.html gives a 404.

https://martinescardo.github.io/HoTT-UF-in-Agda-Lecture-Notes/index.html is reachable.

2026-09-24 Wk 39 Thu - 01:43 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-6--Univalence.lagda.md`,

This has an incomplete sentence

```
A ``Glue`` type enable us to "cap off" this open box of types, giving us a . That
is, if `φ = ∂ i`, then `Glue A Te : Type` is the line of types living
*under* `A` in the capped off version of the above square.
```

```
A ``Glue`` type enable us to "cap off" this open box of types, giving us a [???]. That
is, if `φ = ∂ i`, then `Glue A Te : Type` is the line of types living
*under* `A` in the capped off version of the above square.
```

a what? a full element?

2026-10-01 Wk 40 Thu - 18:38 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-7--Propositions.lagda.md`,

```
First, we have implication. If `A` and `B` are propositions then the
type of functions `A → B` is a proposition --- namely, the proposition
that `A` implies `B`.

isProp-→ : isProp B → isProp (A → B)
-- Exercise:
isProp-→ p f g i a = {!!}
```

Should be

```
First, we have implication. If ~~`A` and ~~`B` [is a] proposition~~s~~ then the
type of functions `A → B` is a proposition --- namely, the proposition
that `A` implies `B`.

isProp-→ : isProp B → isProp (A → B)
-- Exercise:
isProp-→ p f g i a = {!!}
```

We know this is a conceptual typo, since later they use this result for a type `A` that might not be a proposition:

```
As a special case of implication, we find that type negation `¬ A` is
always a proposition even when `A` isn't.
```

2026-10-05 Wk 41 Mon - 13:37 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-8--Sets-and-Higher-Types.lagda.md`,

```
Similarly to contractible types and propositions, any retract of a set
is a set. This follows easily from the following general about fact
about homotopies, which is a higher-dimensional generalisation of
``homotopy-Path``. On its own, a homotopy lets us
```

should be

```
Similarly to contractible types and propositions, any retract of a set
is a set. This follows easily from the following general ~~about~~ fact
about homotopies, which is a higher-dimensional generalisation of
``homotopy-Path``. On its own, a homotopy lets us
```

2026-10-07 Wk 41 Wed - 00:15 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-8--Sets-and-Higher-Types.lagda.md`,

```
This is all well and good, but do we have concrete examples of types
with a h-level higher than that of sets? We saw in ``¬isSet-S¹`` that
``S¹`` is not a set, but it does have the next h-level beyond that.
(Such types are often called "groupoids".)
```

should be

```
This is all well and good, but do we have concrete examples of types
with [an] h-level higher than that of sets? We saw in ``¬isSet-S¹`` that
``S¹`` is not a set, but it does have the next h-level beyond that.
(Such types are often called "groupoids".)
```

2026-10-09 Wk 41 Fri - 01:30 +03:00

In `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/2--Paths-and-Identifications/2-9--Contractible-Maps.lagda.md`,

```
  path : x₀ ≡ x₁
  -- Exercise:
  path₂ = {!!} ∙∙ refl ∙∙ {!!}
```

should be

```
  path₂ : x₀ ≡ x₁
  -- Exercise:
  path₂ = {!!} ∙∙ refl ∙∙ {!!}
```

or

```
  path : x₀ ≡ x₁
  -- Exercise:
  path = {!!} ∙∙ refl ∙∙ {!!}
```

They refer to `path` afterwards.

2026-10-09 Wk 41 Fri - 19:45 +03:00

In `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/2--Paths-and-Identifications/2-9--Contractible-Maps.lagda.md`,

```
We knew already that univalence ``ua`` has a retract ``au``. But we
can now use ``equiv≡`` to show that ``au`` is also a section, and so
``ua`` is an equivalence.

au-ua : (e : A ≃ B) → au (ua e) ≡ e
-- Exercise: (Hint: ``ua-comp``)
au-ua e = {!!}
```

This seems to be a conceptual typo.

`ua-au` shows us that `au` is a section. `au-ua` shows us that `au` is a retract (that is, `ua` now is its section).

2026-10-13 Wk 42 Tue - 20:36 +03:00

In `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/3--Topics/3-1--Structure-Identity-Principle.lagda.md`,

```
In Lecture 2-6 we saw how univalence can be used to show that paths
between types the same as equivalences between those types. But what
if our types have extra structure, like algebraic operations or
axioms? In this Lecture, we extend univalence to the *Structure
Identity Principle*, which shows that paths between structured types
are equivalent to structure-preserving equivalences between those
types.
```

should be 

```
In Lecture 2-6 we saw how univalence can be used to show that paths
between types [are] the same as equivalences between those types. But what
if our types have extra structure, like algebraic operations or
axioms? In this Lecture, we extend univalence to the *Structure
Identity Principle*, which shows that paths between structured types
are equivalent to structure-preserving equivalences between those
types.
```