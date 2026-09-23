---
parent: '[[000 Proc Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Proc Analysis I Terrance Tao Proofs]]'
context_type: entry
---

Parent: [000 Proc Analysis I Terrance Tao Proofs](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Proc Analysis I Terrance Tao Proofs](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned in: [^spawn-entry-a8b5da](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md#spawn-entry-a8b5da)

# 1 Journal

2026-04-16 Wk 16 Thu - 02:57 +03:00

While writing how to prove in terms of the style of Agda, I wrote `∀ (a : ℕ) → P₁ a`

Then went on to talk about cases of this proof like `P₁ 0` and `P₁ (suc n)`. This is an error, because `P₁` doesn't by itself encode the expression that `"for all n, P₁ holds for n"`. The correction is to write `P₁ = ∀ (a : ℕ) → Q₁ a`. Now `Q₁` can be any proposition we intend to prove holds for all natural numbers, and `P₁` is the actual proof term that we can split on.

2026-04-16 Wk 16 Thu - 04:04 +03:00

If I'm proving something like

````
three-nat : ∃ (a : ℕ) → a ≡ (suc (suc (suc 0)))
````

We need a consistent interpretation of what is being asked. And constructively, an existential quantifier is encoded as a pair (a, p) where a is the element of choice, and p is the proof term that it satisfies `three-nat`.

2026-04-16 Wk 16 Thu - 06:07 +03:00

In proof `six-not-two` I wrote

 > 
 > The book proves this by contradiction. Since we haven't yet proven that equality on ℕ is decidable, to remain constructive we avoid proof by contradiction.

But actually we are trying to prove

````haskell
six-not-two : 6 ≠ 2
````

So it is by definition assuming `6 ≡ 2` and arriving at a contradiction. My statement would be correct if we instead assumed `~p` to arrive at `p` by negation.
