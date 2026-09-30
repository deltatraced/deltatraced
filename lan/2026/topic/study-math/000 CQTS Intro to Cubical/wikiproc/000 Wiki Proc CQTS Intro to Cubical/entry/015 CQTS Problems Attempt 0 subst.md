---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[009 CQTS Problems Attempt Entries]]

Spawned in: [[009 CQTS Problems Attempt Entries#^spawn-entry-39fc73|^spawn-entry-39fc73]]

# Journal

2026-09-19 Wk 38 Sat - 21:07 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-3-substitution-and-J.agda`,

```haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {a a' : A}
  where
    subst :
      (B : A → Type ℓ₂)
      → (p : a ≡ a')
      → B a
      → B a'
    subst B p b = {!transport (λ (i : I) → B (p i)) b!}
```

This fails to be resolved. `C-c C-.` gives

```
error: [UnequalTypes]
The type
  (i : I) → Type ℓ₂
is not a subtype of
  _A_20 ≡ _B_21
when checking that the expression λ (i : I) → B (p i) has type
_A_20 ≡ _B_21
```

```haskell
subst B p b = {!transport (λ (i) → B (p i)) b!}
```

yields

```
error: [PatternInPathLambda]
Patterns are not allowed in Path-lambdas
when checking that the expression λ (i) → B (p i) has type
_A_20 ≡ _B_21
```

Finally

```haskell
subst B p b = {!transport (λ i → B (p i)) b!}
```

yields `Have: B a'`.

It seems that paths from `i` are treated specially here.

Though sometimes I can get away with specifying `(_ : I)`. for example like here:

```haskell
module _
  {ℓ : Level}
  {A B : Type ℓ}
  where
    -- transport is derived through the more general transport-fixing
    transport : A ≡ B → A → B
    transport p a = transport-fixing (λ (i : I) → p i) i0 a
```

Here too

```haskell
subst B p b = {!(λ (i : I) → p i) i1!}
```

giving `Have: A`
