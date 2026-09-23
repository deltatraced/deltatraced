---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned in: [^spawn-entry-3cfc10](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md#spawn-entry-3cfc10)

# What?

Comments and thoughts on CQTS Problems. One subheading under `# Journal` per problem or set of closely related problems.

If the effort and exploration for a given problem is long, it should be promoted to its own note file.

I also have [CQTS Problems Attempt Entries](CQTS%20Problems%20Attempt%20Entries.md), which also includes one subheading under `# Journal` per problem, but can include the interim while attempting thoughts and notes. It can include partial progress and interpretation.

# Journal

## `_+pos_` and `_+negsuc_`

2026-09-06 Wk 36 Sun - 09:56 +03:00

This is in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda`.

````haskell
_+pos_ : ℤ → ℕ → ℤ
a +pos zero = a
a +pos suc b = sucℤ (a +pos b)

-- pos a    +pos b = pos (a + b)
-- negsuc a +pos zero  = negsuc a
-- negsuc a +pos suc b = (sucℤ (negsuc a)) +pos b

_+negsuc_ : ℤ → ℕ → ℤ
a +negsuc zero = predℤ a
a +negsuc suc b = predℤ (a +negsuc b)

-- pos a +negsuc zero = predℤ (pos a)
-- pos a +negsuc suc b = predℤ ((pos a) +negsuc b)
-- negsuc a +negsuc b = {!!}
````

I figured instead of the commented out, it is shorter to solve this using our prior definitions of `predℤ` and `sucℤ`. Removing a suc from a `negsuc` results in rolling out one `predℤ` of the whole. And for the pos, it instead results in a `sucℤ`.

## `_·ℤ_`

2026-09-06 Wk 36 Sun - 11:14 +03:00

This is in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda`.

````lua
-- Hint: case split on only one of the sides 
_·ℤ_ : ℤ → ℤ → ℤ
pos zero ·ℤ b = pos zero
pos (suc a) ·ℤ b = b +ℤ ((pos a) ·ℤ b)
negsuc a ·ℤ b = - ((pos (suc a)) ·ℤ b)
````

This gives us an error.

````lua
-- Hint: case split on only one of the sides 
_·ℤ_ : ℤ → ℤ → ℤ
pos zero ·ℤ b = pos zero
pos (suc a) ·ℤ b = b +ℤ ((pos a) ·ℤ b)
negsuc a ·ℤ b = - ((pos (suc a)) ·ℤ b)
````

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1
mikan src/lect-1-2-inductive-types.agda

# out (relevant)
error: [TerminationIssue]
Termination checking failed for the following function:
  _·ℤ_
Problematic call:
  pos (suc a) ·ℤ b
    (at /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda:392.34-36)
````

I guess there is no structural decrement in the definition of `negsuc a ·ℤ b`, so it rejects it.

## `_⊎_`

2026-09-09 Wk 37 Wed - 20:11 +03:00

````haskell
data _⊎_
  {ℓ₁ ℓ₂ : Level}
  (A : Type ℓ₁)
  (B : Type ℓ₂)
  : Type (ℓ-max ℓ₁ ℓ₂)
  where
    inl : A → A ⊎ B
    inr : B → A ⊎ B
````

I wanted to write this myself without looking, to get used to this syntax of defining inductive data types.

The way I interpret this is (as of this writing):

* First, we need this to be universe-polymorphic. We specify automatically inferred parameters `ℓ₁ ℓ₂` for any universe level, and our type is in the max of the two, since it just lifts the lower one up by packaging it under `inl` or `inr`
* This can often look like function syntax, and we can think of `_⊎_` like an operator between two types, to define a new type for us (CQTS authors call it a *type former*). So that for any `A` and `B`, there could also be `A ⊎ B`.
  * I wanted to call this a type constructor, but it specifies the type constructors under `where`.
* I think of the `inl` and `inr` as not functions, so I'm not thinking of `inl` for example as a map out of `A` and into `A ⊎ B`. Instead, `inl` is type constructor. It describes a mathematical object, just like `zero` and `suc (zero)` does for the natural numbers, or `false` and `true` for booleans. This object, *to be specified*, requires as part of its definition a value of `A`. So there is no such thing as just `inl`, a full object is always an `inl a` for some `a : A`.
  * So what does the `→ A ⊎ B` bit mean? I read it "forms the given type". So `inl`, alongside with `A`, forms the given type `A ⊎ B`.

````haskell
data _⊎_
  {ℓ₁ ℓ₂ : Level}
  (A : Type ℓ₁)
  (B : Type ℓ₂)
  : Type (ℓ-max ℓ₁ ℓ₂)
-- Declares how we can specify `_⊎_`. I read it as requiring an A and a B.
-- `: Type (ℓ-max ℓ₁ ℓ₂)` I interpret as specifying that we're forming a new type, and it allows us to configure it for universe polymorphism

  where
-- v defines the constructors for _⊎_
    inl : A → A ⊎ B
    inr : B → A ⊎ B
````

Sometimes we write a map that outputs a `Type`:

````haskell
{C : (a : A) → (b : B a) → Type ℓ₃}
````

This I interpret as there being any definition of a type under `Type ℓ₃`. Here we may refer to `C a b`, but the code supplier can write any definition that fits the form of depending on a and `B a`. So `→ Type ℓ₃` here could also be read as yielding some definition of a type that depends on the prior `(a : A) → (b : B a)`.

## `¬-not-same`

2026-09-13 Wk 37 Sun - 17:48 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-5--Propositions-as-Types.lagda.md`,

````haskell
    ¬-not-same : ¬ (P iffP (¬ P))
    ¬-not-same (f , g) = f p p
      where
        p : P
        p = g (λ p → f p p)
````

I wasn't sure how to solve this for a bit, but it can be automatically solved in emacs with `C-c C-a`.

I was thinking I wasn't sure where to supply `p` to either `f` or `g`, but I don't have to. `g` takes a *function* so we can build one. That is, assuming `p`, produce a contradiction.

## `+ℕ-≡ℕ-comm`

2026-09-13 Wk 37 Sun - 21:03 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-5--Propositions-as-Types.lagda.md`,

\[\[CQTS Problems Attempt Entries#`+ℕ-≡ℕ-comm` Attempt 000\]\]

## `stretch-vertical`

2026-09-16 Wk 38 Wed - 06:16 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-1-paths.agda`,

````haskell
    --           p  
    --      a — — — > b
    --      ^         ^          ^
    -- refl |    A    | refl   j |
    --      |         |          . — >
    --      a — — — > b           i
    --           p  
    stretch-vertical :
      (p : a ≡ b)
      → Square A refl refl p p
    stretch-vertical p i j = p i
````

--/  2026-09-16 Wk 38 Wed - 06:17 +03:00

How do we interpret that this ignores the `j`? We can solve it by following the type checker, but how do we intuit it must be the case without it?

No matter what the value, a change in `j` will do nothing. Since we're only sensitive to horizontal change but not vertical, we see `p` down and below for `p i`.

Similarly `p j` would suggest that horizontal change is ignored, but vertical is the endpoints of `p`, so we would expect the flip:

````haskell
    --      refl  
    --   b — — — > b
    --   ^         ^       ^
    -- p |    A    | p   j |
    --   |         |       . — >
    --   a — — — > a         i
    --      refl  
    stretch-horizontal :
      (p : a ≡ b)
      → Square A p p refl refl
    stretch-horizontal p i j = p j
````

Since `Square` is defined via a square sweep, we can also think of `i` as picking the type of vertical slice paths, these are different in the case of `stretch-vertical` by the `p` endpoints, but nothing else differentiates them further, so then the vertical slice paths are refl.
--/

## `homotopy-path`

2026-09-16 Wk 38 Wed - 08:26 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-1-paths.agda`,

````haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {B : Type ℓ₂}
  {f g : A → B}
  {a a' : A}
  where
    --        ap g p
    --    g a — — — > g a'      
    --     ^           ^          ^
    -- H a |     B     | H a'   j |
    --     |           |          . — >
    --    f a — — — > f a'          i
    --        ap f p

    -- H is a homotopy between f and g
    homotopy-path :
      (H : (a : A) → f a ≡ g a)
      → (p : a ≡ a')
      → Square B (H a) (H a') (ap f p) (ap g p)
    homotopy-path H p i j = H (p i) j
````

One way to interpret the `H (p i) j` definition here is that we need to make a horizontal distinction first (a or a'?)  with `(p i)`.
`H (p i)` gives us one of two paths where we need to choose `f` or `g` and this distinction is then captured vertically with `j`.

## `connectionEx1`

2026-09-19 Wk 38 Sat - 03:28 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-2-equivalences-and-path-algebra.agda`,

````haskell
    --           p⁻¹ 
    --      b  — — — >  a        
    --      ^           ^          ^
    --    p |     A     | refl   j |
    --      |           |          . — >
    --      a  — — — >  a            i
    --          refl
    connectionEx1 :
      (p : a ≡ b)
      → Square A p refl refl (sym p)
    connectionEx1 p i j = p ((~ i) ∧ (j))
````

This could be solved by looking for short circuits:

````
———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ a
j = i1 ⊢ p (~ i)
i = i0 ⊢ p j
i = i1 ⊢ a
````

We want `j=i0` and `i=i1` to short to a constant. `∧` shorts for i0, so `∧ j` gets us that for `j=i0`. It would short for `i0`, but we do `(~ i)` so it shorts for `i1` instead.

Similarly for

````haskell
    --              p   
    --        a  — — — >  b        
    --        ^           ^          ^
    --  sym p |     A     | refl   j |
    --        |           |          . — >
    --        b  — — — >  b            i
    --            refl
    connectionEx2 :
      (p : a ≡ b)
      → Square A (sym p) refl refl p
    connectionEx2 p i j = {!p (i ∨ (~ j))!}
````

````
———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ b
j = i1 ⊢ p i
i = i0 ⊢ p (~ j)
i = i1 ⊢ b
````

We're shorting at `j=i0` and `i=i1` (read those as the formula for the faces, so `i=i0` is the vertical left, `i=i1` is vertical right, `j=i0` is horizontal bottom, and `j=10` is horizontal top)

`∨` shorts for i1, so it takes cae of `i=i1`, and we flip `(~ j)` so it shorts `j=i0` also.
