---
parent: "[[000 Wiki Proc introduction-to-cubical archive]]"
spawned_by: "[[000 Wiki Proc introduction-to-cubical archive]]"
context_type: entry
---

Parent: [[000 Wiki Proc introduction-to-cubical archive]]

Spawned by: [[000 Wiki Proc introduction-to-cubical archive]]

Spawned in: [[000 Wiki Proc introduction-to-cubical archive#^spawn-entry-a22647|^spawn-entry-a22647]]

# 1 Journal

2026-05-06 Wk 19 Wed - 20:13 +03:00

This is as far as I got right now:

```haskell
retract-≡ : (r : B RetractOnto A)
  → {x y : A}
  → (r .section .map x ≡ r .section .map y) RetractOnto (x ≡ y)
-- Exercise: (Hint: Using `∙∙` gives the cleanest argument!)
retract-≡ r {x} {y} .map q = 
                          x    ≡⟨ sym (((r .section) .proof) x) ⟩
  r .map (r .section .map x)   ≡⟨ ap (λ a → r .map a) q ⟩
  r .map (r .section .map y)   ≡⟨ ((r .section) .proof) y ⟩
                          y    ∎
retract-≡ r {x} {y} .section .map p = ap (λ a → r .section .map a) p
retract-≡ r {x} {y} .section .proof p i j = {!  !}
```

```haskell
{- Hole Information -}

Goal and Context

Agda v2.8.0

- Goal
A ———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ x
j = i1 ⊢ y
i = i0 ⊢ step-≡ x (step-≡ (r .map (r .section .map x)) (step-≡ (r .map (r .section .map y)) (y ∎) (r .section .proof y)) (ap (r .map) (ap (r .section .map) p))) (sym (r .section .proof x)) j
i = i1 ⊢ p j

- j : I
- i : I
- p : x ≡ y
- y : A
- x : A
- r : B RetractOnto A
- A : Type A.ℓ (not in scope)
- A.ℓ : Level (not in scope)
- B : Type B.ℓ (not in scope)
- B.ℓ : Level (not in scope)
```

Unsure how I can go with simplifying those `step-≡` terms though.

```haskell
retract-≡ r {x} {y} .section .proof p  = {!  !}
    Sq₀-faces : (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
    Sq₀-faces i j k (i = i0) = face j k
      where
        face : Square (((r .section) .proof) x) (((r .section) .proof) y) (ap (r .map) (ap (r .section .map) p))
                      (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
        face = flip-square (∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y))
    Sq₀-faces i j k (j = i0) = face i k
      where
        face : Square (((r .section) .proof) x) refl (((r .section) .proof) x) refl
        face i k = (r .section . proof x) (i ∨ k)
    Sq₀-faces i j k (j = i1) = face i k
      where
        face : Square (((r .section) .proof) y) refl (((r .section) .proof) y) refl
        face i k = (r .section . proof y) (i ∨ k)
    Sq₀-faces i j k (k = i0) = face i j
      where
        face : Square  (ap (r .map) (ap (r .section .map) p)) 
                       (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                       (((r .section) .proof) x) (((r .section) .proof) y)
        face = ∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y)
    Sq₀ : Square (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                 p refl refl
    Sq₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}```

```haskell
{- Hole Information -}

step-≡ x (step-≡ (r .map (r .section .map x)) (step-≡ (r .map (r .section .map y)) (y ∎) (r .section .proof y)) (ap (r .map) (ap (r .section .map) p))) (sym (r .section .proof x)) ≡ p
```

We do need a square for this, and the hint mentioned     Sq₀-faces : (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
    Sq₀-faces i j k (i = i0) = face j k
      where
        face : Square (((r .section) .proof) x) (((r .section) .proof) y) (ap (r .map) (ap (r .section .map) p))
                      (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
        face = flip-square (∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y))
    Sq₀-faces i j k (j = i0) = face i k
      where
        face : Square (((r .section) .proof) x) refl (((r .section) .proof) x) refl
        face i k = (r .section . proof x) (i ∨ k)
    Sq₀-faces i j k (j = i1) = face i k
      where
        face : Square (((r .section) .proof) y) refl (((r .section) .proof) y) refl
        face i k = (r .section . proof y) (i ∨ k)
    Sq₀-faces i j k (k = i0) = face i j
      where
        face : Square  (ap (r .map) (ap (r .section .map) p)) 
                       (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                       (((r .section) .proof) x) (((r .section) .proof) y)
        face = ∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y)
    Sq₀ : Square (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                 p refl refl
    Sq₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}a cube...  Maybe we will need to make it via hcomp?

2026-05-06 Wk 19 Wed - 20:42 +03:00

We can rewrite the path we derived via chaining in terms of `∙∙`:

```haskell
retract-≡ r {x} {y} .map q = (sym (((r .section) .proof) x)) ∙∙ (ap (λ a → r .map a) q) ∙∙ (((r .section) .proof) y)
  --                         x    ≡⟨ sym (((r .section) .proof) x) ⟩
  -- r .map (r .section .map x)   ≡⟨ ap (λ a → r .map a) q ⟩
  -- r .map (r .section .map y)   ≡⟨ ((r .section) .proof) y ⟩
  --                         y    ∎
```

2026-05-06 Wk 19 Wed - 22:42 +03:00

Maybe I could somehow form this square by horziontal square composition, but then I ran into a blocker because the points don't quite line up when you remove any of the terms in `∙∙`

```haskell
    Sq₀ : Square p (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y) refl refl
    Sq₀ = {!!}
```

2026-05-06 Wk 19 Wed - 23:27 +03:00

`(r .section .map x)` is of type `B`, so their `r (s x)` in their cube

```

                y — — — — — — — — > y
              / ^                 / ^
      .map  /   |            p  /   |
          /     |             /     |
        x — — — — — — — — > x       |
        ^       |           ^       |                    ^   j
        |       |           |       |                  k | /
        |       |           |       |                    ∙ — >
        |       |           |       |                      i
        |    r (s y)  — — — | — — > y
        |     /             |     /
        |   /               |   /
        | /                 | /
     r (s x) — — — — — — —  x
```

likely meant `r .map (r .section .map x)`

2026-05-06 Wk 19 Wed - 23:40 +03:00

Here's a possible cube we can use to construct the needed upper square that can complete the proof:

```
                            ↓ refl
                      y — — — — — — — — > y
                    / ^                 / ^
 (A ∙∙ B ∙∙ C) →  /   |           p → /   |
                /     |    ↓ refl   /     |
              x — — — — — — — — > x       |
              ^       |           ^       |                    ^   j
              |       | ← C       |refl → |                  k | /
              |       |    refl → |       |                    ∙ — >
              |       |           |       |                      i
    (sym A) → |    r (s y)  — — — | — — > y
              | B → /       ↑ C   |     /
              |   /               |   / ← p
              | /                 | /
          r (s x) — — — — — — —  x
                    ↑ (sym A)
```


where

```
A = (sym (((r .section) .proof) x))
B = ap (r .map) (ap (r .section .map) p)
C = (((r .section) .proof) y)
```

2026-05-07 Wk 19 Thu - 00:50 +03:00

```haskell
        face : Square (    (    (((r .section) .proof) x))) (((r .section) .proof) y) (ap (r .map) (ap (r .section .map) p))
                      (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
        face = flip-square (∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y))
```

and

```haskell
        face : Square (sym (sym (((r .section) .proof) x))) (((r .section) .proof) y) (ap (r .map) (ap (r .section .map) p))
                      (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
        face = flip-square (∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y))
```

both work, because `(sym (sym _))` and `_` are computationally equal!

2026-05-07 Wk 19 Thu - 01:12 +03:00

```
                            ↓ refl
                      y — — — — — — — — > y
                    / ^                 / ^
 (A ∙∙ B ∙∙ C) →  /   |           p → /   |
                /     |    ↓ refl   /     |
              x — — — — — — — — > x       |
              ^       |           ^       |                    ^   j
              |       | ← C       |refl → |                  k | /
              |       |    refl → |       |                    ∙ — >
              |       |           |       |                      i
    (sym A) → |    r (s y)  — — — | — — > y
              | B → /       ↑ C   |     /
              |   /               |   / ← p
              | /                 | /
          r (s x) — — — — — — —  x
                    ↑ (sym A)
```

We need to update this. The face `(k = i0)` is difficult to produce, but It should be easier to change that `p` to `(A ∙∙ B ∙∙ C)`, then we should be able to skip specifying the face `(i = i1)` because it's exactly like the face `(k = i1)` we want to fill, and the face `(k = i0)` should be a similar `∙∙-filler` to face `(i = i0)`:

```
                            ↓ refl
                      y — — — — — — — — > y
                    / ^                 / ^
 (A ∙∙ B ∙∙ C) →  /   |           p → /   |
                /     |    ↓ refl   /     |
              x — — — — — — — — > x       |
              ^       |           ^       |                    ^   j
              |       | ← C       |refl → |                  k | /
              |       |    refl → |       |                    ∙ — >
              |       |           |       |                      i
    (sym A) → |    r (s y)  — — — | — — > y
              | B → /       ↑ C   |     /
              |   /               |   / ← (A ∙∙ B ∙∙ C)
              | /                 | /
          r (s x) — — — — — — —  x
                    ↑ (sym A)
```

2026-05-07 Wk 19 Thu - 01:49 +03:00

This change did cause issues though. No longer does this refine:

```haskell
    Sq₀ : Square (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y) p refl refl
    Sq₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}
```

```
Goal and Context

Agda v2.8.0

- Goal

A ———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ x
j = i1 ⊢ y
i = i0 ⊢ hcomp (j ∨ ~ j) (double-comp-box (λ i₂ → r .section .proof x (~ i₂)) (λ i₂ → r .map (r .section .map (p i₂))) (r .section .proof y) j)
i = i1 ⊢ p j

- Have

A ———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ x
j = i1 ⊢ y
i = i0 ⊢ hcomp (j ∨ ~ j) (double-comp-box (λ i₂ → r .section .proof x (~ i₂)) (λ i₂ → r .map (r .section .map (p i₂))) (r .section .proof y) j)
i = i1 ⊢ hcomp (j ∨ ~ j) (Sq₀-faces i1 j)
```

Everything matches except for `(i = i1)`.

2026-05-07 Wk 19 Thu - 20:39 +03:00

```
                            ↓ refl
                      y — — — — — — — — > y
                    / ^                 / ^
 (A ∙∙ B ∙∙ C) →  /   |           p → /   |
                /     |    ↓ refl   /     |
              x — — — — — — — — > x       |
              ^       |           ^       |                    ^   j
              |       | ← C       |refl → |                  k | /
              |       |    refl → |       |                    ∙ — >
              |       |           |       |                      i
    (sym A) → |    r (s y)  — — — | — — > y
              | B → /       ↑ C   |     /
              |   /               |   / ← (A ∙∙ B ∙∙ C)
              | /                 | /
          r (s x) — — — — — — —  x
                    ↑ (sym A)

where
  A = (sym (((r .section) .proof) x))
  B = ap (r .map) (ap (r .section .map) p)
  C = (((r .section) .proof) y)
```

```haskell
    Sq₀-faces : (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
    Sq₀-faces i j k (i = i0) = face j k
      where
        face : Square (((r .section) .proof) x) (((r .section) .proof) y) (ap (r .map) (ap (r .section .map) p))
                      (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
        face = flip-square (∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y))
    Sq₀-faces i j k (j = i0) = face i k
      where
        face : Square (((r .section) .proof) x) refl (((r .section) .proof) x) refl
        face i k = (r .section . proof x) (i ∨ k)
    Sq₀-faces i j k (j = i1) = face i k
      where
        face : Square (((r .section) .proof) y) refl 
(((r .section) .proof) y) refl
        face i k = (r .section . proof y) (i ∨ k)
    Sq₀-faces i j k (k = i0) = face i j
      where
        face : Square  (ap (r .map) (ap (r .section .map) p)) 
                       (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                       (((r .section) .proof) x) (((r .section) .proof) y)
        face = ∙∙-filler (sym (r .section .proof x)) (ap (r .map) (ap (r .section .map) p)) (r .section .proof y)
    Sq₀ : Square (sym (r .section .proof x) ∙∙ ap (r .map) (ap (r .section .map) p) ∙∙ r .section .proof y)
                 p refl refl
    Sq₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}
```

The issue still persists. Maybe we can modify the order of the double compose and in the top face and see if this helps.

```
                            ↓ refl
                      y — — — — — — — — > y
                    / ^                 / ^
             p →  /   |               / ←-|-- (A ∙∙ B ∙∙ C)
                /     |    ↓ refl   /     |
              x — — — — — — — — > x       |
              ^       |           ^       |                    ^   j
              |       | ← C       |refl → |                  k | /
              |       |    refl → |       |                    ∙ — >
              |       |           |       |                      i
    (sym A) → |    r (s y)  — — — | — — > y
              | B → /       ↑ C   |     /
              |   /               |   / ← (A ∙∙ B ∙∙ C)
              | /                 | /
          r (s x) — — — — — — —  x
                    ↑ (sym A)
```

We would still need to figure out face (i = i0) which we stumbled on before.

Maybe let's create a cube just to construct that face.

Here's a template:

```
                        — — — — — — — — >  
                    / ^                 / ^
                  /   |               /   |                
                /     |             /     |
                  — — — — — — — >         |
              ^       |           ^       |                    ^   j
              |       |           |       |                  k | /
              |       |           |       |                    ∙ — >
              |       |           |       |                      i
              |         — — — — — | — — >  
              |     /             |     /
              |   /               |   /                
              | /                 | /
                  — — — — — — —   
```

And the current square we want to fill:

```
                            ↓p    
                      x — — — — — — — — > y
                    / ^                 / ^
        (sym A) → /   |           C → /   |                
                /     |   ↓B        /     |
          r (s x) — — — — — — — > r (s y) |
              ^       |           ^       |                    ^   j
              |       |           |       |                  k | /
              |       |           |       |                    ∙ — >
              |       |           |       |                      i
              |         — — — — — | — — >  
              |     /             |     /
              |   /               |   /                
              | /                 | /
                  — — — — — — —   
```

This is one possible configuration to try, although it has the same feature of requiring `i = i1` to be similar to `k = i1` so it might not work:

```
                            ↓p    
                 
     				  x — — — — — — — — > y
                    / ^                 / ^
        (sym A) → /   |           C → /   |                
                /     |   ↓B        /     |
          r (s x) — — — — — — — > r (s y) |
              ^       |           ^       | ← p                ^   j
              |refl → |           |       |                  k | /
              |       |           |       |                    ∙ — >
              |       |   ↓ refl  | ← B   |                      i
       refl → |       x — — — — — | — — > x
              |     /             |     /
              |   / ← (sym A)     |   / ← (sym A)             
              | /                 | /
          r (s x) — — — — — — — r (s x)
                      ↑ refl       
```

2026-05-07 Wk 19 Thu - 22:17 +03:00

Yup it's the same problem.

```haskell
    pSymA = (((r .section) .proof) x)
    pB = ap (r .map) (ap (r .section .map) p)
    pC = (((r .section) .proof) y)

    Sq₁-faces : (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
    Sq₁-faces i j k (i = i0) = face j k
      where
        face : Square refl refl pSymA pSymA
        face j k = pSymA j
    Sq₁-faces i j k (j = i0) = face j k
      where
        face : Square refl pB refl pB
        face j k = pB (j ∧ k)
    Sq₁-faces i j k (j = i1) = face i k
      where
        face : Square refl p refl p
        face i k =
 p (i ∧ k)
    Sq₁-faces i j k (k = i0) = face i j
      where
        face : Square pSymA pSymA refl refl
        face i j = pSymA j

    Sq₁ : Square pB p pSymA pC
    Sq₁ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₁-faces i j)!}
```

```
- A ———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ r .section .proof x i
j = i1 ⊢ r .section .proof y i
i = i0 ⊢ r .map (r .section .map (p j))
i = i1 ⊢ p j
    
- Have
A ———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ r .map (r .section .map x)
j = i1 ⊢ p i
i = i0 ⊢ r .section .proof x j
i = i1 ⊢ hcomp (j ∨ ~ j) (Sq₁-faces i1 j)
```

This might be related to `B` and `p` being in reverse orientation, so the squares aren't actually identical in `(i = i1)`.  The square `(i = i1)` is direct down to up, while `(k = i1)` is left to right, so if we try to match them it turns out that the `B` and `p` sides switch. But also we can't just switch the points because we will get `A` instead of `(sym A)` and it will again not be the identical square. And we know that cubical agda allows its omission and does fill it with an hcomp, it just doesn't computationally reduce to what we want.


2026-05-08 Wk 19 Fri - 14:54 +03:00

```
    pSymA = (((r .section) .proof) x)
    pB = ap (r .map) (ap (r .section .map) p)
    pC = (((r .section) .proof) y)

    Sq₁-faces : (i j k : I) → Partial (∂ i ∨ (~ j) ∨ (~ k)) A
    Sq₁-faces i j k (i = i0) = face j k
      where
        face : Square refl pSymA refl pSymA
        face j k = {!!}
    Sq₁-faces i j k (i = i1) = face j k
      where
        face : Square refl pC refl pC
        face j k = {!!}
    Sq₁-faces i j k (j = i0) = face i k
      where
        face : Square refl refl pB pB
        face j k = {!!}
    Sq₁-faces i j k (k = i0) = face i j
      where
        face : Square refl refl pB pB
        face i j = {!!}

    Sq₁ : Square pSymA pC pB p
    Sq₁ i j = {!hcomp (∂ i ∨ (~ j)) (Sq₁-faces i j)!}
```

Same issue here.