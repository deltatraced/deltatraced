---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries#^spawn-entry-71f348|^spawn-entry-71f348]]

# Journal

2026-09-09 Wk 37 Wed - 20:11 +03:00

```haskell
data _⊎_
  {ℓ₁ ℓ₂ : Level}
  (A : Type ℓ₁)
  (B : Type ℓ₂)
  : Type (ℓ-max ℓ₁ ℓ₂)
  where
    inl : A → A ⊎ B
    inr : B → A ⊎ B
```

I wanted to write this myself without looking, to get used to this syntax of defining inductive data types.

The way I interpret this is (as of this writing):
- First, we need this to be universe-polymorphic. We specify automatically inferred parameters `ℓ₁ ℓ₂` for any universe level, and our type is in the max of the two, since it just lifts the lower one up by packaging it under `inl` or `inr`
- This can often look like function syntax, and we can think of `_⊎_` like an operator between two types, to define a new type for us (CQTS authors call it a *type former*). So that for any `A` and `B`, there could also be `A ⊎ B`.
	- I wanted to call this a type constructor, but it specifies the type constructors under `where`.
- I think of the `inl` and `inr` as not functions, so I'm not thinking of `inl` for example as a map out of `A` and into `A ⊎ B`. Instead, `inl` is type constructor. It describes a mathematical object, just like `zero` and `suc (zero)` does for the natural numbers, or `false` and `true` for booleans. This object, *to be specified*, requires as part of its definition a value of `A`. So there is no such thing as just `inl`, a full object is always an `inl a` for some `a : A`. 
	- So what does the `→ A ⊎ B` bit mean? I read it "forms the given type". So `inl`, alongside with `A`, forms the given type `A ⊎ B`.

```haskell
data _⊎_
  {ℓ₁ ℓ₂ : Level}
  (A : Type ℓ₁)
  (B : Type ℓ₂)
  : Type (ℓ-max ℓ₁ ℓ₂)
-- Declares how we can specify `_⊎_`. I read it as requiring an A and a B.
-- `: Type (ℓ-max ℓ₁ ℓ₂)` I interpret as specifying that we're forming a new type, and it allows us to configure it for universe polymorphism

  where
-- v defines the constructors for _⊎_
    inl : A → A ⊎ B
    inr : B → A ⊎ B
```

Sometimes we write a map that outputs a `Type`: 

```haskell
{C : (a : A) → (b : B a) → Type ℓ₃}
```
This I interpret as there being any definition of a type under `Type ℓ₃`. Here we may refer to `C a b`, but the code supplier can write any definition that fits the form of depending on a and `B a`. So `→ Type ℓ₃` here could also be read as yielding some definition of a type that depends on the prior `(a : A) → (b : B a)`.
