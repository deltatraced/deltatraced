---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Analysis I Terrance Tao Proofs]]'
context_type: entry
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Spawn Logs for Analysis I Terrance Tao Proofs](000%20Spawn%20Logs%20for%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned in: [^spawn-entry-67346a](000%20Spawn%20Logs%20for%20Analysis%20I%20Terrance%20Tao%20Proofs.md#spawn-entry-67346a)

---

As used in the proof [002 six_not_2](../tasks/002%20six_not_2.md), we have a proof that looks like

````haskell
six-not-two : 6 ≠ 2
six-not-two (h₀ : 6 ≡ 2) = (p₀ .fst) h₀
  where
    p₀ : (6 ≡ 2) ↔ ⊥
    p₀ = ((    6) ≡ (    2)) ↔⟨ ↔-refl {- unfold 6, 2 -} ⟩ 
         ((suc 5) ≡ (suc 1)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    5) ≡ (    1)) ↔⟨ ↔-refl {- unfold 5, 1 -} ⟩ 
         ((suc 4) ≡ (suc 0)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    4) ≡ (    0)) ↔⟨ ↔-refl {- unfold 4 -} ⟩ 
         ((suc 3) ≡ (    0)) ↔⟨ ↔-⊥ ℕ-ax3 3 ⟩ 
         ⊥                   ∎↔
````

`↔-refl` should encode the expectation that having a term implies we have it. The reason it is there is for elaboration reasons, for things that hold by definition.

`↔-⊥` should take maps into ⊥ and produce maps into and out of ⊥ to be compatible with this syntax.

`∎↔` concludes the proof, by showing that the term implies itself.

The `↔⟨ ⟩` syntax is inspired by the path composition equality syntax used in https://cqts.github.io/introduction-to-cubical.

`(p₀ .fst)` allows us to pick the first implication in `(6 ≡ 2) ↔ ⊥` which is definitionally equal to `((6 ≡ 2) → ⊥) × (⊥ → (6 ≡ 2))`.

So `(p₀ .fst)` takes in `h₀` and is computationally equivalent to a chaining of `→` maps at each step that transform it into `⊥ `.
