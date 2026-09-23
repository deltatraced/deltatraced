---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Analysis I Terrance Tao Proofs]]'
context_type: entry
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Spawn Logs for Analysis I Terrance Tao Proofs](000%20Spawn%20Logs%20for%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned in: [^spawn-entry-262fe8](000%20Spawn%20Logs%20for%20Analysis%20I%20Terrance%20Tao%20Proofs.md#spawn-entry-262fe8)

---

I will record my proofs going through the book here. I have already started this effort by formalization in Rocq, but the idea here is to write human proofs. I take inspiration however from how one writes proofs in proof assistants like Agda and Rocq, so I will attempt to replicate some aspects in my proof writing style here.

For example, induction on `ℕ` for some proposition `P₁ = ∀ (a : ℕ) → Q₁ a` is encoded by proving the case `P₁ 0 = ?` and then making use of `P₁ a` in the proof case `P₁ (suc a) = ? (P₁ a) ?`.

Proofs will be written as a linear list of cases, so notice that `a` is an inductive type and permits us to split the proof into cases `P₁ 0` and `P₁ (suc a)`. This pattern applies to many things, if you had a proof of the trichotomy of the integers

````haskell
H₀ = ∀ (z : ℤ) → (z ≡ ∃ (n : ℕ) → z ≡ -ℤ n) ∨ (z ≡ ℤ0) ∨ (∃ (n : ℕ) → z ≡ +ℤ n)
````

And I have to prove some property about the integers `P₂ = ∀ (a : ℤ) → Q₂ a` then since the above proof also applies for all integers, we may prove `P₂ = ∀ (a : ℤ) → H₀ a → Q₂ a` which grants us the ability to add the additional term `h₀ : H₀` into our context:

````
P₂ a h₀ = ?0
````

Since `h₀` is a disjoint union of multiple cases, we allow ourselves to handle each case individually:

````haskell
P₂ ?1 refl = ?0
P₂ ?1 refl = ?0
P₂ ?1 refl = ?0
````

Now we need something to fill in the holes `?1` such that the proof becomes reflexive. In the zero case, if `z` is `ℤ0`, then `ℤ0 ≡ ℤ0` is a reflexivity proof, so we have:

````haskell
P₂ ?1 refl = ?0
P₂ ℤ0 refl = ?0
P₂ ?1 refl = ?0
````

For the other `?1` holes, observe that the existence quantifier allows us to introduce a new natural number `n` into our context. Then to get the reflexivity proofs, it ought be of the form:

````haskell
P₂ (-ℤ n) refl = ?0
P₂ ℤ0     refl = ?0
P₂ (+ℤ n) refl = ?0
````

Notice that this allows us to split into further cases if we wanted, for example with `(+ℤ n)`:

````haskell
P₂ (-ℤ n)       refl = ?0
P₂ ℤ0           refl = ?0
P₂ (+ℤ 0)       refl = ?0
P₂ (+ℤ (suc n)) refl = ?0
````

If one uses `P₂ (+ℤ 0)` and `P₂ (+ℤ n)` in `P₂ (+ℤ (suc n))` then they are also applying natural number induction!
