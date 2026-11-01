---
context_type: entry
---

Parent: [000 Cubical Analysis I](../000%20Cubical%20Analysis%20I.md)

Spawned by: [000 Cubical Analysis I Thought Stream](000%20Cubical%20Analysis%20I%20Thought%20Stream.md)

Spawned in: [^spawn-entry-c8e4b5](000%20Cubical%20Analysis%20I%20Thought%20Stream.md#spawn-entry-c8e4b5)

Next: [005 Cubical Analysis I Thought Stream 26 Wk 43](005%20Cubical%20Analysis%20I%20Thought%20Stream%2026%20Wk%2043.md)

# Journal

2026-10-14 Wk 42 Wed - 11:14 +03:00

I do wonder where I moved the rocq files for Analysis I. I at least still have them here:

https://github.com/LanHikari22/lan-exp-scripts/tree/main/files/2026/moved_out/003-analysis-I-terece-tao/analysis_coq

Let's look at how we defined the equivalence for the integers.

https://github.com/LanHikari22/lan-exp-scripts/blob/e4918ead7e5d18e5b54af4c1b78ca09b0699a60b/files/2026/moved_out/003-analysis-I-terece-tao/analysis_coq/Z.v#L24

````
(*
  a - b = c - d
  a     = c - d + b
  a + d = c + b
*)
Definition z_eq (a b : Z) : Prop :=
  pos a + neg b = pos b + neg a.

````

We say it's a proposition, and it's defined as this type `pos a + neg b = pos b + neg a`.

Now whenever we state `z_eq` it just is that we're stating the prior equality proof obligation.

So it's a new relation that we define. It takes data, and outputs a proposition.

We could do a similar thing if we wanted in cubical agda. Maybe something like this:

````haskell
ℤᵣ_eq : (a b : ℤᵣ) → Type ℓ-zero
ℤᵣ_eq a b = (pos a) + (neg b) ≡ (pos b) + (neg a)
````

And then we go on and prove reflexivity, symmetry, and transitivity on the inputs to this path definition. But this would be an issue for us because we're allowing operations that use `ℤᵣ`  to not respect this equality. So it would be better to do `a ≡ b` here and have this definition be baked into the type instead.

2026-10-14 Wk 42 Wed - 11:36 +03:00

````haskell
-- [book] Proposition 2.2.12 (b) (Order is transitive): 
-- ∀ (a b c : ℕ) → (p : a ≤ b) → (q : b ≤ c) → a ≤ c

≤ℕ-trans :
    (a b c : ℕ)
  → (p : a ≤ℕ b)
  → (q : b ≤ℕ c)
  → a ≤ℕ c
≤ℕ-trans a b c a≤b b≤c .fst = {!!}
≤ℕ-trans a b c a≤b b≤c .snd = {!!}
````

Hmm. I remember It could be hard to know what to fill in for the witness at first.

https://github.com/LanHikari22/lan-exp-scripts/blob/e4918ead7e5d18e5b54af4c1b78ca09b0699a60b/files/2026/moved_out/003-analysis-I-terece-tao/analysis_coq/Nat.v#L641

In here, I just provided a witness.

but here is an example where I would introduce it as a parameter temporarily, to simplify, and then provide the witness.

https://github.com/LanHikari22/lan-exp-scripts/blob/e4918ead7e5d18e5b54af4c1b78ca09b0699a60b/files/2026/moved_out/003-analysis-I-terece-tao/analysis_coq/Z.v#L2287

2026-10-14 Wk 42 Wed - 12:07 +03:00

Yup, this reverse-discovery strategy works here too!

````haskell
-- [book] Proposition 2.2.12 (b) (Order is transitive): 
-- ∀ (a b c : ℕ) → (p : a ≤ b) → (q : b ≤ c) → a ≤ c

≤ℕ-trans :
    (a b c : ℕ)
  → (p : a ≤ℕ b)
  → (q : b ≤ℕ c)
  → a ≤ℕ c
≤ℕ-trans a b c a≤b b≤c .fst = (a≤b .fst) +ℕ (b≤c .fst)
  -- where
    -- simplifying to get the existence witness
    -- witness : (n₃ : ℕ) → c ≡ a +ℕ n₃
    -- witness n₃ =
    --   c
    --     ≡⟨ b≤c .snd ⟩
    --   b +ℕ (b≤c .fst)
    --     ≡⟨ ap (_+ℕ (b≤c .fst)) (a≤b .snd) ⟩
    --   a +ℕ (a≤b .fst) +ℕ b≤c .fst
    --     ≡⟨ {- OK -} ⟩
    --   a +ℕ n₃
    --     ∎
≤ℕ-trans a b c a≤b b≤c .snd =
  c
    ≡⟨ b≤c .snd ⟩
  b +ℕ (b≤c .fst)
    ≡⟨ ap (_+ℕ (b≤c .fst)) (a≤b .snd) ⟩
  a +ℕ (a≤b .fst) +ℕ (b≤c .fst)
    ≡⟨ sym (+ℕ-assoc a (a≤b .fst) (b≤c .fst)) ⟩
  a +ℕ ((a≤b .fst) +ℕ (b≤c .fst))
    ∎
````

2026-10-15 Wk 42 Thu - 02:32 +03:00

````haskell
-- Let's get some syntax for chaining `subst`.

module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {a â : A}
  where
    step-subst :
        (B : A → Type ℓ₂)
      → (p : a ≡ â)
      → (b : B a)
      → B â
    step-subst B p b = subst B p b

    syntax step-subst B p b = B subst⟨ p ⟩ b

    infixr 2 step-subst

module _
  {ℓ : Level}
  where
    _∎subst_ : (B : Type ℓ) → B → B
    B ∎subst b = b

    infix 3 _∎subst_
````

````haskell
        exp : (a +ℕ zero) ≤ℕ (b +ℕ zero)
        exp =
          (λ s → (a +ℕ zero) ≤ℕ s)
            subst⟨ (sym (+ℕ-idr b)) ⟩
          (λ s → s ≤ℕ b)
            subst⟨ (sym (+ℕ-idr a)) ⟩
          (a ≤ℕ b)
            ∎subst a≤b
````

This seems to work alright for chain substitutions. You can see the full type all the way at the top and all the way at the bottom, and otherwise infer from the shifting of the substitution areas `s`.

In any subst path obligation, you can infer what you need from the entry below you and above you.

It should be more organized than this

````haskell
h1 : (a +ℕ zero) ≤ℕ b
h1 = subst (λ s' → s' ≤ℕ b) (sym (+ℕ-idr a)) a≤b
h0 : (a +ℕ zero) ≤ℕ (b +ℕ zero)
h0 = subst (λ s' → (a +ℕ zero) ≤ℕ s') (sym (+ℕ-idr b)) h1
````

2026-10-15 Wk 42 Thu - 03:13 +03:00

You can make those by beginning like this

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → (a +ℕ suc c) ≤ℕ (b +ℕ suc c))
  (λ s → (a +ℕ suc c) ≤ℕ (b +ℕ suc c))
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

