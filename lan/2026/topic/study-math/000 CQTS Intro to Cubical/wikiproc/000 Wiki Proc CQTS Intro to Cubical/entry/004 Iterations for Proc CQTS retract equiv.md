---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv](001%20Proc%20CQTS%20retract%20equiv.md)

Spawned in: [^spawn-entry-9534d2](001%20Proc%20CQTS%20retract%20equiv.md#spawn-entry-9534d2)

# Journal

## Iteration 1

### Iteration 1.0 Work I had from before

Before I change too much:

````haskell
(Me)
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

where
  A = (sym (((r .section) .proof) x))
  B = ap (r .map) (ap (r .section .map) p)
  C = (((r .section) .proof) y)
(/Me)

                            ↓p    
                      x — — — — — — — — > y
                    / ^                 / ^
        (sym A) → /   |           C → /   |                
                /     |   ↓B        /     |
          r (s x) — — — — — — — > r (s y) |
              ^       |           ^       | ← C                ^   j
              |       | ← (sym A) |       |                  k | /
              |       |           |       |                    ∙ — >
              |       |     ↓ B   |       |                      i
       refl → | r (s x) — — — — — | — — > r (s y)
              |     /             |     /
              |   / ← refl refl → |   / ← refl
              | /                 | /
          r (s x) — — — — — — — r (s y)
                      ↑ B
                                   

$$$
retract-≡ : {ℓ₀ : Level} → {A B : Type ℓ₀} → (r : B RetractOnto A)
  → {x y : A}
  → (r .section .map x ≡ r .section .map y) RetractOnto (x ≡ y)
-- Exercise: (Hint: Using `∙∙` gives the cleanest argument!)
retract-≡ r {x} {y} .map q = (sym (((r .section) .proof) x)) ∙∙ (ap (λ a → r .map a) q) ∙∙ (((r .section) .proof) y)
  --                         x    ≡⟨ sym (((r .section) .proof) x) ⟩
  -- r .map (r .section .map x)   ≡⟨ ap (λ a → r .map a) q ⟩
  -- r .map (r .section .map y)   ≡⟨ ((r .section) .proof) y ⟩
  --                         y    ∎
retract-≡ r {x} {y} .section .map p = ap (λ a → r .section .map a) p
-- retract-≡ {A = A} {B = B} r {x} {y} .section .proof p i = Sq₀ (~ i)
retract-≡ {A = A} {B = B} r {x} {y} .section .proof p = H₀ p
  where
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

    -- Sq₁ : Square pSymA pC pB p
    Sq₁ : Square pSymA pC pB (λ i → hcomp (∂ i ∨ ~ i1) (Sq₁-faces i i1))
    Sq₁ i j = hcomp (∂ i ∨ (~ j)) (Sq₁-faces i j)
    Sq₂ : Square pSymA pC pB p
    Sq₂ i j = {!Sq₁ (~ i) j !}

    Sq₀-faces : (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
    Sq₀-faces i j k (i = i0) = face j k
      where
        face : Square pSymA pC (pSymA ∙∙ p ∙∙ (sym pC)) p
        face = {!!}
    Sq₀-faces i j k (j = i0) = face i k
      where
        face : Square pSymA refl pSymA refl
        face i k = pSymA (i ∨ k)
    Sq₀-faces i j k (j = i1) = face i k
      where
        face : Square pC refl pC refl
        face i k = pC (i ∨ k)
    Sq₀-faces i j k (k = i0) = face i j
      where
        face : Square (pSymA ∙∙ p ∙∙ (sym pC)) p pSymA pC
        face = {!!}

    Sq₀ : Square p ((sym pSymA) ∙∙ pB ∙∙ pC) refl refl
    Sq₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}

    Sq₀c : Square p ((sym pSymA) ∙∙ pB ∙∙ pC) refl refl
    Sq₀c i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)!}
    -- Sq₀ : Square ((sym pSymA) ∙∙ pB ∙∙ pC) (λ i₁ → hcomp (~ i1 ∨ ∂ i₁) (Sq₀-faces i1 i₁)) refl refl
    -- Sq₀ i j = hcomp ((~ i) ∨ ∂ j) (Sq₀-faces i j)
    H₀ : (p : x ≡ y) → (retract-≡ r .map) ((retract-≡ r .section .map) p) ≡ p
    H₀ p = λ i j → {!   !}
