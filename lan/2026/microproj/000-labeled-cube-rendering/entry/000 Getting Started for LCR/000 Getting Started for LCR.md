# 1 Journal

2026-05-09 Wk 19 Sat - 18:24 +03:00

Hmm. I might want to be able to do this in Rust and also in Agda. We wanna use [OpenGL](https://www.opengl.org/).

2026-05-09 Wk 19 Sat - 19:47 +03:00

Currently trying to compile `hello-world-prog.agda` from [a-taste-of-agda](https://agda.readthedocs.io/en/latest/getting-started/a-taste-of-agda.html). Need the standard library.

From https://github.com/agda/agda-stdlib,

````sh
sh -c "$(curl --proto '=https' --tlsv1.2 -s https://raw.githubusercontent.com/agda/agda-stdlib/refs/heads/master/stdlib-install.sh)"
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda
agda --compile hel
Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv#^spawn-entry-9534d2|^spawn-entry-9534d2]]

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical#^spawn-entry-be7615|^spawn-entry-be7615]]

Overview: [[000 Overview Wiki Proc CQTS Intro to Cubical]]

Errata: [[002 Errata Proc CQTS retract equiv]]

Iterations: [[004 Iterations for Proc CQTS retract equiv]]

---

# What?

We need to solve

```haskell
retract-≡ : {ℓ₀ : Level} → {A B : Type ℓ₀} → (r : B RetractOnto A)
  → {x y : A}
  → (r .section .map x ≡ r .section .map y) RetractOnto (x ≡ y)
````

Problem is at `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-7--Propositions.lagda.md`

# Journal

2026-06-17 Wk 25 Wed - 12:11 +03:00

Spawn [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/002 Errata Proc CQTS retract equiv](../../../../topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/entry/002%20Errata%20Proc%20CQTS%20retract%20equiv.md) ^spawn-entry-5508d1

Spawn [000 How do we interpret retract and section? CQTS](../../../../topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/investigation/000%20How%20do%20we%20interpret%20retract%20and%20section%3F%20CQTS.md) ^spawn-invst-acfdc3

2026-06-17 Wk 25 Wed - 23:53 +03:00

Spawn [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/004 Iterations for Proc CQTS retract equiv](../../../../topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/entry/004%20Iterations%20for%20Proc%20CQTS%20retract%20equiv.md) ^spawn-entry-9534d2

2026-06-17 Wk 25 Wed - 10:56 +03:00

Okay getting back to this.

We need to solve

````haskell
retract-≡ : {ℓ₀ : Level} → {A B : Type ℓ₀} → (r : B RetractOnto A)
  → {x y : A}
  → (r .section .map x ≡ r .section .map y) RetractOnto (x ≡ y)
````

2026-06-17 Wk 25 Wed - 10:56 +03:00

This is the hint cube:

````
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
````

`r (s x)` should mean `r .map (r . section .map x)`.

2026-08-24 Wk 35 Mon - 00:17 +03:00

Spawn [000 Reproduce how far we got in cqts but in problems-mkn module format](../../../../topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/task/000%20Reproduce%20how%20far%20we%20got%20in%20cqts%20but%20in%20problems-mkn%20module%20format.md) ^spawn-task-89aed8

2026-09-29 Wk 40 Tue - 12:54 +03:00

This will be continued here: [032 CQTS Problems Attempt 0 retract-≡](../../../../topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/entry/032%20CQTS%20Problems%20Attempt%200%20retract-%E2%89%A1.md)

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

lo-world-prog.agda
./hello-world-prog

````

2026-05-09 Wk 19 Sat - 22:20 +03:00

Spawn [[000 Compile cubical agda to an executable using ctqs sources]] ^spawn-task-5385cc

2026-05-13 Wk 20 Wed - 00:35 +03:00

Spawn [[002 Setup openGL rendering app in rust and do some dev]] ^spawn-task-69c8c6

2026-05-13 Wk 20 Wed - 18:32 +03:00

Alright since we got things going on multiple front here, let's move this all to its own repository:

```sh
# in /home/lan/src/cloned/gh/deltachives
mkdir labeled-cube-rendering-2026-m000

# in /home/lan/src/cloned/gh/deltachives/labeled-cube-rendering-2026-m000
mv ~/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering .
mv 000-LabeledCubeRendering/* .
rmdir 000-LabeledCubeRendering
cp rs/README.md rs/FUNDING.yml rs/CONTRIBUTING.md rs/LICENSE .
# update LICENSE
````

Code now in [gh deltachives/labeled-cube-rendering-2026-m000](https://github.com/deltachives/labeled-cube-rendering-2026-m000)

`m000` has `m` for microproject, since their indices are different from projects here.

# 2 Spawn Trees

* [000 Getting Started for LCR](000%20Getting%20Started%20for%20LCR.md)
  * entry [000 LCR Side Activity](entry/000%20LCR%20Side%20Activity.md)
  * todo task [000 Compile cubical agda to an executable using ctqs sources](task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md)
    * entry [001 Full content for agda build 1](entry/001%20Full%20content%20for%20agda%20build%201.md)
    * issue [000 Unable to compile hello world with cubical agda flag GHC-49196](issue/000%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md)
      * entry [002 Updates for Unable to compile hello world with cubical agda flag GHC-49196](entry/002%20Updates%20for%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md)
    * todo task [001 Enable agda-mode in vim and any other intellisense](task/001%20Enable%20agda-mode%20in%20vim%20and%20any%20other%20intellisense.md)
  * todo task [002 Setup openGL rendering app in rust and do some dev](task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)

# 3 Index

**entry**

[000 LCR Side Activity](entry/000%20LCR%20Side%20Activity.md)

[001 Full content for agda build 1](entry/001%20Full%20content%20for%20agda%20build%201.md)

[002 Updates for Unable to compile hello world with cubical agda flag GHC-49196](entry/002%20Updates%20for%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md)

**issue**

[000 Unable to compile hello world with cubical agda flag GHC-49196](issue/000%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md)

**task**

todo [000 Compile cubical agda to an executable using ctqs sources](task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md)

todo [001 Enable agda-mode in vim and any other intellisense](task/001%20Enable%20agda-mode%20in%20vim%20and%20any%20other%20intellisense.md)

todo [002 Setup openGL rendering app in rust and do some dev](task/002%20Setup%20openGL%20rendering%20app%20in%20rust%20and%20do%20some%20dev.md)
