---
parent: '[[000 introduction-to-cubical]]'
spawned_by: '[[000 introduction-to-cubical]]'
context_type: task
status: done
---

Parent: [000 introduction-to-cubical](../000%20introduction-to-cubical.md)

Spawned by: [000 introduction-to-cubical](../000%20introduction-to-cubical.md)

Spawned in: [^spawn-task-c926a2](../000%20introduction-to-cubical.md#spawn-task-c926a2)

# 1 Journal

2026-04-14 Wk 16 Tue - 08:06 +03:00

Gonna pretend it is haskell code. For some reason agda is not yet recognized here (obsidian markdown).

````haskell
×-map-≃-underlying : {A A' B B' : Type ℓ} → (f : A ≃ A') → (g : B ≃ B')
  → (×-map-≃-ua f g) .map ≡ ×-map (f .map) (g .map)
×-map-≃-underlying f g = {!!}
````

I am trying to create the above path goal, so I am going to save some details here.

The problem is in `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-6--Univalence.lagda.md`

2026-04-14 Wk 16 Tue - 08:08 +03:00

(Elaboration note 1.0)

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
                   (au (×-map-≡ (ua f) (ua g))) .map ≡⟨ refl {- elaborated thru ua-comp (*1.0) -} ⟩ 
    (transport         (×-map-≡ (ua f) (ua g)))      ≡⟨ ({!!})⟩ 

  where
    P₀ : (transport (ua (au (×-map-≡ (ua f) (ua g))))) ≡
                        (au (×-map-≡ (ua f) (ua g)) .map)
    P₀ i (a , b) = ua-comp ((au (×-map-≡ (ua f) (ua g)))) (a , b) i
````

(/Elaboration note 1.0)

2026-04-14 Wk 16 Tue - 08:18 +03:00

Current pathways I am exploring:

````haskell
      (×-map-≃-ua f g) .map                                ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
                     (au (×-map-≡ (ua f) (ua g))) .map     ≡⟨ sym P₀ ⟩ 
      (transport (ua (au (×-map-≡ (ua f) (ua g)))))        ≡⟨ {!!} ⟩ 

      (×-map-≃-ua f g) .map                             ≡⟨ refl {- unfold ×-map-≃-ua -} ⟩
                   (au (×-map-≡ (ua f) (ua g))) .map    ≡⟨ refl {- elaborated thru ua-comp (*1.0) -} ⟩ 
    (transport         (×-map-≡ (ua f) (ua g)))         ≡⟨ ({!!})⟩ 
````

It seems we shouldn't use `sym P₀`  here, and it suffices to make use of the 1.0 elaborated path which we have learned by use of `sym P₀` for elaboration purposes.

2026-04-14 Wk 16 Tue - 23:51 +03:00

Putting the elaboration notes in wiki, so that we can evolve them instead of have the source code point to this log: `Elaboration Notes for ×-map-≃-underlying (546a45)` .

2026-04-15 Wk 16 Wed - 01:25 +03:00

Current path we have from the goal:

````haskell
    ×-map (transport (ua f)) (transport (ua g))            ≡⟨ (λ i (a , b) → ×-map (λ a' → ua-comp f a' i) 
                                                                                   (λ b' → ua-comp g b' i) ((a , b))) ⟩ 
    ×-map (f .map)           (g .map)                      ∎
````

Basically application of `ua-comp` to both `f .map` and `g .map`. The hint seemed to suggest that we should apply it to both.

2026-04-17 Wk 16 Fri - 08:49 +03:00

This is one thing I've tried which would require canceling nested `transport-fixing`s, though this may be needlessly complicated. I couldn't apply `transport-cancel` directly to this.

````haskell
    P₁ : (×-map (transport (ua f)) (transport (ua g))) ≡ 
         (transport (×-map-≡ (ua f) (ua g)))
    P₁ i (a , b) .fst = P₁₀ i
      where
         P₁₁ = 
            transport-fixing (λ i₁ → A') i0
              (transport-fixing (λ i₁ → A') i0
                (f .map (
                  transport-fixing (λ i₁ → A) i0 a)))     ≡⟨ λ i₁ → {!   !} ⟩ 
              transport-fixing (λ i₁ → A') i0
                (f .map (
                  transport-fixing (λ i₁ → A) i0 a))     ∎


         P₁₀ = 
          (transport-fixing (λ i₁ → A') i0 (f .map a))        ≡⟨ {!!} ⟩ 
          -- {!!}                                                ≡⟨ {!!} ⟩ 

          -- {!!}                                                ≡⟨ {!!} ⟩ 
          (transport-fixing (λ i₁ → A') i0
            (f .map (
              transport-fixing (λ i₁ → A) i0 a)))              ≡⟨ {!transport-cancel (λ i₁ → A') (f .map (transport-fixing (λ i₁ → A) i0 a))!} ⟩ 

          transport-fixing (λ i₁ → A') i0
            (transport-fixing (λ i₁ → A') i0
              (transport-fixing (λ i₁ → A') i0
                (transport-fixing (λ i₁ → A') i0
                  (f .map (
                    transport-fixing (λ i₁ → A) i0 a)))))     ≡⟨ refl {- elaborated from below -} ⟩ 
          (transport (×-map-≡ (ua f) (ua g)) (a , b) .fst)    ∎
````

2026-04-18 Wk 16 Sat - 05:21 +03:00

We are able to cancel these `transport-cancel`s for this case:

````haskell
         P₁₁ = 
            transport-fixing (λ i₁ → A') i0 (
              transport-fixing (λ i₁ → A') i0 (f .map a))     ≡⟨ transport-cancel (λ i₁ → A') (f .map a) ⟩ 
                                              (f .map a)      ≡⟨ sym (transport-refl (f .map a)) ⟩ 
            transport-fixing (λ i₁ → A') i0 (f .map a)        ∎
````

To make it more extensible, we abstract out `(f .map a)`:

````haskell
         P₁₁ = 
            transport-fixing (λ i₁ → A') i0 (
              transport-fixing (λ i₁ → A') i0 a')             ≡⟨ transport-cancel (λ i₁ → A') a' ⟩ 
                                              a'              ≡⟨ sym (transport-refl a') ⟩ 
            transport-fixing (λ i₁ → A') i0 a'                ∎
              where
                a' = (f .map a)
````

In our case we are interested in this particular specialization for `a'`:

````haskell
         P₁₁ = 
            transport-fixing (λ i₁ → A') i0 (
              transport-fixing (λ i₁ → A') i0 a')             ≡⟨ transport-cancel (λ i₁ → A') a' ⟩ 
                                              a'              ≡⟨ sym (transport-refl a') ⟩ 
            transport-fixing (λ i₁ → A') i0 a'                ∎
              where
                a' = (f .map (transport-fixing (λ i₁ → A) i0 a))
````

This choice will have to change multiple times for transport towers, so let's actually abstract `a'`:

````haskell
         P₁₁ = λ (a' : A') →
            transport-fixing (λ i₁ → A') i0 (
              transport-fixing (λ i₁ → A') i0 a')             ≡⟨ transport-cancel (λ i₁ → A') a' ⟩ 
                                              a'              ≡⟨ sym (transport-refl a') ⟩ 
            transport-fixing (λ i₁ → A') i0 a'                ∎
````

2026-04-18 Wk 16 Sat - 05:57 +03:00

We were able to solve this tower! A similar situation happens with

````haskell
    P₀ i (a , b) .snd = P₀₀ i
      where
        P₀₀ =
          transport-fixing (λ i₁ → B') i0 (g .map b) ≡⟨ {!!} ⟩ 

          transport (×-map-≡ (ua f) (ua g)) (a , b) .snd ∎
````

Note that `P₀₀` is abstracted away because directly putting the path composition results in an error.

2026-04-18 Wk 16 Sat - 10:32 +03:00

We were able to solve it by reducing these transport-fixing towers.

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

Saving this to wiki `546a45` as well.

This proof looks longer than it might need to be, but it does indicates that destructing the pair and consider each projection in isolation was key to producing the path.
