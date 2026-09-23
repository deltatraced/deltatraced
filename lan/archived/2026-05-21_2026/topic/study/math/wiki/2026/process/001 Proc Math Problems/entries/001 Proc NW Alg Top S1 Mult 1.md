---
parent: '[[001 Proc Math Problems]]'
spawned_by: '[[001 Proc Math Problems]]'
context_type: entry
---

Parent: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned by: [001 Proc Math Problems](../001%20Proc%20Math%20Problems.md)

Spawned in: [^spawn-entry-c3e498](../001%20Proc%20Math%20Problems.md#spawn-entry-c3e498)

# 1 Journal

2026-04-27 Wk 18 Mon - 10:23 +03:00

In `001 Failed attempts while Defining Multiplication to satisfy Alg Top S1 Mult 1` `b236db`,

We need a definition that satisfies the property `∀ (h₁ h₂ : ℚ) → (e h₁) * (e h₂) ≡ (e h₃)`.

From [Two-dimensional surfaces: the sphere | Algebraic Topology 3 | NJ Wildberger](https://www.youtube.com/watch?v=R_gDV17X7pc&list=PLIljB45xT85D7wczwyUQdwDe2duZ7wPTf&index=3),

The professor mentioned complex multiplication, so this is likely what we need to use.

We know some facts about that:

````haskell
i² ≡ -1
∀ (a b : ℝ) → ∃ (A θ : ℝ) → (a + bi) ≡ A · e^ⁱθ
∀ (θ : ℝ) → e^ⁱθ ≡ cos θ + i sin θ
∀ (a b A θ : ℝ) → (a + bi) ≡ A · e^ⁱθ → a² + b² ≡ A
````
