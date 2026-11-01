---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-c4ff1d](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-c4ff1d)

# Journal

2026-09-20 Wk 38 Sun - 21:19 +03:00

* [021 CQTS Problems Attempt Fragments J-ump-≃](021%20CQTS%20Problems%20Attempt%20Fragments%20J-ump-%E2%89%83.md)

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
