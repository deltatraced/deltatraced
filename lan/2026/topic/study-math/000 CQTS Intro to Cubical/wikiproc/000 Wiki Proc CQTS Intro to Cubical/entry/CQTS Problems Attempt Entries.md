---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned in: [^spawn-entry-c06dcc](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md#spawn-entry-c06dcc)

# What?

This includes partial attempt and interpretation for a given problem, and offers a space to store partial solutions.

It should have one subheading 2 per problem per *attempt* as an incrementing counter, since we can try multiple times in different ways, and draw different conclusions about our attempt.

Supplementary material per attempt might also be found at [010 CQTS Problems Attempt Fragments](010%20CQTS%20Problems%20Attempt%20Fragments.md)

Commentary or explanations after a solution was found can be found at [005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md).

# Journal

## `+ℕ-≡ℕ-comm` Attempt 000

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

## `→-map-≃`

2026-09-18 Wk 38 Fri - 08:28 +03:00

Pushed attempt to `/home/lan/src/cloned/cb/lan22h-experiments/note-files/paste/cc8a4d38`.
(Name is originally based on `sha1sum | head -c8`, but content can change afterwards. We can also just use `cat /dev/random | head -n10 | sha1sum | head -c8`.)

Expanding `fro-to` doesn't seem to help much here. Resetting back to doing `fro-to` without trying to expand the type out.

2026-09-18 Wk 38 Fri - 08:36 +03:00

````haskell
fro-to : to hasRetract ret
fro-to = {!!}
````

````
Goal: (b : B → C) →
      (λ a →
         ecd .proof .retract .map
         (ecd .map (b (eab .map (eab .proof .retract .map a)))))
      ≡ b
````

2026-09-18 Wk 38 Fri - 09:19 +03:00

````haskell
fro-to : to hasRetract ret
fro-to mbc i b = (ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i
````

````
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-2-equivalences-and-path-algebra.agda:566.9-23: error: [UnequalTerms]
The terms
  eab .proof .retract .map b
and
  eab .proof .section .map b
are not equal at type A
when checking that a clause of fro-to has the correct boundary.

Specifically, the terms
  ret eab ecd (to eab ecd mbc) b
and
  ecd .proof .retract .proof (mbc (eab .proof .section .proof b i0))
  i0
must be equal, since fro-to eab ecd mbc i0 b could reduce to
either.

````

It gives a similar goal, but actually uses `eab .proof .section` instead of `eab .proof .retract`:

````haskell
fro-to : to hasRetract ret
fro-to mbc i b = {!(ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i!}
````

````
Goal: C
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ ecd .proof .retract .map
         (ecd .map (mbc (eab .map (eab .proof .retract .map b))))
i = i1 ⊢ mbc b
Have: C
———— Boundary (actual) —————————————————————————————————————
i = i0 ⊢ ecd .proof .retract .map
         (ecd .map (mbc (eab .map (eab .proof .section .map b))))
i = i1 ⊢ mbc b
———— Context ———————————————————————————————————————————————
````

So this can be an issue with how we wrote `ret`:

````haskell
ret : (A → D) → (B → C)
ret mad = (ecd .proof .retract .map) ∘ mad ∘ (eab .proof .retract .map)
````

This works:

````haskell
ret : (A → D) → (B → C)
ret mad = (ecd .proof .retract .map) ∘ mad ∘ (eab .proof .section .map)

fro-to : to hasRetract ret
fro-to mbc i b = (ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i
````

## `subst`

2026-09-19 Wk 38 Sat - 21:07 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-3-substitution-and-J.agda`,

````haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {a a' : A}
  where
    subst :
      (B : A → Type ℓ₂)
      → (p : a ≡ a')
      → B a
      → B a'
    subst B p b = {!transport (λ (i : I) → B (p i)) b!}
````

This fails to be resolved. `C-c C-.` gives

````
error: [UnequalTypes]
The type
  (i : I) → Type ℓ₂
is not a subtype of
  _A_20 ≡ _B_21
when checking that the expression λ (i : I) → B (p i) has type
_A_20 ≡ _B_21
````

````haskell
subst B p b = {!transport (λ (i) → B (p i)) b!}
````

yields

````
error: [PatternInPathLambda]
Patterns are not allowed in Path-lambdas
when checking that the expression λ (i) → B (p i) has type
_A_20 ≡ _B_21
````

Finally

````haskell
subst B p b = {!transport (λ i → B (p i)) b!}
````

yields `Have: B a'`.

It seems that paths from `i` are treated specially here.

Though sometimes I can get away with specifying `(_ : I)`. for example like here:

````haskell
module _
  {ℓ : Level}
  {A B : Type ℓ}
  where
    -- transport is derived through the more general transport-fixing
    transport : A ≡ B → A → B
    transport p a = transport-fixing (λ (i : I) → p i) i0 a
````

Here too

````haskell
subst B p b = {!(λ (i : I) → p i) i1!}
````

giving `Have: A`

## `J-line`

2026-09-19 Wk 38 Sat - 21:07 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-3-substitution-and-J.agda`,

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a â : A}
  where
    J-line : 
        (Q : (a' : A) → a ≡ a' → Type ℓ)
      → (p : a ≡ â)
      → Q a refl ≡ Q â p
    J-line Q p i = {!!}
````

````
Goal: Type ℓ
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ Q a refl
i = i1 ⊢ Q â p
````

We want to show what the square for this is.

So far we have a square that is at least

````
--                ?
--         a₀₁ — — — > a₁₁       
--          ^           ^          ^
-- Q a refl |   Type ℓ  | Q â p  j |
--          |           |          . — >
--         a₀₀ — — — > a₁₀           i
--                ?    
--   a₀₀ a₀₁ a₁₀ a₁₁ = ?
````

This looks like it can derive from a square by construction via application of `Q a`:

````
--                ?
--         a₀₁ — — — > a₁₁       
--          ^           ^          ^
--     refl |     A     | p      j |
--          |           |          . — >
--         a₀₀ — — — > a₁₀           i
--                ?    
--   a₀₀ a₀₁ a₁₀ a₁₁ = ?
````

Treating this as the goal, we want to create a square that gives us `refl` at `i=i0` and `p` at `i=i1`.

Recall that `refl` indicates that the endpoints are constant with regard to some path. And `∧ ∨` short circuit to constant paths for some endpoints.
i0 for `∧` and i1 for `∨`.

We know one of the constant paths is at `i=i0`, so we expect `∧`:

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a â : A}
  {a₀₀ a₁₀ a₀₁ a₁₁ : A} 
  where
    target-square :
        (p : a ≡ â)
      → Square A refl p refl p
    target-square p i j = p (i ∧ j)
````

````
--                p
--         a₀₁ — — — > a₁₁       
--          ^           ^          ^
--     refl |     A     | p      j |
--          |           |          . — >
--         a₀₀ — — — > a₁₀           i
--              refl    
````

This choices turns out to be `connection∧`:

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a b : A}
  where
    --            p 
    --      a  — — — >  b        
    --      ^           ^          ^
    -- refl |     A     | p      j |
    --      |           |          . — >
    --      a  — — — >  a            i
    --          refl
    connection∧ :
      (p : a ≡ b)
      → Square A refl p refl p
    connection∧ p i j = p (i ∧ j)
````

A square allows us to construct a path of paths of interest, like from refl to p.

## `J-ump-≃`

2026-09-20 Wk 38 Sun - 05:37 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-3-substitution-and-J.agda`,

ncf gave me a hint before on this some months back, but let's try to solve it again before we check that again, I might remember.

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ â : A}
  where
    J-ump-≃ :
        (Q : (a : A) → a̬ ≡ a → Type ℓ)
      → ((a : A) → (p : a̬ ≡ a) → Q a p) ≃ (Q a̬ refl)
    J-ump-≃ Q = packEquiv to sec to-fro ret fro-to
      where
        to : ((a : A) → (p : a̬ ≡ a) → Q a p) → (Q a̬ refl)
        to f = f a̬ refl

        sec : (Q a̬ refl) → ((a : A) → (p : a̬ ≡ a) → Q a p)
        sec r a p = J Q r p

        to-fro : to hasSection sec
        to-fro r i = J-refl Q r i

        ret : (Q a̬ refl) → ((a : A) → (p : a̬ ≡ a) → Q a p)
        ret r a p = J Q r p

        fro-to : to hasRetract ret
        fro-to f i a p = {! !}
          where
            r : Q a̬ refl
            r = f a̬ refl
````

````haskell
fro-to f i a p = {! !}

-- goal
Goal: Q a p
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ ret (to f) a p
i = i1 ⊢ f a p

-- goal (simplifying)
Goal: Q a p
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ transport-fixing (λ i₁ → Q (p i₁) (λ j → p (i₁ ∧ j))) i0
         (f a̬ (λ i₁ → a̬))
i = i1 ⊢ f a p
````

The idea is we can design a `Q` that simplifies this retract proof.

From the above goal, we can see that `transport-fixing (λ i₁ → Q (p i₁) (λ j → p (i₁ ∧ j))) i0` is our definition of `J`. We want:

````haskell
fro-to : to hasRetract ret
fro-to f i a p = h0 i
  where
	r : Q a̬ refl
	r = f a̬ refl
	h0 : J Q r p ≡ f a p
	h0 = {!!}
````

Now we want to use `J` to solve this. Having `J` as the path induction has it so it suffices to solve a simpler version, with `refl` instead of `p`.

2026-09-20 Wk 38 Sun - 21:19 +03:00

* \[\[010 CQTS Problems Attempt Fragments#`J-ump-≃` - Side Notes\]\]
* \[\[010 CQTS Problems Attempt Fragments#`J-ump-≃` - Tried to extend the type of Q\]\]
* \[\[010 CQTS Problems Attempt Fragments#`J-ump-≃` - Q might already take into account path and refl variance\]\]

2026-09-21 Wk 39 Mon - 02:32 +03:00

Yup, just choosing a `Q` that that can resolve to the path of `h1` or `h0` via `J` solves this:

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ : A}
  where
    J-ump-≃ :
        (Q : (a : A) → a̬ ≡ a → Type ℓ)
      → ((a : A) → (p : a̬ ≡ a) → Q a p) ≃ (Q a̬ refl)
    J-ump-≃ Q = inv→equiv to inv to-fro fro-to
      where
        to : ((a : A) → (p : a̬ ≡ a) → Q a p) → (Q a̬ refl)
        to f = f a̬ refl

        inv : (Q a̬ refl) → ((a : A) → (p : a̬ ≡ a) → Q a p)
        inv r a p = J Q r p

        to-fro : to hasSection inv
        to-fro r i = J-refl Q r i

        fro-to : to hasRetract inv
        fro-to f i a p = h0 i
          where
            r : Q a̬ refl
            r = f a̬ refl
            h1 : Path (Q a̬ refl) (J Q r refl) r
            h1 i = J-refl Q r i
            Q₁ : (a' : A) → a̬ ≡ a' → Type ℓ
            Q₁ a' p' = Path (Q a' p') (J Q r p') (f a' p')
            h0 : Path (Q a p) (J Q r p) (f a p)
            h0 = J Q₁ h1 p
````

Now let's check out our prior solution and what the hint we got was.

Creating a random file name: `cat /dev/random | head -n1 | sha1sum | head -c8`

The previous attempt is in `/home/lan/src/cloned/cb/lan22h-experiments/note-files/paste/5ced8d7f`.

````haskell
--  Exercise: (Hint: this is an instance of `J-refl`)
    to-fro : isSection to fro
    to-fro q = J-refl Q q
````

We got this.

````haskell
--  Exercise: (Hint: use `J` again!)
    fro-to : isRetract to fro
    fro-to f i y p = P₀ i
      where 
        P₁ : Path (Q x refl) (J Q (f x refl) refl) (f x refl) -- hint
        P₁ = J-refl Q (f x refl)
        P₀ : Path (Q y p) (J Q (f x refl) p) (f y p) -- hint
        P₀ = J (λ y₁ p₁ → Path (Q y₁ p₁) (J Q (f x refl) p₁) (f y₁ p₁)) P₁ p
````

Pretty much the same solution, although I've done more abstractions in my case, while here I did it more inline.

## `≡≃≡⊤`

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

## `_∙∙_∙∙_`

2026-09-22 Wk 39 Tue - 07:03 +03:00

in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

in `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-4--Composition-and-Filling.lagda.md`,

````haskell
 _∙∙_∙∙_ :
     (r : a ≡ b)
   → (p : b ≡ c)
   → (q : c ≡ d)
   → (a ≡ d)
 (r ∙∙ p ∙∙ q) i = hcomp (∂ i) ((double-comp-box r p q i))    diamond-tube-alt p q i j k (i = i0) = {!!}
    diamond-tube-alt p q i j k (i = i1) = {!!}

````

Why is it that hcomp for this only takes an interval formula that is i1 at (i=i0) and (i=i1), when the open box also has (j=i0) as a side?

CQTS Authors explain:

````
Writing more formally, ``hcomp`` takes in two arguments:

* A formula `φ : I`, specifying which sides of the box are going to be
  present in the next argument.
  
  For the double composite, we are specifying the left and right sides
  of the square, that is, the places where where `∂ i` holds.
  
* An open box `box : (j : I) → Partial (φ ∨ ~ j) A`. We can think of
  `box` as specifying a vertical slice of the open box as `j` runs
  from ``i0`` to ``i1``. Each vertical slice will be defined in more
  or fewer places depending on the formula `φ`. When `j` is ``i0``,
  the formula `φ ∨ ~ j` always holds and so these vertical slices are
  all defined when `j = i0`.

  For the double composite, `box j` is totally defined when `i = i0`,
  because we are on the left side of the square in that case, and
  similarly when `i = i1` and we are on the right side of the square.
  Otherwise, we only know for sure that `box j` is defined when `j =
  i0`, in which case we are on the bottom of the square.
````

Well box is `box : (i j : I) → Partial (open-box i j) A`, it seems they mean this when evaluated at `i` to the first argument of hcomp `∂ a`:

* `box : (i j : I) → Partial ((~ i) ∨ i ∨ (~ j)) A`
* $\to$ `box : (j : I) → Partial (∂ i ∨ (~ j)) A`

The other thing is that ` _∙∙_∙∙_` is a 1-cube path. In its very definition, it could not supply us with more than one interval.

````
--                  
--         a  .......  d        
--         ^           ^              ^
--   sym r |     A     | q          j |
--         |           |              . — >
--         b  — — — >  c                i
--              p   
````

So maybe we should interpret the the first argument of `hcomp` as specifying the endpoints of the lid 1-cube to fill in, which runs from the face (i = i0) to the face (i = i1).

They did mention that `box` slides from `j = i0` to `j = i1`...

At `i = i0` `box j` should resolve to `(sym r) j`, and at `i = i1` `box j` should resolve to `q j`.

So it seems for any partial n-cube open box, in order to perform hcomp on it (close the lid), we need to specify as the first argument of hcomp a formula for an (n-1) cube face sliding in the direction of the open (n-1) cube face.

In our case this is a formula of 1-cube faces, sliding in the direction of the open lid (i = i0 $\to$ i = i1). So one variable is always left unspecified in the first argumen: the one that is defined as the upper surface of the sweep.

Then because only one missing variable remains in `box j` (no matter the n-dimension of the subcube), its formula should always be `(φ ∨ ~ j)`, the second argument expects our third defined case already as the only remaining pattern `(j = i0)` we need to define to close the lid and perform the sweep.

If we omit the j case with this bad `double-comp-box'`, it will complain about it:

````haskell
    double-comp-box' :
        (r : a ≡ b)
      → (p : b ≡ c)
      → (q : c ≡ d)
      → (i j : I)
      → Partial (∂ i) A
    double-comp-box' r p q i j (i = i0) = sym r j
    double-comp-box' r p q i j (i = i1) = q j

    -- We start with the double composition as it readily forms closing the lid
    -- on an open box.
    -- hcomp takes a formula for the *sides* 
    _∙∙_∙∙_ :
        (r : a ≡ b)
      → (p : b ≡ c)
      → (q : c ≡ d)
      → (a ≡ d)
    (r ∙∙ p ∙∙ q) i = hcomp (∂ i) ((double-comp-box' r p q i))
							--      ~~~~~~~~~~~~~~~~~~~~~~~~ problem
````

````
The terms
  (i ∨ ~ i) ∨ ~ j
and
  i ∨ ~ i
are not equal at type I
when checking that the expression double-comp-box' r p q i has type
(j : I) → Partial (∂ i ∨ ~ j) _A_301
````

Another way to state it is that the formula φ specifies the boundaries of the lid subcube. For a line, those are the endpoints, but for a square it would require 2 interval variables to get its boundaries within the cube.

And because the result is a face, the number of intervals we have access to is only as much as (n-1)-cube, hence we can't specify the extra bottom face there anyway.

2026-09-22 Wk 39 Tue - 12:17 +03:00

Seeing it as specifying the boundaries also helps explain why for `∙∙-filler` we do not include all sides, only `~ i ∨ ∂ j`, since we do not specify a face at (i = i1), leaving it to hcomp to fill it by inferring the shared line between `(k = i1)` and `(i = i1)`, so that boundary is missing in the formula as a result.

2026-09-22 Wk 39 Tue - 08:45 +03:00

https://mzhang.io/posts/2024-09-18-hcomp/indexlagda/

## transport-cancel

2026-09-23 Wk 39 Wed - 06:06 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-5-transport.agda`,

````haskell
module _
  {ℓ : Level}
  {A B : Type ℓ}
  where
    transport-cancel :
        (p : A ≡ B)
      → (b : B)
      → transport (λ i → p i) (transport (λ i → (sym p) i) b) ≡ b
    transport-cancel p b i = 
      transport-fixing (λ j → p (i ∨ j)) i
     (transport-fixing (λ j → (sym p) (~ i ∧ j)) i b)
````

As with the hints in the CQTS lecture notes, we had to design this transport of transport expression directly with uses of cubical intervals that make it reduce.

````haskell
    transport-cancel p b i = {!
      transport-fixing (λ j → p (i ∨ j)) i
     (transport-fixing (λ j → (sym p) (~ i ∧ j)) i b)!}

Goal: B
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ transport-fixing (λ i₁ → p i₁) i0
         (transport (λ i₁ → sym p i₁) b)
i = i1 ⊢ b
Have: B
———— Boundary (actual) —————————————————————————————————————
i = i0 ⊢ transport-fixing (λ j → p j) i0
         (transport-fixing (λ j → sym p j) i0 b)
i = i1 ⊢ b
````

One key to this is that since we want it to be constant at `i = i1`, we specified the subcube formula `i`. And in the first transport, to make it only reduce to a constant at `i = i1`, an `∨ ` connective does this.

This wouldn't compile if we don't handle the second choice of intervals right at `(transport-fixing (λ j → (sym p) (~ i ∧ j))`. We were already expecting just `(sym p) j`, but we allow it to short at `i = i1` with `~ i ∧ _`.

Bit experimental, but I think I did it easier than last time.
