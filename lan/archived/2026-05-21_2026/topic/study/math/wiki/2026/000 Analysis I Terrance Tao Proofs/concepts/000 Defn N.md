---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Notes for Posts]]'
context_type: concept
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md) ^22570d

Spawned by: [000 Spawn Logs for Notes for Posts](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md)

Spawned in: [^spawn-cncpt-5d5c76](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md#spawn-cncpt-5d5c76)

---

As guided by the textbook, we define the natural numbers `ℕ` as a set equipped with the following structure:

1. We have an element `0`. ^ax1

````haskell
ℕ-ax1 = ∃ (n : ℕ) → n ≡ 0
````

2. We have the following total function: ^ax2

````haskell
suc : ℕ → ℕ
````

and ℕ is closed under it:

````haskell
ℕ-ax2 = ∀ (a : ℕ) → ∃ (b : ℕ) → suc a ≡ b
````

Although `ℕ-ax2` should follow by definition of the totality of `suc` for us since we are writing in a type-theoretic style.

::Notation
We write 1 to mean `suc 0`, 2 to mean `suc 1`, 3 to mean `suc 2`, and so on. Rather than defining the decimal number system explicitly, we leave it as shorthand here.
:: ^notation1

3. To prevent wrap-around, we provide a proof that 0 has no successor: ^ax3

````haskell
ℕ-ax3 = ∀ (a : ℕ) → suc a ≠ 0
````

4. To prevent other wrap-arounds and ceiling behavior, we provide a proof that succ is  [injective](../../../../../../concept/math/concepts/2025/012%20injective.md): ^ax4

````haskell
ℕ-ax4 = ∀ (a b : ℕ) → suc a ≡ suc b → a ≡ b
````

Or by contrapositive:

````haskell
ℕ-ax4' = ∀ (a b : ℕ) → a ≠ b → suc a ≠ suc b
````

Not also that this follows since `a ≡ b` implies also `suc a ≡ suc b` , as they would not be equal if `suc` could differentiate them.

````haskell
ℕ-ax4'' = ∀ (a b : ℕ) → suc a ≡ suc b ↔ a ≡ b
````

To illustrate that this prevents wrap-back, see [six-not-two](../tasks/002%20six_not_2.md). The same logic applies for ceiling, if `1 ≡ 2`, then `suc 0 ≡ suc 1` then `0 ≡ 1`
