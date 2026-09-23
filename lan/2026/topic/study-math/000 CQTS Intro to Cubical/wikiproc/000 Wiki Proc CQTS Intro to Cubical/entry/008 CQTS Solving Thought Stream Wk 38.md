---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/007 CQTS Solving Thought Stream#^spawn-entry-029e13|^spawn-entry-029e13]]

# Journal

## 000

2026-09-13 Wk 37 Sun - 20:08 +03:00

Through [[000 TSW 26 Wk 37#000]],

While trying to solve this problem (again for review, not looking back at my solution from some months ago)

```haskell
+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ b) ≡ℕ (b +ℕ a)
+ℕ-≡ℕ-comm zero b = ≡ℕ-sym (b +ℕ zero) b (+ℕ-≡ℕ-idr b)
+ℕ-≡ℕ-comm (suc a) b = {!≡ℕ-rwr _ _ _ (+ℕ-≡ℕ-comm a b)!}
_ = ≡ℕ-rwr
```

I ended up writing `_ = ≡ℕ-rwr` So that I can `C-]` into that symbol. It would be nice to have something like neovim's autocomplete always ready, and for C-] to work inside of holes, but at least I can do this to jump to a definition for now.

Also since I've learned `_` before it's been so useful at constructing function machinery, focusing on one part at a time, ignoring the others!

2026-09-13 Wk 37 Sun - 20:17 +03:00

We can use `C-.` to check the goal against what we've written in a given hole. It can also be useful to check multiple definitions as we're constructing the definition:

```haskell
+ℕ-≡ℕ-comm : (a b : ℕ) → (a +ℕ b) ≡ℕ (b +ℕ a)
+ℕ-≡ℕ-comm zero b = ≡ℕ-sym (b +ℕ zero) b (+ℕ-≡ℕ-idr b)
+ℕ-≡ℕ-comm (suc a) b = {!+ℕ-≡ℕ-comm a b!}
+ℕ-≡ℕ-comm (suc a) b = {!≡ℕ-rwr _ _ _ (+ℕ-≡ℕ-comm a b)!}
_ = ≡ℕ-rwr
```

Agda-mode will still let us use `C-.` to inspect the duplicate definitions here, it'll just issue an unreachable clause for the last one.

2026-09-14 Wk 38 Mon - 03:19 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-1--Paths.lagda.md`,

Authors explain:

```
If $f, g : X → Y$ are two continuous functions between spaces $X$ and
$Y$ (say, subsets of Euclidean space), then a homotopy $h$ between $f$
and $g$ is a function $h : [0, 1] × X → Y$ of two variables $h(t, x)$
where $h(0, x) = f(x)$ and $h(1, x) = g(x)$ for all $x$. So, for a
fixed $x$, the function $t ↦ h(t, x)$ traces out a path in $Y$ from
$f(x)$ to $g(x)$. By packing these paths together into a single
function $[0, 1] × X → Y$, the idea is that $h(t, x)$ continuously
transforms the function $f$ into the function $g$ as $t$ travels from
$0$ to $1$.
```

The intuition this wants to capture is that we have some shape `X` at `t=0`, and it is continuously deformed, until it is shape `Y` at `t=1`.

But does this capture the conditions sufficiently enough?

For example, can we not construct a function piecewise such that:

```
h(t,x) =  {
  @[t=0]:        f(x)
  @[t>0 && t<1]: 0
  @[t=1]:        g(x)
}
```

This seems as informative as just constructing

```
h2(x) = (f(x), g(x))
```

but I wouldn't think this would show us that the spaces share a homotopy.

Also, later on, we will find that we cannot do case analysis on `i : I`, as this would mean that we can cherry pick what goes on `i0` and `i1`, similar to what I've done here. Instead, we would need to make those derivations by rule. (There are cases where we supply a subcube formula in order to fill the lid with `hcomp`, but even then, we have to be able to derive the corresponding faces.)

And even proving equivalence between two types (or spaces), would require that we find three functions f, g1, g2 such that `f (g1 b) ≡ b` and `g2 (f a) ≡ a`. That is a stronger criteria for equivalence of spaces, because it would mean each space is encodable in the other, and we can prove by encoding then decoding the original message. 

https://encyclopediaofmath.org/wiki/Homotopy also specifies that homotopy is an equivalence relation. 

https://aeb.win.tue.nl/at/algtop-3.html Has a similar definitions, although it instead specifies that `X,Y` are topological spaces.

Also note that my piecewise `h(t,x)` can fail to be continuous. h should be a continuous function, so this is not enough. For some codomains, we may be able to derive `h(t, x) = (1-t) * f(x) + t * g(x)`, which would a continuous function and may serve as a valid homotopy. It goes send any point `f(x)` to a corresponding point `g(x)`, so it also fits on first approximation the intuition of a continuous deformation that preserves all points on both ends.

2026-09-14 Wk 38 Mon - 04:34 +03:00

```haskell
open import Agda.Primitive using (
  LevelUniv; 
  Level) renaming (
  lzero to ℓ-zero;
  lsuc to ℓ-suc;
  _⊔_ to ℓ-max)

open import Agda.Primitive.Cubical using (
  I; 
  i0; 
  i1; 
  Partial) renaming (
    primIMin to infixr 20 _∧_;
    primIMax to infixr 20 _∨_;
    primINeg to infix 30 ~_)

data ⊤ : Set where
  tt : ⊤

_ = Set (ℓ-suc (ℓ-suc ℓ-zero))
_ = I → ⊤
```

This checks fine with agda and mikan (minus the deprecation of `Set` in mikan).

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-1--Paths.lagda.md`,

Authors explain:

```
However, since we want to discuss paths in any type, there is a
special rule that for any actual type `A : Type ℓ`, functions `I → A`
is also an actual type in `Type ℓ`.

_ : Type ℓ-zero
_ = I → Bool
```

But even though `⊤` is of `Set ℓ-zero`, that checks. So that check doesn't seem to do much?

2026-09-14 Wk 38 Mon - 17:51 +03:00

Oh my bad I wrote

```haskell
_ = Set (ℓ-suc (ℓ-suc ℓ-zero))
_ = I → ⊤
```

Since both have =, I did no type check!

This fails as we expect:

```haskell
open import Agda.Primitive using (
  LevelUniv; 
  Level) renaming (
  lzero to ℓ-zero;
  lsuc to ℓ-suc;
  _⊔_ to ℓ-max)

open import Agda.Primitive.Cubical using (
  I; 
  i0; 
  i1; 
  Partial) renaming (
    primIMin to infixr 20 _∧_;
    primIMax to infixr 20 _∨_;
    primINeg to infix 30 ~_)

data ⊤ : Set where
  tt : ⊤

_ : Set (ℓ-suc (ℓ-suc ℓ-zero))
_ = I → ⊤
```

```
error: [UnequalTypes]
The types
  Type
and
  Type₂
are not equal
when checking that the expression I → ⊤ has type Type₂
```

2026-09-15 Wk 38 Tue - 07:11 +03:00

```haskell
    apⁿ-∘ :
      (f : A → B)
      → (g : B → C)
      → (p : x ≡ y)
      → (apⁿ (g ∘ f) p) ≡ apⁿ g (apⁿ f p)
    apⁿ-∘ f g p i i₁ = g (f (p i₁))
```

Has issues writing this definition, turns out it is because I put `f` before`g` in the definition of `_∘_`! It should be like this, with `g` first:

```haskell
_∘_ :
  (g : {a : A} → (b : B a) → C a b)
  → (f : (a : A) → B a)
  → ((a : A) → C a (f a))
_∘_ g f a = g (f a)
```

I confirmed the issue when I flipped `(g ∘ f)` and saw that it accepted that.

2026-09-16 Wk 38 Wed - 06:18 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-1-paths.agda`,

```haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {B : Type ℓ₂}
  where
    ∘-idl :
      (f : A → B)
      → idfun ∘ f ≡ f
    -- ∘-idl f = refl
    ∘-idl f = funextˢ h0
      where
        h0 : (a : A) → (idfun ∘ f) a ≡ f a
        h0 a = refl
```

If I don't explicitly mention these types `A B` like this in an anonymous module but instead have them be inferred from the private variables like so:

```haskell
private
  variable
    ℓ₁ : Level
    A B C D : Type ℓ₁
    x y a a' : A
    b b' : B

∘-idl :
  (f : A → B)
  → idfun ∘ f ≡ f
-- ∘-idl f = refl
∘-idl f = funextˢ h0
  where
    h0 : (a : A) → (idfun ∘ f) a ≡ f a
    h0 a = refl
```

I get the error

```
error: [UnequalTypes]
The type
  A
is not a subtype of
  A
because:
  one has de Bruijn index 1, the other 5
when checking that the expression a has type A
```

--/ 2026-09-16 Wk 38 Wed - 07:51 +03:00
::Aside
They quote de Bruijin for the name `telescope` here: https://agda.readthedocs.io/en/latest/language/telescopes.html
::
--/

2026-09-16 Wk 38 Wed - 06:56 +03:00

```haskell
data ℤˢ : Type where
  posˢ : ℕ → ℤˢ
  negˢ : ℕ → ℤˢ
  zeroˢ≡ : posˢ zero ≡ negˢ zero

sucℤˢ : ℤˢ → ℤˢ
sucℤˢ (posˢ a) = posˢ (suc a)
sucℤˢ (negˢ zero) = posˢ (suc zero)
sucℤˢ (negˢ (suc a)) = negˢ a
sucℤˢ (zeroˢ≡ i) = posˢ (suc zero)
```

--/ 2026-09-16 Wk 38 Wed - 06:57 +03:00

Why is it that `zeroˢ≡` here unpacks to `(zeroˢ≡ i)`? 

I think this might have to do with how constructors can be interpreted as denoting new objects rather than as maps. (For example, `suc a` is an object, almost like a named box that contains an `a` in it).

This is different from the `s` in

```haskell
flip-square :
	Square A a₀- a₁- a-₀ a-₁
  → Square A a-₀ a-₁ a₀- a₁- 
flip-square s = {!!}
```

Here `s` is just a map `I → I → Type`. It doesn't *give* us an `i j`, it's more like a mold with `i j` being like holes/negative prints, or like how a key fits in a keyhole. The map is the keyhole-to-mechanism but not the key! Maybe many keys that fit the defined characteristics can be used.

So it seems we shouldn't interpret `zeroˢ≡ : posˢ zero ≡ negˢ zero` as saying we have a *path* `zeroˢ≡` because paths are maps and maps are negative (they are supplied input), while this is positive (supplying us with an `i`)

PEND
--/ 

2026-09-16 Wk 38 Wed - 19:28 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-2--Equivalences-and-Path-Algebra.lagda.md`,

```
Again, thinking of ``Bool`` as being smaller than ``ℕ``, a function is
a retract when it is describing a way to shrink, or "retract", the
larger type into the smaller type.
```

I tried to explain this by writing

```haskell
-- Given two maps (f : A → B) (g : B → A) and assuming that (size A) ≤ (size B),
-- f being a retract of g means that g shrinks the type B, or retracts it, to fit in A.
-- f is called its retract as it undoes this operation, given us back our data in B.

-- We're describing the same thing by saying g has a section f, since f is curving out
-- a section in B to fit A. f being the retract of g is the perspective of g shrinking B
-- to fit A, rather than the perspective of f curving out a section of B to fit A.

-- f being a retract of g means that g has a section f means that g (f a) ≡ a. 
```

But then *the* retract is the one doing the shrinking. and it is *f* being the retract, so I gotta flip that in the explanation.

I wanted to show that both perspectives are symmetrical, whether we think of `B` retracting to `A` so that we get a representation or whether `A` curves out a section in `B` to get a representation. Second attempt:

```haskell
-- Given two maps (f : A → B) (g : B → A) and assuming that (size A) ≤ (size B),

-- f being a retract of g means that g has a section f means that g (f a) ≡ a. 

-- Here f retracts B (or shrinks it) to the size of A, so that (f a) can represent a via g
-- with g (f a) ≡ a.

-- We can also describe this as f (being the section), then we would say it (f a) curves
-- a section in B that represents A via g: g (f a) ≡ a.
```

I also am renaming the `isSection`, `isRetract`, and consequent records from the CQTS lecture notes. It seems more clear to me to use `_hasSection_` and `_isRetractOf_`. Last time I worked with `isSection`, I had to keep noting that `isSection f g` means `g is a section of f`. I don't have to when I write `f hasSection g`. And I know f is the retract when I write `f isRetractOf g`. 

--/ 2026-09-16 Wk 38 Wed - 19:38 +03:00

I am also renaming the constructors. Instead of something like `sectionData`, I'm having it be uniform with the name of the record. For a record `Foo`, we have `newFoo` as its constructor.

The records I chose for are also themselves close to the proofs `hasSection` and `isRetractOf`, just capitalized: `HasSection` and `IsRetractOf`, to try to signify they are just packaging for those proofs and the maps.
--/

--/ 2026-09-17 Wk 38 Thu - 01:03 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-2--Equivalences-and-Path-Algebra.lagda.md`,

```
For this reversed situation, we say that `f : A → B` is a *retract*
when it *has* a section.

When the function `f : A → B` has a section `g : B → A` *and* has a
retract `g' : B → A`, as is the case for ``RedOrBlue→Bool``, we say
that `f` is an *equivalence*.

`g'` faithfully represents elements of `B` as elements of `A` (which we
know because it has a section `f`, i.e. is a retract of `f`). 
```

So `g'` is the retract *because* it has a section `f`.  It's not `f` that's the retract, they called the retract `f` up there, and I'm not sure if this is a mistake or they just wanted a generic map to refer to when they wrote that.

I'm renaming `isRetractOf` for `hasRetract`. What we want is something like `g' <-hasRetract- f -hasSection-> g`.

Here's my `attempt 3`.

```haskell
-- Given three maps (f : A → B) (g : B → A) (g' : B → A),

-- `g' <-hasRetract- f -hasSection-> g`

-- `f hasSection g` tells us that for all elements b : B, f represents them as (g b) : A. In other words,
-- if g is treated like a tag representation in A, f (g b) removes the tag, recovering the b.

-- `f hasRetract g'` tells us for all elements a : A, g' represents them
-- as (f a) : B, and can 'untag' them: g' (f a) ≡ a.

-- g being a section of f can be interpreted as g picking a (possibly smaller
-- than 100%) section of A to fit B in. So `f hasSection g` also implies
-- (size B) ≤ (size A), since it must fit all of B in A for g to be total.

-- g' being a retract of f (i.e. g' has a section f) can be interpreted as
-- g' retracting(/shrinking) B to the size of A. It is a symmetrical to the
-- interpretation that f (as the section of g')  picks a section in B
-- to fill A in. This implies the size constraint (size A) ≤ (size B).

-- So `g' <-hasRetract- f -hasSection-> g` gives us (size B) ≤ (size A) and
-- (Size B) ≥ (Size A).
-- In this case, we say f is an equivalence. It establishes that A and B are
-- equivalent and can be represented in one another.
```

--/

2026-09-20 Wk 38 Sun - 02:03 +03:00

I need a lot of `a`s in the problems I'm solving. Here's some convention to keep it to one character: `a â a̭ : A`. We have a, a-top, and a-bot all of which are of type A.

We can call a-top also a-hat, I've seen it called before. There's also `a'` but then it takes two characters and can cause some misalignments sometimes, though not a big deal if we use it. I usually call this a-prime.

I'm choosing a-top and a-bot because from afar, even if the exact mark is unclear, the orientation (top/bottom) at least is.

The symmetry of a-bot and a-top could also help to align it with a at i0 and a at i1.

```haskell
    J :
        (Q : (a : A) → a̭ ≡ a → Type ℓ)
      → (r : Q a̭ refl)
      → (p : a̭ ≡ â)
      → Q â p
    J Q r p = transport (J-line Q p) r
```

Like in here, `(p : a̭ ≡ â)`.

Though maybe let's use `a̬` instead so `a-bottom` actually has an inverted hat. I guess we can call it `a-v` too, and `a-hat` for top.

`a̬a̭âǎ` So I guess we even got 4 distinctions here. `a-v`, `a-fro-hat`, `a-hat`, `a-to-v`? 

2026-09-20 Wk 38 Sun - 07:54 +03:00

A telescope in agda is the list of parameters, where each following the last can be dependant on it. I think in the docs they said it's called a telescope because it's like the instrument, where each layer depends on the last being inserted.

