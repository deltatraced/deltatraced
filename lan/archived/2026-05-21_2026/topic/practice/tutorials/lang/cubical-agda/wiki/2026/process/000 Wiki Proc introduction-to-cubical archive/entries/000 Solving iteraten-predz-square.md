---
parent: '[[000 Wiki Proc introduction-to-cubical archive]]'
spawned_by: '[[000 Wiki Proc introduction-to-cubical archive]]'
context_type: entry
---

Parent: [000 Wiki Proc introduction-to-cubical archive](../000%20Wiki%20Proc%20introduction-to-cubical%20archive.md)

Spawned by: [000 Wiki Proc introduction-to-cubical archive](../000%20Wiki%20Proc%20introduction-to-cubical%20archive.md)

Spawned in: [^spawn-entry-279326](../000%20Wiki%20Proc%20introduction-to-cubical%20archive.md#spawn-entry-279326)

# 1 Journal

2026-05-05 Wk 19 Tue - 17:58 +03:00

````haskell
iterateⁿ-predℤ-square : {x : A} → (p : x ≡ x) → (n : ℤ) → Square (iterateⁿ p (predℤ n)) (iterateⁿ p n) refl p
-- Exercise:
iterateⁿ-predℤ-square p (pos zero)    i j = p (i ∨ ~ j) -- Use a connection
-- iterateⁿ-predℤ-square p (pos (suc n)) i j = {!(iterateⁿ-predℤ-square p (pos n)) (j) ((  i) ∧ (  j))!} -- Try `∙-filler` for `p`
-- iterateⁿ-predℤ-square p (pos (suc n)) i j = {! (∙-filler p (Sq₀ j)) ((  i1) ∧ (  j)) ((  i) ∧ (  i1)) !} -- Try `∙-filler` for `p`
-- iterateⁿ-predℤ-square p (pos (suc n)) i j = {!   !} -- Try `∙-filler` for `p`
iterateⁿ-predℤ-square p (pos (suc n)) = Sq₀ -- Try `∙-filler` for `p`
  where
    Sq₂ : Square refl (p ∙ refl)
                 (iterateⁿ (p ∙ refl) (predℤ (pos n))) (iterateⁿ (p ∙ refl) (pos n))
    Sq₂ = flip-square (iterateⁿ-predℤ-square ((∙-filler p refl) i1) (pos n))

    Sq₁ : Square (iterateⁿ p (predℤ (pos n))) (iterateⁿ p (pos n)) refl p
    Sq₁ = (iterateⁿ-predℤ-square p (pos n))
    Sq₀ : Square (iterateⁿ p (pos n)) (iterateⁿ p (pos n) ∙ p) refl p
    Sq₀ i j = ∙-filler (iterateⁿ p (pos n)) p i j
    -- ∙-filler : (p : x ≡ y) (q : y ≡ z) → Square p (p ∙ q) refl q
    -- Sq₀ = {!∙-filler p (Sq₁)!}
    -- Sq₀ i = {! ∙-filler p (Sq₁ i) i !}
````

I spent a long time just trying to somehow get `∙-filler` and `Sq₁` to somehow fit together, but they didn't.

I was expecting this problem would be solved by induction, and I had to use `∙-filler` then to somehow compose by p to get from the prior case, but it turns out that `Sq₀` was just in the shape of `∙-filler`, and nothing else was necessary.

2026-05-05 Wk 19 Tue - 18:22 +03:00

Now having problems with this

````haskell
iterateⁿ-predℤ-square p (negsuc n) = Sq₀ -- Try `∙-filler` for `sym p`
  where
    Sq₀ : Square (iterateⁿ p (negsuc n) ∙ sym p) (iterateⁿ p (negsuc n)) refl p
    Sq₀ i j = {!∙-filler (iterateⁿ p (negsuc n) ∙ sym p) p i j !}
````

````
Goal and Context

Agda v2.8.0

- Goal

A ———— Boundary (wanted) ————————————————————————————————————— 
j = i0 ⊢ x 
j = i1 ⊢ p i 
i = i0 ⊢ (iterateⁿ p (negsuc n) ∙ sym p) j 
i = i1 ⊢ iterateⁿ p (negsuc n) j

- Have

A ———— Boundary (actual) ————————————————————————————————————— 
j = i0 ⊢ refl i 
j = i1 ⊢ p i 
i = i0 ⊢ (iterateⁿ p (negsuc n) ∙ sym p) j 
i = i1 ⊢ ((iterateⁿ p (negsuc n) ∙ sym p) ∙ p) j
````

I need `sym p` and `p` to cancel for `i = i1` for all this to match.

2026-05-05 Wk 19 Tue - 18:57 +03:00

This is it! Just had to come at it from the other direction rather than adding too much and hoping to cancel

````haskell
iterateⁿ-predℤ-square p (negsuc n) = Sq₀ -- Try `∙-filler` for `sym p`
  where
    Sq₁ : Square (iterateⁿ p (negsuc n)) (iterateⁿ p (negsuc n) ∙ sym p) refl (sym p)
    Sq₁ = ∙-filler (iterateⁿ p (negsuc n)) (sym p)
    Sq₀ : Square (iterateⁿ p (negsuc n) ∙ sym p) (iterateⁿ p (negsuc n)) refl p
    Sq₀ i j = Sq₁ (~ i) j
````
