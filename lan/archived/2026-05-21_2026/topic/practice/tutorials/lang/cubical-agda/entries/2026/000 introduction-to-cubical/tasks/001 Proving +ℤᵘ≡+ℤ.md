---
parent: '[[000 introduction-to-cubical]]'
spawned_by: '[[000 introduction-to-cubical]]'
context_type: task
status: todo
---

Parent: [000 introduction-to-cubical](../000%20introduction-to-cubical.md)

Spawned by: [000 introduction-to-cubical](../000%20introduction-to-cubical.md)

Spawned in: [^spawn-task-5d2bb5](../000%20introduction-to-cubical.md#spawn-task-5d2bb5)

# 1 Journal

2026-04-18 Wk 16 Sat - 13:26 +03:00

````haskell
+ℤᵘ≡+ℤ : _+ℤᵘ_ ≡ _+ℤ_
-- Exercise:
+ℤᵘ≡+ℤ i m (pos zero) = (+pos-ℤ-idr m) i
+ℤᵘ≡+ℤ i m (pos (suc x)) = P₀ i
  where 
    P₀ : m +ℤᵘ pos (suc x) ≡ m +pos suc x
    P₀ = 
            m +ℤᵘ  pos (suc x)        ≡⟨ refl {- reduces by computation -} ⟩ 
      sucℤ (m +ℤᵘ  pos      x)        ≡⟨ (λ i₁ → sucℤ (+ℤᵘ≡+ℤ i₁ m (pos x))) ⟩ 
      sucℤ (m +pos          x)        ≡⟨ sym (+pos-ℤ-extract-sucℤ-r m x) ⟩ 
            m +pos      suc x         ∎
+ℤᵘ≡+ℤ i m (negsuc zero) = predℤ m
+ℤᵘ≡+ℤ i m (negsuc (suc x)) = P₀ i
  where
    P₀ : m +ℤᵘ negsuc (suc x) ≡ predℤ m +negsuc x
    P₀ = 
             m +ℤᵘ negsuc (suc x)      ≡⟨ refl {- reduces by computation -} ⟩ 
      predℤ (m +ℤᵘ negsuc (    x))     ≡⟨ (λ i₁ → {! predℤ (+ℤᵘ≡+ℤ i₁ m (negsuc x))  0!}) ⟩ 
      predℤ  m +negsuc         x       ∎
````

This proof fails termination checking when trying to refine hole `?0`.

2026-04-19 Wk 16 Sun - 02:47 +03:00

````haskell
+ℤᵘ≡+ℤ : _+ℤᵘ_ ≡ _+ℤ_
-- Exercise:
+ℤᵘ≡+ℤ i m (pos zero) = (+pos-ℤ-idr m) i
+ℤᵘ≡+ℤ i m (pos (suc x)) = P₀ i
                           ~~
  where 
    P₀ : m +ℤᵘ pos (suc x) ≡ m +pos suc x
    P₀ = 
            m +ℤᵘ  pos (suc x)        ≡⟨ refl {- reduces by computation -} ⟩ 
      sucℤ (m +ℤᵘ  pos      x)        ≡⟨ (λ i₁ → sucℤ (+ℤᵘ≡+ℤ i₁ m (pos x))) ⟩ 
	                                                   ~~~~~~
      sucℤ (m +pos          x)        ≡⟨ sym (+pos-ℤ-extract-sucℤ-r m x) ⟩ 
            m +pos      suc x         ∎
+ℤᵘ≡+ℤ i m (negsuc zero) = predℤ m
+ℤᵘ≡+ℤ i m (negsuc (suc x)) = P₀ i
                              ~~
  where
    P₀ : m +ℤᵘ negsuc (suc x) ≡ predℤ m +negsuc x
    P₀ = 
             m +ℤᵘ negsuc (suc x)      ≡⟨ (λ i₁ → (+ℤᵘ≡+ℤ i₁ m (negsuc (suc x)))) ⟩ 
			                                       ~~~~~~
      predℤ  m +negsuc         x       ∎
````

This works but has red lines as marked by `~~`

Using `C-, C-.` for

````haskell
_ = {!transport +ℤᵘ≡+ℤ  !}
````

Gives this error segment:

````
Univalence.lagda.md:549,1-566,41: error: [TerminationIssue] Termination checking failed for the following functions: +ℤᵘ≡+ℤ
````
