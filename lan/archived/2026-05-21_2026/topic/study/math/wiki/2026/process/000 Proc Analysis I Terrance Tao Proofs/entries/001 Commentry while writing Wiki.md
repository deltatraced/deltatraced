---
parent: '[[000 Proc Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Proc Analysis I Terrance Tao Proofs]]'
context_type: entry
---

Parent: [000 Proc Analysis I Terrance Tao Proofs](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Proc Analysis I Terrance Tao Proofs](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned in: [^spawn-entry-a451b5](../000%20Proc%20Analysis%20I%20Terrance%20Tao%20Proofs.md#spawn-entry-a451b5)

# 1 Journal

2026-04-16 Wk 16 Thu - 10:05 +03:00

`003 Syntax for dual implication composition (67346a)`  gives us syntax to reason about logical equivalences directly in proof. It would be cool to be able to directly prove the square equality from `6 ≡ 2` and `⊥ `, and transport `h₀` alongside that path of types directly to contradiction.

Since we are dealing with propositions, `↔` can be proven to be an equivalence, since by definition for a type to be a proposition, that any two terms of it must be equal, and then we are able to prove that each directed implication is the other's section. Then my dual-implication based proofs should be able to produce equalities by univalence in cubical agda if I decide to formalize them there.
