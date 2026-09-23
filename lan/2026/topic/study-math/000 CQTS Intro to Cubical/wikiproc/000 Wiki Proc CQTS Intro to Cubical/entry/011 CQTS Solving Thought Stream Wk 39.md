---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream](007%20CQTS%20Solving%20Thought%20Stream.md)

Spawned in: [^spawn-entry-4aff11](007%20CQTS%20Solving%20Thought%20Stream.md#spawn-entry-4aff11)

# Journal

2026-09-22 Wk 39 Tue - 10:42 +03:00

````
--                          ↓ [q......]
--               [......b] — — — — — — — — > [c......]
--                     / ^                 / ^
--      [......p] →  /   |               / ←-|-- [q......]
--                 /     |  ↓[p......] /     |
--       [......a] — — — — — — — > [b......] | 
--               ^       |           ^       |                    ^   j
--               |       |← [.......]|       | ← [.......]      k | /
--               |       |           |       |                    ∙ — >
--   [.......] → |       |[.......] →|       |                      i
--               |   [.......] — — — | — — > [.......]
--               |     / [.......] ↑ |     /
--   [.......] —-|-→ /               |   / ← [.......]
--               | /                 | /
--         [.......] — — — — — — —  [.......]
--                     ↑ [.......]
````

I made this template before, though it's kinda noisy for cube drawings. But it is made to give some space for the names.

This could be cleaner, with arrows still to help not get confused.

````
--                     ↓ ?
--                ? — — — — — — — — > ?
--              / ^                 / ^
--        ? → /   |               /   |
--          /     |             / ← ? |
--        ? — — — — — — — — > ?       |
--        ^       | ? ↑       ^       |                    ^   j
--        |   ? → |           |       | ← ?              k | /
--        |       |           |       |                    ∙ — >
--    ? → |       |    ↓ ?    |       |                      i
--        |       ? — — — — — | — — > ?
--        | ? → /         ? → |     /
--        |   /               |   / ← ?
--        | /                 | /
--        ? — — — — — — — — > ?
--                ↑ ?
````

In some cases, we'll want to use single-letter names to keep things clean, although this can get harder later on when we're composing things, which was the context I made the first messier one in.

Added to [000 CQTS Intro to Cubical Templates](../../../wiki/000%20Wiki%20CQTS%20Intro%20to%20Cubical/entry/000%20CQTS%20Intro%20to%20Cubical%20Templates.md)

2026-09-22 Wk 39 Tue - 11:50 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

This gives an error, I guess they're not immediately identical since they're hcomps of different tubes?

````
    _ = λ (p : a ≡ b) (q : b ≡ c) →
        test-identical (diamond p q) (diamond-alt p q)
````

````
_664
  : Test-Id-Family (diamond p q)
    (diamond-alt p
     q) [ at /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda:600.9-54 ]
———— Error —————————————————————————————————————————————————
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda:600.9-54: error: [InstanceNoCandidate]
No instance of type Test-Id-Family (diamond p q) (diamond-alt p q)
was found in scope.
when checking that (diamond-alt p q) is a valid argument to a
function of type
(a' : PathP (Square-sweep A p q) p q)
⦃ _ : Test-Id-Family (diamond p q) a' ⦄ →
Test-Identical
````

2026-09-22 Wk 39 Tue - 21:42 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

````
    -- Now let's prove that p ∙ p̃ ≡ refl, ie composition with the inverse is
    -- refl. We also need a cube for this.

    -- This time, we have the composite in (Line i̬k̂), so we will ignore
    -- (Face i̬) while verifying that (Line i̬k̂) has to be the composite of
    -- (Face i̬)'s lines.

    --                     ↓ —
    --                a — — — — — — — — > a
    --              / ^                 / ^
    --    p ∙ p̃ → /   |               /   |
    --          /     |             / ← — |
    --        a — — — — — — — — > a       |
    --        ^       | — ↑       ^       |                    ^   j
    --        |   p̃ → |           |       | ← —              k | /
    --        |       |           |       |                    ∙ — >
    --    — → |       |    ↓ p̃    |       |                      i
    --        |       b — — — — — | — — > a
    --        | p → /         — → |     /
    --        |   /               |   / ← —
    --        | /                 | /
    --        a — — — — — — — — > a
    --                ↑ —
````

Before I thought it was that the two faces has to be identical, but this shows it's not the case. If we leave out a face, we just have to make sure the shared line must compute to a composite of the missing face, that is all.

2026-09-23 Wk 39 Wed - 02:51 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

````haskell
module _
  {ℓ : (i : I) → Level}
  {A : (i : I) → Type (ℓ i)}
  where
    x : (φ : I) → A i0 → A i1
    x φ = transport-fixing {ℓ = ℓ} (λ i → A i) φ
````

gives the error

````
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda:48.37-46: error: [UnequalTerms]
The terms
  i
and
  i0
are not equal at type I
when checking that the expression
transport-fixing {ℓ = ℓ} (λ i → A i) φ has type A i0 → A i1
````

Although it looks like the types match.

2026-09-23 Wk 39 Wed - 08:43 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

````haskell
module _
  {A : I → Type}
  {m̬ : A i0 × A i0 → A i0}
  where
    _ = test-identical
        (transport (λ i → A i × A i → A i) m̬)
        λ (âl , âr) → {!!}
````

````haskell
Goal: A i1
———— Context ———————————————————————————————————————————————
âr : A i1
âr = .patternInTele0 .snd
âl : A i1
âl = .patternInTele0 .fst
.patternInTele0
    : A i1 × A i1   (not in scope)
m̬  : A i0 × A i0 → A i0
A   : I → Type
````

I guess `Tele` here refers to a Telescope 0, the first argument in the lambda that I pattern matched to `(âl , âr)`.
