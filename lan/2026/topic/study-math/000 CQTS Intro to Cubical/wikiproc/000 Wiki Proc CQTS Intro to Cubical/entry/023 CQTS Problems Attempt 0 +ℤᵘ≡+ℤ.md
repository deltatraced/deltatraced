---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries#^spawn-entry-d18651|^spawn-entry-d18651]]

# Journal

2026-09-26 Wk 39 Sat - 03:33 +03:00

```haskell
+ℤᵘ≡+ℤ i (pos (suc a)) b = p0 i
  where
    p0 : ((pos (suc a)) +ℤᵘ b) ≡ ((pos (suc a)) +ℤ b)
    p0 =
      ((pos (suc a)) +ℤᵘ b)
        ≡⟨ refl ⟩ -- ↓ Expand defn +ℤᵘ
      (transport (add-ℤ-≡ (pos (suc a))) b)
        ≡⟨ refl ⟩ -- ↓ Expand defn add-ℤ-≡
      (transport ((iterateⁿ sucℤ-≡) (pos (suc a))) b)
        ≡⟨ refl ⟩ -- ↓ Expand defn iterateⁿ 
      (transport ((iterateⁿ sucℤ-≡ (pos a)) ∙ sucℤ-≡) b)
        ≡⟨ {!!} ⟩ 
      ((pos (suc a)) +ℤ b)
        ∎
```

`+ℤ` computes on the right, not on the left:

```
_+ℤ_ : ℤ → ℤ → ℤ
a +ℤ pos b = a +pos b
a +ℤ negsuc b = a +negsuc b
```

I do want to be consistent and have everything compute on the left where possible, but this I did on the right to match the same definitions of `+pos` and `+negsuc`, which by their name expect to compute/expand on the right.

2026-09-26 Wk 39 Sat - 12:35 +03:00

```haskell
+ℤᵘ≡+ℤ : _+ℤᵘ_ ≡ _+ℤ_
+ℤᵘ≡+ℤ i a (pos zero) = p i
  where
    p = 
      a +ℤᵘ pos zero
        ≡⟨ refl ⟩ -- ↓ unfold +ℤᵘ.
      transport (add-ℤ-≡ a) (pos zero)
        ≡⟨ refl ⟩ -- ↓ ua ∙ au ≡ refl
      transport (ua (au (add-ℤ-≡ a))) (pos zero)
        ≡⟨ ua-comp (au (add-ℤ-≡ a)) (pos zero) ⟩
      au (add-ℤ-≡ a) .map (pos zero)
        ≡⟨ {!!} ⟩
      {!!}
        ≡⟨ {!!} ⟩
      a
        ∎
```

When I don't case split on the letft, I can't unfold `add-ℤ-≡` much further, which relies on `iterateⁿ`.

Here I tried to eliminate the `transport` and get a map by fitting it to the form of `ua-comp`, but it didn't seem to buy me anything.

I just noticed that they defined `_+ℤᵘ_` to take the right first.

```
_+ℤᵘ_ : ℤ → ℤ → ℤ
m +ℤᵘ n = transport (add-ℤ-≡ n) m
```

Wheras I defined it as

```
_+ℤᵘ_ : ℤ → ℤ → ℤ
a +ℤᵘ b = transport (add-ℤ-≡ a) b
```

Let's update the definition to do that, since we're being forced to case split on the right here.