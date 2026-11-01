---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-0aaed8](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-0aaed8)

# Journal

2026-09-21 Wk 39 Mon - 06:23 +03:00

This should be the encode-decode method template, replace ⊤ for some other inductive type. We have to come up with a code and then an equivalence from that code to the path in that type.

I made a modification to what the CQTS authors did, which is to declare the code at the same level as the equivalence, so that we can refer to it more uniformly.

````haskell
≡code-⊤ : ⊤ → ⊤ → Type ℓ-zero
≡code-⊤ a̬ â = {!!}

≡≃≡⊤ : (a̬ â : ⊤) → (a̬ ≡ â) ≃ (≡code-⊤ a̬ â)
≡≃≡⊤ a̬ â = inv→equiv (encode a̬ â) (decode a̬ â) (to-fro a̬ â) (fro-to a̬ â)
  where
    encode-refl : (a : ⊤) → ≡code-⊤ a a
    encode-refl = {!!}

    encode : (a̬ â : ⊤) → a̬ ≡ â → ≡code-⊤ a̬ â
    encode a̬ â = J (λ a _ → ≡code-⊤ a̬ a) (encode-refl a̬)

    decode : (a̬ â : ⊤) → ≡code-⊤ a̬ â → a̬ ≡ â
    decode = {!!}

    to-fro : (a̬ â : ⊤) → (encode a̬ â) hasSection (decode a̬ â)
    to-fro = {!!}

    fro-to-refl : (a : ⊤) → (decode a a) (encode a a refl) ≡ refl
    fro-to-refl = {!!}

    fro-to : (a̬ â : ⊤) → (encode a̬ â) hasRetract (decode a̬ â)
    fro-to a̬ â = J (λ a p → (decode a̬ a) (encode a̬ a p) ≡ p) (fro-to-refl a̬)
````

Type formers can look a little different:

````haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {B : Type ℓ₂}
  where
    ≡code-⊎ : A ⊎ B → A ⊎ B → Type (ℓ-max ℓ₁ ℓ₂)
    ≡code-⊎ a̬ â = {!!}
    
    ≡≃≡⊎ : (a̬ â : A ⊎ B) → (a̬ ≡ â) ≃ (≡code-⊎ a̬ â)
    ≡≃≡⊎ a̬ â = inv→equiv (encode a̬ â) (decode a̬ â) (to-fro a̬ â) (fro-to a̬ â)
      where
        encode-refl : (a : A ⊎ B) → ≡code-⊎ a a
        encode-refl = {!!}
    
        encode : (a̬ â : A ⊎ B) → a̬ ≡ â → ≡code-⊎ a̬ â
        encode a̬ â = J (λ a _ → ≡code-⊎ a̬ a) (encode-refl a̬)
    
        decode : (a̬ â : A ⊎ B) → ≡code-⊎ a̬ â → a̬ ≡ â
        decode = {!!}
    
        to-fro : (a̬ â : A ⊎ B) → (encode a̬ â) hasSection (decode a̬ â)
        to-fro = {!!}
    
        fro-to-refl : (a : A ⊎ B) → (decode a a) (encode a a refl) ≡ refl
        fro-to-refl = {!!}
    
        fro-to : (a̬ â : A ⊎ B) → (encode a̬ â) hasRetract (decode a̬ â)
        fro-to a̬ â = J (λ a p → (decode a̬ a) (encode a̬ a p) ≡ p) (fro-to-refl a̬)
````

You'll also have to decide whether `encode` defined through `encode-refl` is necessary or whether to do it directly. Same for `fro-to`.
