---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream](007%20CQTS%20Solving%20Thought%20Stream.md)

Spawned in: [^spawn-entry-9fade4](007%20CQTS%20Solving%20Thought%20Stream.md#spawn-entry-9fade4)

# Journal

2026-09-29 Wk 40 Tue - 12:39 +03:00

In `/home/lan/src/cloned/gh/CQTS/introduction-to-cubical/lectures/2--Paths-and-Identifications/2-7--Propositions.lagda.md`,

````haskell
record _RetractOnto_ (A : Type ℓ) (B : Type ℓ') : Type (ℓ-max ℓ ℓ') where
  constructor retractOntoData
  field
    map : A → B
    section : SectionOf map

open _RetractOnto_ public

-- [modified to turn it to comment]
-- If `A` is equivalent to `B` then certainly `B` retracts onto `A`,
-- forgetting the other part of the equivalence.

equiv→retract : {A : Type ℓ} → {B : Type ℓ'} → (A ≃ B) → B RetractOnto A
equiv→retract e .map = e .proof .retract .map
equiv→retract e .section .map = e .map
equiv→retract e .section .proof = e .proof .retract .proof
````

I reversed this since it's kind of confusing. They mean that `B` retracts onto `A`, and the map here is in fact the retract map of an equivalence `A ≃ B`.

So if we consider both the statements `A ≃ B` and `B RetractOnto A`, the equivelance map of the first is `A → B` and the map of the second is `B → A` despite it written as `A → B` in the
definition of the record `_RetractOnto_`!

I've also changed the name `_RetractOnto_` into `_RetractsOnto_`. It just reads better. That's how I would read the operation, and how the authors did too in the comments up there.

We also keep the new convention of naming every record constructor `new{RecordName}`.

This is what I did instead:

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda
module _
  {ℓ₁ ℓ₂ : Level}
  where
    -- Now we want to prove the converse of isContr→≃⊤. If A is equivalent to ⊤,
    -- then A is contractible.
    
    -- It seems we only need half of the data of an equivalence for this.

    record _RetractsOnto_ (B : Type ℓ₁) (A : Type ℓ₂) : Type (ℓ-max ℓ₁ ℓ₂) where
      constructor newRetractsOnto
      field
        map : B → A
        section : HasSection map

    -- The map of `B RetractsOnto A` is `A ≃ B`'s retract map.
    -- The section map of `B RetractsOnto A` is `A ≃ B`'s equivalence map
    -- (f : A → B).

    -- since f is a section, it fits all of A into B. This means that
    -- size A ≤ size B.

    -- `B RetractsOnto A` then can be interpreted as A being a continuous shrinking
    -- of B.

    open _RetractsOnto_ public

module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {B : Type ℓ₂}
  where
    -- If A is equivalent to B, then B retracts onto A by forgetting part of the
    -- equivalence.
    equiv→retract : (A ≃ B) → B RetractsOnto A
    equiv→retract eab .map = eab .proof .retract .map
    equiv→retract eab .section .map = eab .map
    equiv→retract eab .section .proof = eab .proof .retract .proof
````

2026-09-29 Wk 40 Tue - 12:51 +03:00

Finally we reached `retract-≡`. This is where we stopped before.

[001 Proc CQTS retract equiv](001%20Proc%20CQTS%20retract%20equiv.md)

We will continue it here: [032 CQTS Problems Attempt 0 retract-≡](032%20CQTS%20Problems%20Attempt%200%20retract-%E2%89%A1.md)

2026-09-29 Wk 40 Tue - 14:32 +03:00

````
a̮ a̬
````

Seems I somehow typed two of these.

The first is just `a\_v` but not sure about the second.

2026-09-29 Wk 40 Tue - 15:12 +03:00

````haskell
Sq : Square A ŝ refl ŝ refl
Sq i k = ŝ (i ∨ k)
````

I can see the connection squares faster now! You can see this must be constant in the i1 for both `i` and `k`. What connection short-circuits in i1? `_∨_`!

2026-10-01 Wk 40 Thu - 18:30 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-7--Propositions.lagda.md`,

````
First, we have implication. If `A` and `B` are propositions then the
type of functions `A → B` is a proposition --- namely, the proposition
that `A` implies `B`.

isProp-→ : isProp B → isProp (A → B)
-- Exercise:
isProp-→ p f g i a = {!!}
````

But this only mentions `isProp B`, not `isProp A`? But the text above said if both are propositions...

Later they say

````
As a special case of implication, we find that type negation `¬ A` is
always a proposition even when `A` isn't.
````

So since this is by specialization of the prior theorem, then it follows that `A` does not have to be a proposition! It's a conceptual typo.

[006 CQTS Lecture Note Typos](006%20CQTS%20Lecture%20Note%20Typos.md)