$$$
````

    Sq₀₀-faces i j k (i = i0) = face j k
      where
        face : Square (sp x) (sp y) (pB p) (p₁ p)
    Sq₀₀ : Square p₁ p refl refl
    Sq₀₀ i j = {!hcomp ((~ i) ∨ ∂ j) (Sq₀₀-faces i j) !}### Iteration 1.0.1 Retained
    

### Iteration 1.1 Reset

Resetting back to this workspace:

````haskell
retract-≡ : {ℓ₀ : Level} → {A B : Type ℓ₀} → (r : B RetractOnto A)
  → {x y : A}
  → (r .section .map x ≡ r .section .map y) RetractOnto (x ≡ y)
-- Exercise: (Hint: Using `∙∙` gives the cleanest argument!)
retract-≡ r {x} {y} .map q = (sym (((r .section) .proof) x)) ∙∙ (ap (λ a → r .map a) q) ∙∙ (((r .section) .proof) y)
  --                         x    ≡⟨ sym (((r .section) .proof) x) ⟩
  -- r .map (r .section .map x)   ≡⟨ ap (λ a → r .map a) q ⟩
  -- r .map (r .section .map y)   ≡⟨ ((r .section) .proof) y ⟩
  --                         y    ∎
retract-≡ r {x} {y} .section .map p = ap (λ a → r .section .map a) p
retract-≡ {A = A} {B = B} r {x} {y} .section .proof p = H₀ p
  where
    pSymA = ((r .section .proof) x)
    pB = ap (r .map) (ap (r .section .map) p)
    pC = ((r .section .proof) y)

    H₀ : (p : x ≡ y) → (retract-≡ r .map) ((retract-≡ r .section .map) p) ≡ p
    H₀ p = λ i j → {!   !}
````

## Iteration 2

### Iteration 2.1  Cube Drawing Search

Template

````
                         ↓ [.......]
              [.......] — — — — — — — — > [.......]
                    / ^                 / ^
     [.......] →  /   |               / ←-|-- [.......]
                /     |  ↓[.......] /     |                  _________________________________ 
      [.......] — — — — — — — > [.......] |                 |                    [Goal00]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       |← [.......]|       | ← [.......]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [.....]  |
              |       |[.......] →|       |                 |    i             / |            |
  [.......] → |   [.......] — — — | — — > [.......]         |          [.....]   [.....]      |
   [.......] -|---→ / [.......] ↑ |     /                   |_________________________________|
              |   /               |   / ← [.......]
              | /                 | /
        [.......] — — — — — — —  [.......]
                    ↑ [.......]

````

2026-06-18 Wk 25 Thu - 21:28 +03:00

````
rs{a} = λ (a : A) → r .map (r .section .map a)
sp{a} = λ (a : A) → r .section .proof a
B p   = λ (p : x ≡ y) → ap (r .map) (ap (r .section .map) p)

                         ↓ [refl...]
              [......y] — — — @ — — — — > [y......]
                    / ^                 / ^
[~spx ∙ B ∙..| →  /   |               / ←-|-- [p......]
|spy.........]  /     |  ↓[refl...] /     |                  _________________________________ 
      [......x] — — — — — @ — > [x......] |                 |                    [Goal00]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       |← [spy....]|       @ ← [refl...]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [.....]  |
              |       |[...refl] →@       |                 |    i             / |            |
  [....spx] → |   [....msy] — — — | — — > [y......]         |          [.....]   [Blk01]      |
   [....B p] -|---→ / [spy....] ↑ |     /                   |_________________________________|
              |   /               |   / ← [p......]
              | /                 | /
        [....msx] — — — — — — —  [x......]
                    ↑ [....spx]

````

````
                         ↓ [spy....]
              [msy....] — — — — — — — — > [y......]
                    / ^                 / ^
[B p.........| →  /   |               / ←-|-- [p......]
|............]  /     |  ↓[spx....] /     |                  _________________________________ 
      [msx....] — — — — — — — > [x......] |                 |                    [Goal01]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       @← [refl...]|       | ← [spy....]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [Dup01]  |
              |       |[spx....] →|       |                 |    i             / |            |
  [refl...] → @   [msy....] — @ — | — — > [msy....]         |          [.....]   [.....]      |
   [B p....] -|---→ / [refl...] ↑ |     /                   |_________________________________|
              |   /               |   / ← [B p....]
              | /                 | /
        [msx....] — — — @ — — —  [msx....]
                    ↑ [refl...]


                         ↓ [spy....]
              [msy....] — — — — — — — — > [y......]
                    / ^                 / ^
