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

If I do not specify the uiverse levels of type arguments, are they assumed type 0?

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A : Type ℓ₁} {B : Type ℓ₂} -> A -> B -> B
f2 a b = f1 a b
--       ~~~~~~ error
```

```
error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₂
are not equal
when checking that the expression b has type _B_163
```

Even this fails:

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A B : Type ℓ₁} → A → B → B
f2 a b = f1 a b
--       ~~~~~~ error
```

```
Checking cqts-lect1 (/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda).
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.10-16: error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₁
are not equal
when checking that the expression f1 a b has type B

/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.13-14: error: [UnequalTerms]
The terms
  _A_162 : Type
and
  A : Type ℓ₁
are not equal
when checking that the expression a has type _A_162

/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.15-16: error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₁
are not equal
when checking that the expression b has type _B_163
```

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A B : Type ℓ-zero} → A → B → B
f2 a b = f1 a b
```

This succeeds. So we are to interpret `Type` as strictly `Type ℓ-zero`