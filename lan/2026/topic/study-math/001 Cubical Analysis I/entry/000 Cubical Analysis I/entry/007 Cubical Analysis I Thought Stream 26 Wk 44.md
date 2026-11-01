---
context_type: entry
---

Parent: [lan/2026/topic/study-math/001 Cubical Analysis I/entry/000 Cubical Analysis I/000 Cubical Analysis I](../000%20Cubical%20Analysis%20I.md)

Spawned by: [lan/2026/topic/study-math/001 Cubical Analysis I/entry/000 Cubical Analysis I/entry/000 Cubical Analysis I Thought Stream](000%20Cubical%20Analysis%20I%20Thought%20Stream.md)

Spawned in: [^spawn-entry-058631](000%20Cubical%20Analysis%20I%20Thought%20Stream.md#spawn-entry-058631)

# Journal

2026-10-28 Wk 44 Wed - 09:58 +03:00

I tried to also explore list permutations, to see if we can identify the sum of two lists when they happen to be permutations of one another.

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  where
    List-∈ : (dec≡-A : Dec≡ A) → (as : List A) → (aₛ : A) → Type
    List-∈ dec≡-A [] aₛ = ∅
    List-∈ dec≡-A (a :: as) aₛ = h0 (dec≡-A a aₛ)
      where
        h0 : (dec : Dec (a ≡ aₛ)) → Type
        h0 (yes a≡aₛ) = ⊤
        h0 (no a≠aₛ) = List-∈ dec≡-A as aₛ

    List-∈-first-found :
        (dec≡-A : Dec≡ A)
      → (as : List A)
      → (a aₛ : A)
      → (a≡aₛ : a ≡ aₛ)
      → (List-∈ dec≡-A (a :: as) aₛ) ≡ ⊤
    List-∈-first-found dec≡-A as a aₛ a≡aₛ = h0 (dec≡-A a aₛ)
      where
        h0 : (dec : Dec (a ≡ aₛ)) → (List-∈ dec≡-A (a :: as) aₛ) ≡ ⊤
        h0 (yes p) = {!!} -- seems we're computationally stuck
        h0 (no ¬p) = {!!}
````

Even with this first simple property, it's unclear how to unpack a definition that uses an inner definition with decidable equality.

--/ 2026-10-28 Wk 44 Wed - 10:42 +03:00

We managed with this, by having the definition for `h0` of `List-∈` be a public name.

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  where
    List-∈-non-empty :
        (a aₛ : A)
      → (as : List A)
      → (dec≡-A : Dec≡ A)
      → (dec : Dec (a ≡ aₛ))
      → Type
    List-∈ : (dec≡-A : Dec≡ A) → (as : List A) → (aₛ : A) → Type

    List-∈-non-empty a aₛ as dec≡-A (yes a≡aₛ) = ⊤
    List-∈-non-empty a aₛ as dec≡-A (no a≠aₛ) = List-∈ dec≡-A as aₛ
      
    List-∈ dec≡-A [] aₛ = ∅
    List-∈ dec≡-A (a :: as) aₛ = List-∈-non-empty a aₛ as dec≡-A (dec≡-A a aₛ)

    List-∈-first-found :
        (dec≡-A : Dec≡ A)
      → (as : List A)
      → (a : A)
      → (List-∈ dec≡-A (a :: as) a) ≡ ⊤
    List-∈-first-found dec≡-A as a = h0 (dec≡-A a a)
      where
        h0 : (dec : Dec (a ≡ a)) → List-∈-non-empty a a as dec≡-A dec ≡ ⊤
        h0 (yes a≡a) = refl
        h0 (no a≠a) = ∅-rec (a≠a refl)
````

--/

2026-10-28 Wk 44 Wed - 18:38 +03:00

https://unimath.github.io/agda-unimath/lists.permutation-lists.html

I created the permutations space of `List A` like this:

````haskell
    -- All ways that a list is equivalent to itself form the permutation space of lists
    List-Permutations : Type ℓ
    List-Permutations = (List A) ≃ (List A)
````

But maybe we should separate permutation from lists as they do above for more generality.

2026-10-28 Wk 44 Wed - 23:44 +03:00

https://unimath.github.io/agda-unimath/lists.finite-sequences.html#1340

Finite arrays!

It is cool to think of an array as just the map space from the index to the item.

2026-10-29 Wk 44 Thu - 00:06 +03:00

````
-- Inspired by https://agda.github.io/agda-stdlib/master/Data.Fin.Base.html
-- Fin n is a type with n+1 elements
data Fin : ℕ → Type where
  zeroᶠ : {n : ℕ} → Fin (suc n)
  sucᶠ : {n : ℕ} → (m : Fin n) → Fin (suc n)
````

With this definition, since every `suc` requires a value from the prior finite partition, `n` basically limits how many sucs you can have. When you ask for an object of `Fin 0`, there are none, and so such a suc no longer can be provided, and with this we're limiting the upper range of the construction!

It's a down counter requrement! Always reachng a `Fin 0` which is undefined.

2026-10-29 Wk 44 Thu - 22:59 +03:00

https://github.com/vikraman/generalised-species/blob/master/set/MSet.agda

````
data MSet {ℓ} (A : Type ℓ) : Type ℓ where
  [] : MSet A
  _::_ : (x : A) (xs : MSet A) → MSet A
  swap : (x y : A) (xs : MSet A)
       → x :: y :: xs ≡ y :: x :: xs
  trunc : isSet (MSet A)
````

MSet! They have `swap` built-in so that order doesn't matter.  And then it's truncated to be a set, similar to how propositional truncation truncates to prop.

Ran into this resource through that! https://github.com/DesyncTheThird/Reference-Book/blob/main/Reference.pdf

It is cool that some math students go about writing their own reference note/books!

Added to [010 TSW Resources 26 Wk 44](../../../../../../main/entry/007%20Thought%20Stream%20Weeks/entry/010%20TSW%20Resources%2026%20Wk%2044.md)

2026-10-30 Wk 44 Fri - 06:10 +03:00

We should also be able to add an extra clause to the mset/multiset to make it a set: `uniq : x :: x :: xs ≡ x :: xs`.