[B p.........| →  /   |               / ←-|-- [p......]
|............]  /     |  ↓[spx....] /     |                  _________________________________ 
      [msx....] — — — — — — — > [x......] |                 |                    [Goal01]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       @← [refl...]|       @ ← [refl...]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [.....]  |
              |       |[spx....] →|       |                 |    i             / |            |
  [refl...] → @   [msy....] — — — | — — > [y......]         |          [.....]   [Blk02]      |
   [B p....] -|---→ / [spy....] ↑ |     /                   |_________________________________|
              |   /               |   / ← [spx ∙ p]
              | /                 | /
        [msx....] — — — @ — — —  [msx....]
                    ↑ [refl...]


                         ↓ [spy....]
              [msy....] — — — — — — — — > [y......]
                    / ^                 / ^
[B p.........| →  /   |               / ←-|-- [spx ∙ p]
|............]  /     |  ↓[refl...] /     |                  _________________________________ 
      [msx....] — — — — @ — — > [msx....] |                 |                    [Goal02]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       |← [~spy...]|       | ← [p......]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [Dup01] — ∙ — [.....]  |
              |       |[refl...] →@       |                 |    i             / |            |
  [~spx...] → |   [y......] — — — | — — > [x......]         |          [.....]   [.....]      |
   [p......] -|---→ / [~p.....] ↑ |     /                   |_________________________________|
              |   /               |   / ← [spx....]
              | /                 | /
        [x......] — — — — — — —  [msx....]
                    ↑ [~spx...]
````

2026-06-20 Wk 25 Sat - 10:24 +03:00

````
ms{a} = λ (a : A) → r .map (r .section .map a)
sp{a} = λ (a : A) → r .section .proof a
B p   = λ (p : x ≡ y) → ap (r .map) (ap (r .section .map) p)
p₁    = (sym spx) ∙∙ B p ∙∙ (spy)

                         ↓ [refl...]
              [y......] — — @ — — — — — > [y......]
                    / ^                 / ^
[p₁..........| →  /   |               / ←-|-- [p......]
|............]  /     |  ↓[refl...] /     |                  _________________________________ 
      [x......] — — — — @ — — > [x......] |                 |                    [Goal00]     |    Sq₀₀-faces i j k (i = i0) = face j k
      where
        face : Square k
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       |← [spy....]|       @ ← [refl...]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [Dup00]  |
              |       |[refl...] →@       |                 |    i             / |            |
  [spx....] → |   [msy....] — — — | — — > [y......]         |          [.....]   [.....]      |
   [B p....] -|---→ / [spy....] ↑ |     /                   |_________________________________|
              |   /               |   / ← [p₁.....]
              | /                 | /
        [msx....] — — — — — — —  [x......]
                    ↑ [spx....]
````

### Iteration 2.2 Cube Faces Filled

2026-06-20 Wk 25 Sat - 10:34 +03:00

````
ms{a} = λ (a : A) → r .map (r .section .map a)
sp{a} = λ (a : A) → r .section .proof a
B p   = λ (p : x ≡ y) → ap (r .map) (ap (r .section .map) p)
p₁    = (sym spx) ∙∙ B p ∙∙ (spy)

                         ↓ [refl...]
              [y......] — — @ — — — — — > [y......]
                    / ^                 / ^
     [p₁.....] →  /   |               / ←-|-- [p......]
                /     |  ↓[refl...] /     |                  _________________________________ 
      [x......] — — — — @ — — > [x......] |                 |                    [Goal00]     |
              ^       |           ^       |                 |  ^   j             ^  [.....]   |
              |       |← [spy....]|       @ ← [refl...]     |k | /               | /          |
              |       |           |       |                 |  ∙ — >   [.....] — ∙ — [Dup00]  |
              |       |[refl...] →@       |                 |    i             / |            |
  [spx....] → |   [msy....] — — — | — — > [y......]         |          [.....]   [.....]      |
   [B p....] -|---→ / [spy....] ↑ |     /                   |_________________________________|
              |   /               |   / ← [p₁.....]
              | /                 | /
        [msx....] — — — — — — —  [x......]
                    ↑ [spx....]
````