So long as we always are sandwiched between 3 entries, we can modify the middle to choose where to place the substitution `s`, and then modify the top accordingly.

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → suc (a +ℕ c) ≤ℕ (b +ℕ suc c))
  (λ s → s ≤ℕ (b +ℕ suc c))
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

Like this. Then we continue on, we need the top one to also be sandwiched so we copy it up.

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → suc (a +ℕ c) ≤ℕ (b +ℕ suc c))
  (λ s → suc (a +ℕ c) ≤ℕ (b +ℕ suc c))
  (λ s → s ≤ℕ (b +ℕ suc c))
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

Now we can make a choice for `s` and offer the changed type.

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → suc (a +ℕ c) ≤ℕ suc (b +ℕ c))
  (λ s → suc (a +ℕ c) ≤ℕ s)
  (λ s → s ≤ℕ (b +ℕ suc c))
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

When it's exactly the type of the goal, we omit it.

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → suc (a +ℕ c) ≤ℕ s)
  (λ s → s ≤ℕ (b +ℕ suc c))
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

Then we just place the subst path holes between the substitution types.

````haskell
a+c≤b+c₂ : suc (a +ℕ c) ≤ℕ suc (b +ℕ c)
a+c≤b+c₂ = 
  (λ s → suc (a +ℕ c) ≤ℕ s)
    subst⟨ {!!} ⟩
  (λ s → s ≤ℕ (b +ℕ suc c))
    subst⟨ {!!} ⟩
  (a +ℕ suc c) ≤ℕ (b +ℕ suc c)
    ∎subst a+c≤b+c
````

So a path transform is always down between a below and an above entry.

--/ 2026-10-16 Wk 42 Fri - 06:11 +03:00

I also added `subst-refl` so that we can substitute in the same location twice. It is identical to `∎subst`,
but it is more like a checkpoint or non-computational node in the chain, so I gave it a different name.

