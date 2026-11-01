---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-b35a13](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-b35a13)

# Journal

2026-09-13 Wk 37 Sun - 21:03 +03:00

Through \[\[005 CQTS Problem Entries#`+ℕ-≡ℕ-comm`\]\],

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-5--Propositions-as-Types.lagda.md`,

````haskell
+ℕ-≡ℕ-extract-suc-r : (a b : ℕ) → (a +ℕ (suc b)) ≡ℕ suc (a +ℕ b)
+ℕ-≡ℕ-extract-suc-r zero b = ≡ℕ-refl b
+ℕ-≡ℕ-extract-suc-r (suc a) b = +ℕ-≡ℕ-extract-suc-r a b

+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ b) ≡ℕ (b +ℕ a)
+ℕ-≡ℕ-comm zero b = ≡ℕ-sym (b +ℕ zero) b (+ℕ-≡ℕ-idr b)
+ℕ-≡ℕ-comm (suc a) b = {!≡ℕ-rwr _ _ _ (+ℕ-≡ℕ-comm a b)!}
  where
    heq_ab_step1 : suc (b +ℕ a) ≡ℕ b +ℕ suc a
    heq_ab_step1 = ≡ℕ-sym (b +ℕ (suc a)) (suc (b +ℕ a)) (+ℕ-≡ℕ-extract-suc-r b a)
    heq_ab : suc (a +ℕ b) ≡ℕ b +ℕ suc a
    heq_ab = {!+ℕ-≡ℕ-extract-suc-r a b!}
    heq_ab = {!≡ℕ-sym (b +ℕ (suc a)) (suc (b +ℕ a)) (+ℕ-≡ℕ-extract-suc-r a b)!}
_ = ≡ℕ-sym
````

````haskell
+ℕ-≡ℕ-comm (suc a) b = {!≡ℕ-rwr _ _ _ (+ℕ-≡ℕ-comm a b)!}

Goal: suc (a +ℕ b) ≡ℕ b +ℕ suc a
Have: _a_277 ≡ℕ _b_278 → _a_277 ≡ℕ _b'_279
———— Context ———————————————————————————————————————————————
b : ℕ
a : ℕ
````

It seems like the choice of `+ℕ-≡ℕ-extract-suc-r` would have worked better if I defaulted to perform case analysis on `b` instead of `a`. But I've been trying to be consistent about this and always prefer the first one, unless I have to make a different choice.

`+ℕ-≡ℕ-comm a (suc b)` should have the type

* `+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ (suc b)) ≡ℕ ((suc b) +ℕ a)`
* $\to$ `+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ (suc b)) ≡ℕ suc (b +ℕ a)`

Although we'll still have to deal with `suc (b +ℕ a)` in this case.

2026-09-13 Wk 37 Sun - 21:48 +03:00

Oops, should also avoid using `_` in like `heq_ab`. In Agda, `_` has special meaning. In this case, it allows input in place of `_`.

2026-09-13 Wk 37 Sun - 21:59 +03:00

````haskell
-- ℕ addition is commutative

+ℕ-≡ℕ-extract-suc-r : (a b : ℕ) → (a +ℕ (suc b)) ≡ℕ suc (a +ℕ b)
+ℕ-≡ℕ-extract-suc-r zero b = ≡ℕ-refl b
+ℕ-≡ℕ-extract-suc-r (suc a) b = +ℕ-≡ℕ-extract-suc-r a b

+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ b) ≡ℕ (b +ℕ a)
+ℕ-≡ℕ-comm zero b = ≡ℕ-sym (b +ℕ zero) b (+ℕ-≡ℕ-idr b)
+ℕ-≡ℕ-comm (suc a) b = h0
  where
    prev : suc (a +ℕ b) ≡ℕ suc (b +ℕ a)
    prev = +ℕ-≡ℕ-comm a b
      -- reduces by computation of ≡ℕ to `a +ℕ b ≡ℕ b +ℕ a`
    h0-heq-bb' : suc (b +ℕ a) ≡ℕ b +ℕ suc a
    h0-heq-bb' = ≡ℕ-sym (b +ℕ (suc a)) (suc (b +ℕ a)) (+ℕ-≡ℕ-extract-suc-r b a)
    h0 : suc (a +ℕ b) ≡ℕ b +ℕ suc a 
    h0 = ≡ℕ-rwr (suc (a +ℕ b)) (suc (b +ℕ a)) (b +ℕ suc a) h0-heq-bb' prev
      --   `suc (a +ℕ b) ≡ℕ suc (b +ℕ     a)`
      -- → `suc (a +ℕ b) ≡ℕ      b +ℕ suc a`
````

This did it! Starting from `prev`, which is the prior inductive case, resolves the flip problem for us. Then it's just a matter of using `h0` to turn `suc (b +ℕ a)` into the form yet-to-extract suc, and we're at our inductive case `+ℕ-≡ℕ-comm (suc a) b`!

2026-09-13 Wk 37 Sun - 23:46 +03:00

This was my previous solution from some months back:

````haskell
+ℕ-≡ℕ-a+Sb : (a b : ℕ) → (a +ℕ (suc b)) ≡ℕ suc (a +ℕ b)
+ℕ-≡ℕ-a+Sb zero b = ≡ℕ-refl b
+ℕ-≡ℕ-a+Sb (suc a) b = +ℕ-≡ℕ-a+Sb a b

+ℕ-≡ℕ-comm : (n m : ℕ) → (n +ℕ m) ≡ℕ (m +ℕ n)
+ℕ-≡ℕ-comm zero m = ←m≡m (≡ℕ-refl m)
  where
    ←m≡m : m ≡ℕ m → m ≡ℕ m +ℕ zero
    ←m≡m H₀ = ≡ℕ-sym (m +ℕ zero) m (+ℕ-≡ℕ-idr m)
+ℕ-≡ℕ-comm (suc n) m = ←H₀ (+ℕ-≡ℕ-comm n m)
  where 
    ←H₁ : (n +ℕ m ≡ℕ m +ℕ n) → (m +ℕ suc n ≡ℕ suc (m +ℕ n)) → (m +ℕ suc n ≡ℕ suc (n +ℕ m))
    ←H₁ H₀ H₁ = ≡ℕ-rwr (m +ℕ suc n) (suc (m +ℕ n)) (suc (n +ℕ m)) (≡ℕ-sym (n +ℕ m) (m +ℕ n) H₀) H₁
    ←H₀ : (suc (n +ℕ m) ≡ℕ suc (m +ℕ n)) → (suc (n +ℕ m) ≡ℕ m +ℕ suc n)
    ←H₀ H₀ = ≡ℕ-sym (m +ℕ suc n) (suc (n +ℕ m)) (←H₁ H₀ (+ℕ-≡ℕ-a+Sb m n))
````

More involved machinery, and multiple `≡ℕ-sym`. My current route is cleaner.
