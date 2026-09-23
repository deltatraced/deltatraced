---
parent: '[[000 introduction-to-cubical archive]]'
spawned_by: '[[000 Spawn Logs for intro to cubical]]'
context_type: entry
---

Parent: [000 introduction-to-cubical archive](../000%20introduction-to-cubical%20archive.md)

Spawned by: [000 Spawn Logs for intro to cubical](000%20Spawn%20Logs%20for%20intro%20to%20cubical.md)

Spawned in: [^spawn-entry-546a45](000%20Spawn%20Logs%20for%20intro%20to%20cubical.md#spawn-entry-546a45)

---

# 1 Problem

````haskell
×-map-≃-underlying : {A A' B B' : Type ℓ} → (f : A ≃ A') → (g : B ≃ B')
  → (×-map-≃-ua f g) .map ≡ ×-map (f .map) (g .map)
×-map-≃-underlying f g = {!!}
````

# 2 Elaboration Note 1

`(*1)`

````haskell
(×-map-≃-ua f g) .map                                ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
---                (au (×-map-≡ (ua f) (ua g))) .map ≡⟨ sym P₀ ⟩ 
--- (transport (ua (au (×-map-≡ (ua f) (ua g)))))    ≡⟨ P₀ ⟩ 
--- (transport         (×-map-≡ (ua f) (ua g)))      ≡⟨ _ ⟩ 
                   (au (×-map-≡ (ua f) (ua g))) .map ≡⟨ refl {- elaborated via P₀ above -} ⟩ 
    (transport         (×-map-≡ (ua f) (ua g)))      ≡⟨ ({!!})⟩ 

  where
    P₀ : (transport (ua (au (×-map-≡ (ua f) (ua g))))) ≡
                        (au (×-map-≡ (ua f) (ua g)) .map)
    P₀ i (a , b) = ua-comp ((au (×-map-≡ (ua f) (ua g)))) (a , b) i
````

Use of `ua-comp` allows us to cancel out a `transport (ua (...))`. The above will be simplified to the following in the code:

````haskell
(×-map-≃-ua f g) .map                                ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
                   (au (×-map-≡ (ua f) (ua g))) .map ≡⟨ refl {- elaborated thru ua-comp (*1) -} ⟩ 
    (transport         (×-map-≡ (ua f) (ua g)))      ≡⟨ ({!!})⟩ 

  where
    P₀ : (transport (ua (au (×-map-≡ (ua f) (ua g))))) ≡
                        (au (×-map-≡ (ua f) (ua g)) .map)
    P₀ i (a , b) = ua-comp ((au (×-map-≡ (ua f) (ua g)))) (a , b) i
````

# 3 Closed Paths

## 3.1 Elaboration via P0 from refl

````haskell
               (×-map-≃-ua f g)             .map            ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
               (au (×-map-≡ (ua f) (ua g))) .map            ≡⟨ refl {- elaborated thru ua-comp (*1) -} ⟩ 
    (transport     (×-map-≡ (ua f) (ua g)))                 ≡⟨ (λ i (a , b) → {!!})⟩ 
````

## 3.2 Sym P0 from refl

````haskell
                   (×-map-≃-ua f g)               .map     ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
                   (au (×-map-≡ (ua f) (ua g)))   .map     ≡⟨ sym P₀ ⟩ 
    (transport (ua (au (×-map-≡ (ua f) (ua g)))))          ≡⟨ {!!} ⟩ 
````

## 3.3 Path from Goal

````haskell
    ×-map (transport (ua f)) (transport (ua g))            ≡⟨ (λ i (a , b) → ×-map (λ a' → ua-comp f a' i) 
                                                                                   (λ b' → ua-comp g b' i) ((a , b))) ⟩ 
    ×-map (f .map)           (g .map)                      ∎
````

# 4 Proof

````haskell
×-map-≃-underlying : {ℓ₁ : Level} → {A A' B B' : Type ℓ₁} → (f : A ≃ A') → (g : B ≃ B')
  → (×-map-≃-ua f g) .map ≡ ×-map (f .map) (g .map)
×-map-≃-underlying {ℓ₁ = ℓ₁} {A = A} {A' = A'} {B = B} {B' = B'} f g =
               (×-map-≃-ua f g)             .map           ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
               (au (×-map-≡ (ua f) (ua g))) .map           ≡⟨ refl {- elaborated thru ua-comp (*1) -} ⟩ 
    (transport     (×-map-≡ (ua f) (ua g)))                ≡⟨ sym P₀ ⟩ 
    ×-map (transport (ua f)) (transport (ua g))            ≡⟨ (λ i (a , b) → ×-map (λ a' → ua-comp f a' i) 
                                                                                   (λ b' → ua-comp g b' i) ((a , b))) ⟩ 
    ×-map (f .map)           (g .map)                      ∎
  where
    P₀ : (×-map (transport (ua f)) (transport (ua g))) ≡ 
         (transport (×-map-≡ (ua f) (ua g)))
    P₀ i (a , b) .fst = P₀₀ i
      where
        P₀₁ = λ (a' : A') →
          transport-fixing (λ i₁ → A') i0 (
          transport-fixing (λ i₁ → A') i0 a')                   ≡⟨ transport-cancel (λ i₁ → A') a' ⟩ 
                                          a'                    ≡⟨ sym (transport-refl a') ⟩ 
          transport-fixing (λ i₁ → A') i0 a'                    ∎
        P₀₀ = 
          (transport-fixing (λ i₁ → A') i0 (f .map a))          ≡⟨ transport-refl (f .map a) ⟩ 
                                           (f .map a)           ≡⟨ (λ i₁ → f .map (sym (transport-refl a) i₁)) ⟩ 
                                           (f .map
          (transport-fixing (λ i₁ → A) i0 
                                                   a))          ≡⟨ sym (transport-refl (f .map (
                                                                        transport-fixing (λ i₁ → A) i0 a))) ⟩ 
          (transport-fixing (λ i₁ → A') i0
                                           (f .map
          (transport-fixing (λ i₁ → A) i0 
                                                   a)))          ≡⟨ sym (P₀₁ _) ⟩ 
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
                                           (f .map
          (transport-fixing (λ i₁ → A) i0          a))))         ≡⟨ sym (P₀₁ _) ⟩ 
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
                                           (f .map
          (transport-fixing (λ i₁ → A) i0          a)))))        ≡⟨ sym (P₀₁ _) ⟩ 
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
          (transport-fixing (λ i₁ → A') i0
                                           (f .map
          (transport-fixing (λ i₁ → A) i0 
                                                   a))))))        ≡⟨ refl {- elaborated from below -} ⟩ 
          (transport (×-map-≡ (ua f) (ua g)) (a , b) .fst)        ∎
    P₀ i (a , b) .snd = P₀₀ i
      where
        P₀₁ = λ (A : Type ℓ₁) (a : A) →
          transport-fixing (λ i₁ → A) i0 (
          transport-fixing (λ i₁ → A) i0 a)                 ≡⟨ transport-cancel (λ i₁ → A) a ⟩ 
                                         a                  ≡⟨ sym (transport-refl a) ⟩ 
          transport-fixing (λ i₁ → A) i0 a                  ∎

        P₀₀ =
          transport-fixing (λ i₁ → B') i0 (g .map b)        ≡⟨ transport-refl (g .map b) ⟩ 
                                          (g .map b)        ≡⟨ ap (g .map) (sym (transport-refl b)) ⟩ 
                                          (g .map
          (transport-fixing (λ i₁ → B ) i0 
                                                  b))       ≡⟨ ap (g .map) (sym (P₀₁ B b)) ⟩ 
                                          (g .map
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0 
                                                  b)))      ≡⟨ ap (g .map) (sym (P₀₁ B _)) ⟩ 
                                          (g .map
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0 
                                                  b))))     ≡⟨ sym (transport-refl _) ⟩ 
           transport-fixing (λ i₁ → B') i0
                                          (g .map
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0 
                                                  b))))     ≡⟨ sym (P₀₁ B' _) ⟩ 
           transport-fixing (λ i₁ → B') i0
          (transport-fixing (λ i₁ → B') i0
                                          (g .map
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0
          (transport-fixing (λ i₁ → B ) i0 
                                                  b)))))    ≡⟨ refl {- elaborated from below -} ⟩ 
          transport (×-map-≡ (ua f) (ua g)) (a , b) .snd    ∎
````
