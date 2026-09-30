---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream#^spawn-entry-4aff11|^spawn-entry-4aff11]]

Prev: [[008 CQTS Solving Thought Stream 26 Wk 38]]
# Journal

2026-09-22 Wk 39 Tue - 10:42 +03:00

```
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
  
```

I made this template before, though it's kinda noisy for cube drawings. But it is made to give some space for the names.

This could be cleaner, with arrows still to help not get confused.

```
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
```

In some cases, we'll want to use single-letter names to keep things clean, although this can get harder later on when we're composing things, which was the context I made the first messier one in.

Added to [[000 CQTS Intro to Cubical Templates]]

2026-09-22 Wk 39 Tue - 11:50 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

This gives an error, I guess they're not immediately identical since they're hcomps of different tubes?

```
    _ = λ (p : a ≡ b) (q : b ≡ c) →
        test-identical (diamond p q) (diamond-alt p q)
```

```
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
```

2026-09-22 Wk 39 Tue - 21:42 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

```
    -- Now let's prove that p ∙ p̃ ≡ refl, ie composition with the inverse is
    -- refl. W  
e also need a cube for this.

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
```

Before I thought it was that the two faces has to be identical, but this shows it's not the case. If we leave out a face, we just have to make sure the shared line must compute to a composite of the missing face, that is all.

2026-09-23 Wk 39 Wed - 02:51 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

```haskell
module _
  {ℓ : (i : I) → Level}
  {A : (i : I) → Type (ℓ i)}
  where
    x : (φ : I) → A i0 → A i1
    x φ = transport-fixing {ℓ = ℓ} (λ i → A i) φ
```

gives the error

```
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda:48.37-46: error: [UnequalTerms]
The terms
  i
and
  i0
are not equal at type I
when checking that the expression
transport-fixing {ℓ = ℓ} (λ i → A i) φ has type A i0 → A i1
```

Although it looks like the types match.

2026-09-23 Wk 39 Wed - 08:43 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

```haskell
module _
  {A : I → Type}
  {m̬ : A i0 × A i0 → A i0}
  where
    _ = test-identical
        (transport (λ i → A i × A i → A i) m̬)
        λ (âl , âr) → {!!}
```

```haskell
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
```

I guess `Tele` here refers to a Telescope 0, the first argument in the lambda that I pattern matched to `(âl , âr)`.

2026-09-23 Wk 39 Wed - 10:36 +03:00

This does not work. The last time I've done it, I've used the Path <-> PathP maps directly.

```haskell
      p₁ : transport (λ i → A i) (a₁ i0) ≡ a₁ i1
      p₁ i = transport-fixing (λ j → A (i ∨ j)) i (a₁ i)
      p₂ : transport (λ i → A i) (a₀ i0) ≡ transport (λ i → A i) (a₁ i0)
      p₂ = ap (λ p' → transport (λ i → A i) p') p
      p₃ : transport (λ i → A i) (a₀ i0) ≡ a₀ i1
      p₃ =
        transport (λ i → A i) (a₀ i0) ≡⟨ p₂ ⟩ -- ↓
        transport (λ i → A i) (a₁ i0) ≡⟨ p₁ ⟩ -- ↓
        a₁ i1                         ≡⟨ sym (transport (λ i → a₀ i ≡ a₁ i) p)  ⟩
        a₀ i1                         ∎ -- ↑

      _ = test-identical
        (transport (λ i → a₀ i ≡ a₁ i) p)
        ((sym p₃) ∙∙ p₂ ∙∙ p₁)
```

```
?0 : A ≡ B → A ≃ B
?1 : path→equiv refl ≡ idEquiv A
_366
  : Test-Id-Family (transport (λ i → a₀ i ≡ a₁ i) p)
    (sym p₃ ∙∙ p₂ ∙∙
     p₁) [ at /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda:270.11-272.30 ]
———— Error —————————————————————————————————————————————————
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda:270.11-272.30: error: [InstanceNoCandidate]
No instance of type
Test-Id-Family (transport (λ i → a₀ i ≡ a₁ i) p)
(sym p₃ ∙∙ p₂ ∙∙ p₁)
was found in scope.
when checking that (sym p₃ ∙∙ p₂ ∙∙ p₁) is a valid argument to a
function of type
(a' : a₀ i1 ≡ a₁ i1)
⦃ _ : Test-Id-Family (transport (λ i → a₀ i ≡ a₁ i) p) a' ⦄ →
Test-Identical
```

[[000 CQTS Intro to Cubical Templates]]

But it's fine with this

```haskell
module _
  {A : I → Type}
  {a₀ : (j : I) → A j}
  {a₁ : (j : I) → A j}
  {p : a₀ i0 ≡ a₁ i0}
  where
    private
      p₁ : transport (λ i → A i) (a₁ i0) ≡ a₁ i1
      p₁ = PathP→Path (λ i → a₁ i)
      p₂ : transport (λ i → A i) (a₀ i0) ≡ transport (λ i → A i) (a₁ i0)
      p₂ = ap (λ p' → transport (λ i → A i) p') p
      p₃ : transport (λ i → A i) (a₀ i0) ≡ a₀ i1
      p₃ = PathP→Path (λ i → a₀ i)
	  
	  _ = test-identical (transport (λ i → a₀ i ≡ a₁ i) p)
                         ((sym p₃) ∙∙ p₂ ∙∙ p₁)

```

inspired by my more compressed previous solution:

```haskell
module _ {A : I → Type}
         {a₀ : (i : I) → A i}
         {a₁ : (i : I) → A i}
         {p : a₀ i0 ≡ a₁ i0}
  where
  _ = test-identical
      (transport (λ i → a₀ i ≡ a₁ i) p)
      -- Exercise:
      ((λ i → fromPathP (λ j → a₀ j) (~ i)) 
       ∙∙ (λ i → transport-fixing (λ j → A j) i0 (p i)) 
       ∙∙ λ i → fromPathP (λ j → a₁ j) i)
```

Maybe it's cross talking somehow since my composition is reusing the same paths in another composition from what I tried there. It was a redundant attempt.

When I solved

```haskell
p₁ i = transport-fixing (λ j → A (i ∨ j)) i (a₁ i)
```

I basically reinvented

```haskell
PathP→Path :
    PathP A a₀ a₁
  → Path (A i1) (transport (λ j → A j) a₀) a₁
PathP→Path p i = transport-fixing (λ j → (A (i ∨ j))) i (p i)
```
