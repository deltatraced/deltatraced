---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-f15711](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-f15711)

# Journal

2026-09-23 Wk 39 Wed - 06:06 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

````haskell
module _
  {ℓ : Level}
  {A B : Type ℓ}
  where
    transport-cancel :
        (p : A ≡ B)
      → (b : B)
      → transport (λ i → p i) (transport (λ i → (sym p) i) b) ≡ b
    transport-cancel p b i = 
      transport-fixing (λ j → p (i ∨ j)) i
     (transport-fixing (λ j → (sym p) (~ i ∧ j)) i b)
````

As with the hints in the CQTS lecture notes, we had to design this transport of transport expression directly with uses of cubical intervals that make it reduce.

````haskell
    transport-cancel p b i = {!
      transport-fixing (λ j → p (i ∨ j)) i
     (transport-fixing (λ j → (sym p) (~ i ∧ j)) i b)!}

Goal: B
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ transport-fixing (λ i₁ → p i₁) i0
         (transport (λ i₁ → sym p i₁) b)
i = i1 ⊢ b
Have: B
———— Boundary (actual) —————————————————————————————————————
i = i0 ⊢ transport-fixing (λ j → p j) i0
         (transport-fixing (λ j → sym p j) i0 b)
i = i1 ⊢ b
````

One key to this is that since we want it to be constant at `i = i1`, we specified the subcube formula `i`. And in the first transport, to make it only reduce to a constant at `i = i1`, an `∨ ` connective does this.

This wouldn't compile if we don't handle the second choice of intervals right at `(transport-fixing (λ j → (sym p) (~ i ∧ j))`. We were already expecting just `(sym p) j`, but we allow it to short at `i = i1` with `~ i ∧ _`.

Bit experimental, but I think I did it easier than last time.
