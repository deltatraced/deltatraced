---
context_type: task
status: todo
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv#^spawn-task-89aed8|^spawn-task-89aed8]]

# What?

We want to setup a `problems-mkn` project that can be directly ran with `mikan` or `agda`. We want to reproduce our cqts progress with only the minimal necessary dependencies needed for each lecture.

`problems-mkn` is setup to have many projects that interconnect through `$(git root)`, so that we can categorize our studying efforts per resource while being able to leverage prior work.

First we setup the project structure, and then we carry on to reimplement the solutions and review what we've solved so far with CQTS.

# Journal

2026-08-24 Wk 35 Mon - 00:19 +03:00

Spawn [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/task/001 Setup two mikan projects and have one depend on the other through problems-mkn git root]] ^spawn-task-ae85d1

2026-08-24 Wk 35 Mon - 03:24 +03:00

Now that we can have modules and dependencies, let's start bringing in things we've solved from CQTS.

2026-08-24 Wk 35 Mon - 03:44 +03:00

```haskell
-- in /home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-1--Types-and-Functions.lagda.md
double : ℕ → ℕ
double x = 2 · x
```

Okay, we need to define ℕ to be able to write down the definition of `double`  in `cqts-lect1.agda`. Its definition is in `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/Library/Prelude.lagda.md`

2026-08-24 Wk 35 Mon - 17:56 +03:00

I wonder why this inconsistency with agda mode input:

```
aₐ a_b a_c a_d aₑ a_f a_g aₕ aᵢ aⱼ aₖ aₗ aₘ aₙ aₒ aₚ a_q aᵣ aₛ aₜ aᵤ a̬ a_w aₓ a_y a_z
```

- https://wiki.portal.chalmers.se/agda/Main/Community
- $\to$ https://agda.zulipchat.com/#recent

[#general > agda-mode inconsistent subscript letters @ 💬](https://agda.zulipchat.com/#narrow/channel/238741-general/topic/agda-mode.20inconsistent.20subscript.20letters/near/618390085)

superscript has inconsistencies too:

```
aᵃ aᵇ aᶜ aᵈ aᵉ aᶠ aᵍ aʰ aⁱ aʲ aᵏ a⃖ aᵐ aⁿ aᵒ aᵖ a𐞥 a⃗ aˢ aᵗ aᵘ ǎ aʷ aˣ aʸ aᶻ
aᴬ aᴮ aĈ aᴰ aᴱ aꟳ aᴳ aᴴ aᴵ aᴶ aᴷ aᴸ aᴹ aᴺ aᴼ aᴾ aꟴ aᴿ aŜ aᵀ aᵁ aⱽ aᵂ âX aŶ aẐ
```

2026-08-25 Wk 35 Tue - 09:05 +03:00

Spawn [[003 mkn If I do not specify the universe levels of type arguments - are they assumed type 0?]] ^spawn-invst-756aad

2026-08-27 Wk 35 Thu - 07:38 +03:00

```haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda
Σ-map¹ :
  {ℓ₁ ℓ₂ ℓ₃ ℓ₄ : Level}
  → {A : Type ℓ₁}
  → {A' : Type ℓ₂}
  → {B : A → Type ℓ₃}
  → {B' : A' → Type ℓ₄}
  → (f : A → A')
  → (g : {a : A} → B a → B' (f a))
  → (Σ[ a ∈ A ] B a → Σ[ a' ∈ A' ] B' a')

Σ-map¹ f g (a , b) = f a , g b
```

This type should be more strict than this. We should be able to map to an `Σ[ a' ∈ A' ] B' a'` where `a' ≡ f a` holds, but this should do for this review and we're just in the first lecture.

2026-08-27 Wk 35 Thu - 08:07 +03:00

In `cqts-lect1.agda` I have been giving outputs of different universe levels than the inputs. Maybe it could also do as the `ℓ-max` of the input levels though.

2026-08-28 Wk 35 Fri - 05:40 +03:00

```haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda
data List (A : Type) : Type where
  [] : List A
  _::_ : A → List A → List A
```

I think it's good to think of the constructors to an inductive type as being their own declared objects, rather than as functions.

So here we have an object `[] : List A`. We also have objects whose definition depend on an `A` and another `List A`. Say for example `5 :: []` for the case where `A` is a natural number.

This is why we don't have to supply a definition for this as if it were a function declaration. The last enter in the constructor is the type in question and signifies that it is an object of that type, but it may depend on some other data, including of the type itself.

