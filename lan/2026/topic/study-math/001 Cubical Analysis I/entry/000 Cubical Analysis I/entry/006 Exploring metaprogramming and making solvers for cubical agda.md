---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/001 Cubical Analysis I/entry/000 Cubical Analysis I/000 Cubical Analysis I]]

Spawned by: [[lan/2026/topic/study-math/001 Cubical Analysis I/entry/000 Cubical Analysis I/entry/005 Cubical Analysis I Thought Stream 26 Wk 43]]

Spawned in: [[lan/2026/topic/study-math/001 Cubical Analysis I/entry/000 Cubical Analysis I/entry/005 Cubical Analysis I Thought Stream 26 Wk 43#^spawn-entry-862439|^spawn-entry-862439]]

# Journal

2026-10-19 Wk 43 Mon - 14:36 +03:00

Coming from [[005 Cubical Analysis I Thought Stream 26 Wk 43]]

There it is, the addition commutativity/associativity permutations proof obligations issue.

```haskell
∙ℕ-distsOver-+ℕ-l :
  (a b c : ℕ)
  → a ∙ℕ (b +ℕ c) ≡ (a ∙ℕ b) +ℕ (a ∙ℕ c)
∙ℕ-distsOver-+ℕ-l zero b c = refl
∙ℕ-distsOver-+ℕ-l (suc a) b c =
  (a ∙ℕ (b +ℕ c)) +ℕ (b +ℕ c)
    ≡⟨ ap (_+ℕ (b +ℕ c)) (∙ℕ-distsOver-+ℕ-l a b c) ⟩
  (a ∙ℕ b) +ℕ (a ∙ℕ c) +ℕ (b +ℕ c)
    ≡⟨ {!!} ⟩
  (a ∙ℕ b) +ℕ b +ℕ ((a ∙ℕ c) +ℕ c)
    ∎
```

In RocQ I ran into more and more involved ones. So much so I made propositions like the below to deal with them.

```
Proposition add_comm_8_a0_swap_a1 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a1 + a0 + a2 + a3 + a4 + a5 + a6 + a7.

Proposition add_comm_8_a0_swap_a2 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a2 + a1 + a0 + a3 + a4 + a5 + a6 + a7.
  
Proposition add_comm_8_a0_swap_a3 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a3 + a1 + a2 + a0 + a4 + a5 + a6 + a7.

Proposition add_comm_8_a0_swap_a4 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a4 + a1 + a2 + a3 + a0 + a5 + a6 + a7.

Proposition add_comm_8_a0_swap_a5 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a5 + a1 + a2 + a3 + a4 + a0 + a6 + a7.

Proposition add_comm_8_a0_swap_a6 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a6 + a1 + a2 + a3 + a4 + a5 + a0 + a7.

Proposition add_comm_8_a0_swap_a7 : forall a0 a1 a2 a3 a4 a5 a6 a7 : nat, 
  a0 + a1 + a2 + a3 + a4 + a5 + a6 + a7 = a7 + a1 + a2 + a3 + a4 + a5 + a6 + a0.
```

Is there a way to avoid having to do these permutation puzzles? Can we make all permutations equal in some way?

2026-10-19 Wk 43 Mon - 14:46 +03:00

https://dl.acm.org/doi/10.1145/3703595.3705873

https://stackoverflow.com/questions/69957842/how-do-you-use-the-ring-solver-in-cubical-agda

What is a ring solver?

This gives an explanation of ring structures: https://1lab.dev/Algebra.Ring.html

We were just solving how multiplication distributes over addition for Nat, and already rings are a mathematical structure that requires distributivity of some multiplication operation on some addition operation for some type, but also other things, like being a commutative monoid and being an abelian group.

And this gives us an example of a solver, though for categories: https://1lab.dev/Cat.Solver.html

--/ 2026-10-20 Wk 43 Tue - 11:16 +03:00

https://gist.github.com/andrejbauer/358722620c26c09d6be218bcd95ee654

An example for using a ring solver already in the agda standard library

https://github.com/maxdore/csolver

fun someone made a solver for hcomps in haskell. In my case I'm interested in solving within agda, rather than making an external programs that writes agda proofs for me.

--/ 2026-10-20 Wk 43 Tue - 12:59 +03:00

https://agda.readthedocs.io/en/latest/language/reflection.html

Seems to have similar things that you find in the 1lab here: https://1lab.dev/Data.Reflection.Term.html

Tutorial for reflection in Agda: https://github.com/alhassy/gentle-intro-to-reflection

--/

2026-10-21 Wk 43 Wed - 18:23 +03:00

I'm able to get things like `Term` from `Reflection` under mikan:

```haskell
import Agda.Builtin.Reflection using (Term)
```

Though it conflicts with some definitions that I made in `cqts-bg` like for `Nat` currently.

I can also do `import Agda.Builtin.Unit` instead of `import Data.Unit`.

2026-10-22 Wk 43 Thu - 09:35 +03:00

For https://github.com/alhassy/gentle-intro-to-reflection,

I'm not able to find any `TakeWhile` or `toDec` in `/home/lan/.cabal/store/ghc-9.10.3-415c/Mikan-2.9.0-edc2b4172b82b9acc3925ac9c71ebd58e5383fdc77565ef7fb13e3e33852a2af/share/lib/prim/Agda/Builtin`.

Their example is

```haskell
module-name : String
module-name = takeWhile (toDec (λ c → not (c Char.== '.'))) ⟨𝒮⟩ showName (quote Red)
```

But this seems strange. Shouldn't we instead give `takeWhile` a map that, for any `a : A` gives us that `p a ≡ true` is decidable? Because this depends on each `a` in the list, not on the whole map being decidable. Let's do it that way.

Like this:

```haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {p : A → Bool}
  where
    takeWhile :
        (f : (a : A) → Dec (p a ≡ true))
      → List A
      → List A
    takeWhile f [] = []
    takeWhile f (a ∷ as) = h0 (f a)
      where
        h0 : Dec (p a ≡ true) → List A
        h0 (yes pa) = a :: takeWhile f as
        h0 (no ¬pa) = takeWhile f as
```

Then we can use this to give it maps turned into decidable to true procedures:

```haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  where
    Dec-≡true :
        (p : A → Bool)
      → ((a : A) → Dec (p a ≡ true))
    Dec-≡true p a = Dec≡-Bool (p a) true
```

Now we can do this:

```haskell
module-name : String
module-name = takeWhile (Dec-≡true (λ c → not (primCharEquality c '.')))
              ⟨𝒮⟩ primShowQName (quote Red)
			  
_ : module-name ≡ "mkn-indexRGBRed"
_ = {!!}
```

Though we made a mistake in the logic. I think `takeWhile` should stop once it encounters the first false condition. What I implemented was instead `filter`.

```haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {p : A → Bool}
  where
    takeWhile :
        (f : (a : A) → Dec (p a ≡ true))
      → List A
      → List A
    takeWhile f [] = []
    takeWhile f (a ∷ as) = h0 (f a)
      where
        h0 : Dec (p a ≡ true) → List A
        h0 (yes pa) = a :: takeWhile f as
        h0 (no ¬pa) = []

_ = test-identical (primShowQName (quote Red)) "mkn-index.RGB.Red"
_ = test-identical module-name "mkn-index"
```

--/ 2026-10-27 Wk 44 Tue - 00:05 +03:00

```haskell
    List-takeWhile :
        (f : (a : A) → Dec (p a ≡ true))
      → List A
      → List A
    List-takeWhile f [] = []
    List-takeWhile f (a :: as) = h0 (f a)
      where
        h0 : Dec (p a ≡ true) → List A
        h0 (yes pa) = a :: List-takeWhile f as
        h0 (no ¬pa) = []

    List-takeWhile' :
        (f : (a : A) → Bool)
      → List A
      → List A
    List-takeWhile' f [] = []
    List-takeWhile' f (a :: as) = h0 (f a)
      where
        h0 : Bool → List A
        h0 false = []
        h0 true = a :: List-takeWhile' f as
```

Hmm, `List-takeWhile'` would've worked for us too.

```haskell
module-name : String
--module-name = List-takeWhile (Dec-≡true (λ c → not (primCharEquality c '.')))
--                ⟨𝒮⟩ primShowQName (quote Red)

module-name = List-takeWhile' (λ c → not (primCharEquality c '.'))
                ⟨𝒮⟩ primShowQName (quote Red)

_ = test-identical (primShowQName (quote Red)) "mkn-index.RGB.Red"
_ = test-identical module-name "mkn-index"
```

In this case, we do not use the fact that the boolean property being true for more than just case splitting, but it might come in handy if the property itself is of interest. In `List-takeWhile'`, we will not have that information that we can substitute `p a` for `true`. We will just have an opaque `true`.

--/

2026-10-23 Wk 43 Fri - 22:15 +03:00

https://github.com/oisdk/agda-ring-solver/ authors say it was later added to the standard lib.

2026-10-26 Wk 44 Mon - 14:14 +03:00

```haskell
unquoteDecl IsRed =
  do ty ← quoteTC (RGB → Set)
 declareDef (𝓋𝓇𝒶 IsRed) ty
 defineFun IsRed   [ clause [ 𝓋𝓇𝒶 (var "x") ] (def (quote _≡_) (“ℓ₀” ∷ “RGB” ∷ “Red” ∷ 𝓋𝓇𝓋 0 [] ∷ [])) ]
```

This seems outdated. 

For example `clause` seems wrong.

https://agda.readthedocs.io/en/latest/language/reflection.html#id3 shows a different way to write clause:

```haskell
    [ clause
      ( ("A" , arg′ visible (agda-sort (lit 0)))
      ∷ ("x" , arg′ visible (var 0 []))
      ∷ [])
      ( arg′ hidden (var 1)
      ∷ arg′ visible (var 0)
      ∷ [] )
      (var 0 [])
    ]
```

So the `"x"` part is part of the telescope.

There was a fork of the repo that attempted to change the content, but did not complete the clause either.

Anyway, this works:

```haskell
  -- Defining 
  -- IsRed x = Path RGB x Red
  defIsRed : (id-name : Name) → TC ⊤
  defIsRed id-name = do
    defineFun id-name
      [ clause
        (  ("x" , varg (var 0 []))
        :: [])
        (  varg (var 0)
        :: [])
        (def (quote Path) (h“ℓ-zero“ :: v“RGB“ :: v“Red“ :: (varg (var 0 [])) :: []))
      ]

  IsRed' : RGB → Type
  unquoteDef IsRed' = defIsRed IsRed'

  mkIsRed : (id-name : Name) → TC ⊤
  mkIsRed id-name = do
    ty ← quoteTC (RGB → Type)
    declareDef (varg id-name) ty
    defIsRed id-name
    
  unquoteDecl IsRed = mkIsRed IsRed
```

2026-10-27 Wk 44 Tue - 19:39 +03:00

```haskell
  macro
    _trivially-has-rid_ :
        (let A = ℕ)
      → (_⊕_ : A → A → A)
      → (e : A)
      → Term
      → TC ⊤
    _trivially-has-rid_ _⊕_ e hole = do
      τ ← quoteTC (λ (x : ℕ) → x ⊕ e ≡ x)
      unify
        hole
           (def (quote ℕ-ind)
             (
                  harg (def (quote ℓ-zero) []) -- {ℓ : Level}
               :: harg τ -- {A : ℕ → Type ℓ}
               :: varg (def (quote refl) []) -- Base case
               :: varg (quoteTerm (λ (x : ℕ) (An : x ⊕ e ≡ x)
                                   → (ap suc An)))
             :: []))


  _ = _trivially-has-rid_ _+ℕ_ zero -- bad
```

```
/home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/exp/mkn-index/src/mkn-index.agda:521.7-36: error: [DeBruijnIndexOutOfScope]
de Bruijn index 4 is not in scope in the context (x : _)
when checking that the expression
unquote (_+ℕ_ trivially-has-rid zero) has type _761
```

--/ 2026-10-27 Wk 44 Tue - 20:22 +03:00

If you use `quoteTC` instead of `quoteTerm` here, it allows it.

```haskell
  macro
    _trivially-has-rid_ :
        (let A = ℕ)
      → (_⊕_ : A → A → A)
      → (e : A)
      → Term
      → TC ⊤
    _trivially-has-rid_ _⊕_ e hole = do
      τ ← quoteTC (λ (x : ℕ) → x ⊕ e ≡ x)
      Asn ← quoteTC (λ (x : ℕ) (An : x ⊕ e ≡ x) → (ap suc An))
      An→Asn ← quoteTC (λ (x : ℕ) (An : x ⊕ e ≡ x) → (ap suc An))
      unify
        hole
           (def (quote ℕ-ind)
             (
                  harg (def (quote ℓ-zero) []) -- {ℓ : Level}
               :: harg τ -- {A : ℕ → Type ℓ}
               :: varg (def (quote refl) []) -- Base case
               :: varg (An→Asn)
             :: []))

  _ : ∀ (x : ℕ) → x +ℕ zero ≡ x
  _ = _+ℕ_ trivially-has-rid zero

  _ : ∀ (x : ℕ) → x ∙ℕ 1 ≡ x
  _ = _∙ℕ_ trivially-has-rid 1
```

--/
