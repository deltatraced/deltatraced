---
context_type: entry
---

Parent: [lan/2026/topic/study-math/002 Mikan Problem Entries/entry/001 Mikan Problem Attempt Entries/001 Mikan Problem Attempt Entries](../001%20Mikan%20Problem%20Attempt%20Entries.md)

Spawned by: [lan/2026/topic/study-math/002 Mikan Problem Entries/entry/001 Mikan Problem Attempt Entries/entry/000 Weekly Mikan Problem Attempts](000%20Weekly%20Mikan%20Problem%20Attempts.md)

Spawned in: [^spawn-entry-b1f228](000%20Weekly%20Mikan%20Problem%20Attempts.md#spawn-entry-b1f228)

# Journal

2026-11-01 Wk 44 Sun - 09:23 +03:00

General week notes: [008 TSW 26 Wk 44](../../../../../../main/entry/007%20Thought%20Stream%20Weeks/entry/008%20TSW%2026%20Wk%2044.md)

## ≡≃≡code-Fin

2026-11-01 Wk 44 Sun - 00:46 +03:00

````haskell
≡≃≡code-Fin :
    {len : ℕ}
  → (a b : Fin len)
  → (a ≡ b) ≃ (≡code-Fin a b)
≡≃≡code-Fin {len} a b = {!!}
  where
    encode-refl : {len : ℕ} → (a : Fin len) → ≡code-Fin {len} a a
    encode-refl {zero} ()
    encode-refl {suc len} zeroᶠ = tt
    encode-refl {suc len} (sucᶠ a) = encode-refl a
    
    encode : {len : ℕ} → (a b : Fin len) → (p : a ≡ b) → ≡code-Fin {len} a b
    encode {len} a b p = J (λ a' _ → ≡code-Fin {len} a a') (encode-refl {len} a) p

    decode : {len : ℕ} → (a b : Fin len) → (𝒸 : ≡code-Fin {len} a b) → a ≡ b
    decode {len} zeroᶠ zeroᶠ 𝒸 = refl
    decode {len} zeroᶠ (sucᶠ b) ()
    decode {len} (sucᶠ a) zeroᶠ ()
    decode {len} (sucᶠ a) (sucᶠ b) 𝒸 = ap sucᶠ (decode a b 𝒸)

    to-fro : {len : ℕ} → (a b : Fin len) → (encode a b) hasSection (decode a b)
    to-fro {zero} ()
    to-fro {suc len} zeroᶠ zeroᶠ tt = refl
    to-fro {suc len} zeroᶠ (sucᶠ b) ()
    to-fro {suc len} (sucᶠ a) zeroᶠ ()
    to-fro {suc len} (sucᶠ a) (sucᶠ b) 𝒸 = to-fro {len} a b 𝒸

    fro-to-refl : {len : ℕ} → (a : Fin len) → (encode a a) hasRetract (decode a a)
    fro-to-refl {zero} ()
    fro-to-refl {suc len} a p = {!!}

    fro-to : {len : ℕ} → (a b : Fin len) → (encode a b) hasRetract (decode a b)
    fro-to = {!!}
````

Why did the case `to-fro {suc len} (sucᶠ a) (sucᶠ b)` simplify to be exactly its inductive hypothesis?

I think because in `encode-refl` this also occurs. In that case, we only had `(sucᶠ a)`. But through `J` then I expect that the same thing applied to `(sucᶠ b)` now.
