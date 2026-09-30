---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries#^spawn-entry-69bf37|^spawn-entry-69bf37]]

# Journal

2026-09-27 Wk 39 Sun - 05:06 +03:00

```
--                            p   
--                      a  — — — >  a        
--                      ^           ^                           ^
-- iterateⁿ p (predℤ b) |     A     | iterateⁿ p b            j |
--                      |           |                           . — >
--                      a  — — — >  a                             i
--                            —   
```

To help illustrate what this is pointing to, I drew this:

```
--                                
--                                  a        
--                           p    / ^                            
--                          →  →    |                            
--                       /          |                                
--                      a  — — — >  a        
--                      ^           ^                            
--                      |           |                            
--                      |           |                                
--                      a  — — — >  a        
--                      ^           ^                           ^
-- iterateⁿ p (predℤ b) |           | iterateⁿ p b            j |
--                      |           |                           . — >
--                      a  — — — >  a                             i
--                            —   
```

Since one is one more iteration than the other (ie one more composition with p).

Although b can be negative, so the previous would be the higher tower in that case, followed by a `(sym p)` to go down.

2026-09-27 Wk 39 Sun - 07:28 +03:00

```
--                     ↓ loop
--                ∙ — — — — — — — — > ∙
--              / ^                 / ^
--  loopⁿ b → /   |   S¹          /   |
--          /     |             / ← --|-- loopⁿ b
--        ∙ — — — — — — — — > ∙       |
--        ^       | — ↑       ^       |                    ^   j
--        |   — → |           |       | ← ?              k | /
--        |       |           |       |                    ∙ — >
--    — → |       |    ↓ loop |       |                      i
--        |       ∙ — — — — — | — — > ∙
--        |p₁ → /         — → |     /
--        |   /               |   / ← loopⁿ b
--        | /                 | /
--        ∙ — — — — — — — — > ∙
--                ↑ —
-- p₁ = loopⁿ (predℤ (sucℤ b))
-- ∙ = base
```

We have to make some guesses with this square.

For example I set `(Line îj̬)` to refl (`—`) so that we can solve `(Face j̬)` as a base const square.

I'm also setting `(Line îĵ)` to `—` to get a easy squares for faces `(Face î)` and `(Face ĵ))`.

2026-09-27 Wk 39 Sun - 07:41 +03:00

Since we want `(Face î)` to have refl left and right, we need a square like this:

```haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ â : A}
  {p : a̬ ≡ â}
  where
    Sq : Square A refl refl p p
    Sq i j = p i
```

When the boundaries test (i = i0) and (i = i1), the vertical slices we expect to be refl, it will resolve to a constant, as expected, otherwise it would be `p i` at the `(j = 0)` and `(j = 1)` face boundary conditions.

Similarly, for the face `(j = i1)`, we want the rule `loop i` so that at the face boundary tests `(i = i0)` and `(i = i1)` it resolves to constant, and remains for the `(k = i0)` and `(k = i1)` face boundary checks.

Asking what faces become constant for the rule we specify helped us understand the connection squares, and now also the squares that ignore a dimension, since focusing on giving a rule that must resolve to a constant at those face checks where one interval is set to a constant usually gets us to the desired rule that produces these simple squares.

```
--                          loop  
--                      a  — — — >  a        
--                      ^           ^                           ^
--                    — |    S¹     | —                       k |
--                      |           |                           . — >
--                      a  — — — >  a                             i
--                          loop
```

So `loop i` resolves to a constant path at `i` face checks, but also `loop i` cannot be reduced further at `k` face checks, and hence we also `loop`, which varies alongside the `i` dimension.

So I could also say that `loop` is extended across `i` in the `k` face checks, suggesting the rule `loop i`, just like first checking for the constants, where in the i face checks, using `i` should reduce the path to a constant.

2026-09-29 Wk 40 Tue - 07:33 +03:00

```haskell
    -- This can be solved with J
    to-fro : (decode base) hasSection (encode base)
    to-fro p = h1
      where
        to-fro-refl : (loopⁿ (encode base (λ _ → base))) ≡ (λ _ → base)
        to-fro-refl i j = base

        Q₀' : (s : S¹) → (q : base ≡ s) → Type
        Q₀' s q = {!!}

        Q₀ : (s : S¹) → (q : base ≡ s) → Type
        Q₀ base q = loopⁿ (encode base q) ≡ q
        Q₀ (loop i) q = {!loopⁿ _!}

        h0 : (p : base ≡ base) → (loopⁿ (encode base p)) ≡ p
        h0 p = {!J (λ (s : S¹) (q : base ≡ s) → loopⁿ _ ≡ q) to-fro-refl p   !}
        -- h0 i = J {!Q₁!} to-fro-refl p i

        Q₁' : (s : S¹) → (q : base ≡ s) → Type
        Q₁' s q = {! !}
          where
            c1 : code s
            c1 = transport-fixing (λ i → code (q i)) i0 (pos 0)
            c2 : code s
            c2 = encode s q
        -- Q₁' s q = {!(iterateⁿ loop (transport-fixing (λ i → code s) i0 (pos 0))) ≡ q!}

        Q₁ : (s : S¹) → (q : base ≡ s) → Type
        Q₁ base q = (loopⁿ (transport-fixing (λ i → code base) i0 (pos 0))) ≡ q
        Q₁ (loop i) q = {!!}

        Q₂' : (s : S¹) → (q : base ≡ base) → Type
        Q₂' s q = loopⁿ (encode base q) ≡ q

        h1-refl : (loopⁿ (transport-fixing (λ i → code base) i0 (pos 0))) ≡ refl
        h1-refl = refl

        h1 : (loopⁿ (transport-fixing (λ i → code (p i)) i0 (pos 0))) ≡ p
        h1 i j = {!J (λ s q → (decode s) _) to-fro-refl p!}

        h2 : (p : base ≡ base) → (decode base) ((encode base) p) ≡ p
        h2 p = J (λ s q → (decode s) ((encode s) q) ≡ q) to-fro-refl p
```

Got a hint from my past solution to keep `decode` in.

I tried different things, but I forgot that I shouldn't reduce `(decode base)` to `loopⁿ`. I kept focusing on how I could express `encode` for `J` while working with just `loopⁿ`.

Cleaned up:

```haskell
    -- This can be solved with J
    to-fro : (decode base) hasSection (encode base)
    to-fro p = h0 p
      where
        to-fro-refl : (loopⁿ (encode base (λ _ → base))) ≡ (λ _ → base)
        to-fro-refl i j = base

        h0 : (p : base ≡ base) → (decode base) ((encode base) p) ≡ p
        h0 p = J (λ s q → (decode s) ((encode s) q) ≡ q) to-fro-refl p
```