`∎subst` is responsible for supplying us with the first definition to be substituted to the goal from down to up.

--/ 2026-10-18 Wk 42 Sun - 08:17 +03:00

(Note the following example is faulty, which is explained below, but here we explore the method of building chains with `subst-refl`.)

We can use `subst-refl` to build a subst chain by giving each each of definition being transformed like this:

````haskell
m'≤m : m' ≤ℕ m
m'≤m = 
  (m' ≤ℕ m)
    subst-refl
  (m' <ℕ m)
    subst-refl
  (suc m' <ℕ suc m)
    subst-refl
  (m' ≤ℕ suc m)
    ∎subst m'≤sm 
````

Then we can fill in the modified regions and the path for the modification under each `subst-refl`.

````haskell
m'≤m : m' ≤ℕ m
m'≤m = 
  (m' ≤ℕ m)
    subst-refl
  (λ s → s)
    subst⟨ {!!} ⟩
  (m' <ℕ m)
    subst-refl
  (λ s → s)
    subst⟨ {!!} ⟩
  (suc m' <ℕ suc m)
    subst-refl
  (λ s → s)
    subst⟨ {!!} ⟩
  (m' ≤ℕ suc m)
    ∎subst m'≤sm 
````

In this case the substitution has always been over the whole definition, but it can apply in cases where this is not so. It looks longer, but now each stage is clearly spelled out and it can easily deal with substitution happening to the whole definition, or in the same spot twice.

The last `subst-refl` just gives us the goal, we can keep or omit it, these `subst-refl` just pass what's below them over, so they won't add any computational complexity for us, but they give type information at each stage which can be necessary for entries below the one at the goal.

Though I made a mistake in this particular instance, and you can't fill this:

````haskell
  (suc m' <ℕ suc m)
    subst-refl
  (λ s → s)
    subst⟨ {!!} ⟩
  (m' ≤ℕ suc m)
````

The condition instead would require `suc m' ≤ℕ suc m` -> `m' <ℕ suc m`.

But I wanted to write on the method of writing the chains using `subst-refl` type checkpoints here, and the faulty points shouldn't impede on that.

--/ 2026-10-18 Wk 42 Sun - 08:53 +03:00

If we want to be type checking in each step, we can add

````haskell
(λ s → s)
  subst⟨ {!!} ⟩
````

between each stage like here:

````haskell
m'<m : m' ≤ℕ m
m'<m =
  (λ s → s)
    subst⟨ {!!} ⟩
  ((suc m') ≤ℕ (suc m))
    subst-refl
  (λ s → s)
    subst⟨ {!!} ⟩
  (m' <ℕ (suc m))
    ∎subst m'<sm
````

Then we can refine the `(λ s → s)` substs we've added, either by filling in a path, or refining it to only replace a part of the definition and then filling in a path.

--/

2026-10-16 Wk 42 Fri - 06:09 +03:00

Reusing `isProp-¬` instead:

````haskell
isProp-≠ℕ :
    (a b : ℕ)
  → isProp (¬ a ≡ b)
-- isProp-≠ℕ a b ¬p₀ ¬p₁ i p̂ = isProp-∅ (¬p₀ p̂) (¬p₁ p̂) i
isProp-≠ℕ a b ¬p₀ ¬p₁ = isProp-¬ {A = a ≡ b} ¬p₀ ¬p₁
````

I recalled I reproved it when I did this, and I thought it would be better to be able to directly prove the negation of any type.

````haskell
pr1 : isProp (¬ A × B)
pr1 x₀ x₁ = isProp-¬ {A = A × B} x₀ x₁
````

2026-10-16 Wk 42 Fri - 09:06 +03:00

````
    ℕ-strong-ind zero (suc m) f = f (suc m) (≤ℕ-zero≤ (suc m)) f'
````

It seems you can change that into

````
    ℕ-strong-ind zero (suc m) f = {!f (suc m) (≤ℕ-zero≤ (suc 9)) f'!}
````

And it would let you do `C-c C-.` on it, and it would have the same type `A (suc m)`. But it will reject to refine.

2026-10-16 Wk 42 Fri - 11:33 +03:00

````rocq
# in gh/LanHikari22/lan-exp-scripts/2026/files/moved_out/003-analysis-I-terece-tao/analysis_coq/Nat.v
Definition nat_strong_induction : forall p : nat -> Prop,
  (forall n, (forall m, n > m -> p m) -> p n) ->
  forall n, p n.
````

So in RocQ I've proved it for the strong induction principle for ℕ only when specializing `m₀ = 0`.
