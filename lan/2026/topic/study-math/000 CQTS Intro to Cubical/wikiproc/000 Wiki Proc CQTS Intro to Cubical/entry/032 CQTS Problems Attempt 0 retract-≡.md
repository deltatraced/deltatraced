---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-ed4686](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-ed4686)

# Journal

2026-09-29 Wk 40 Tue - 12:54 +03:00

Previous attempt was at [001 Proc CQTS retract equiv](001%20Proc%20CQTS%20retract%20equiv.md). Now continuing here.

[000 CQTS Intro to Cubical Templates](../../../wiki/000%20Wiki%20CQTS%20Intro%20to%20Cubical/entry/000%20CQTS%20Intro%20to%20Cubical%20Templates.md)

This is the cube we can figure out is plausible from the hint at the start.

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

--                     ↓ —
--                â — — — — — — — — > â
--              / ^ G               / ^
--        q → /   |    A          /   |
--          /     |             / ← p |
--        a̬ — — — — — — — — > a̬       |
--        ^       | — ↑       ^       |                    ^   j
--        |   ? → |           |       | ← —              k | /
--        |       |           |       |                    ∙ — >
--    ? → |       |    ↓ ?    |       |                      i
--        |       r̂ — — — — — | — — > â
--        | ? → /         — → |     /
--        |   /               |   / ← ?
--        | /                 | /
--        r̬ — — — — — — — — > a̬
--                ↑ ?
-- r̬ = ?, r̂ = ?
-- q = ?
````

* `(Line îk̬)` probably should be p to easily solve `(i = i1)` face.

`r̬ = (r .section .map a̬)` can't be right because that would be of type `B`, and this is a cube of type `A`.

When they wrote `r (s x)` then I infer it means `r .section .map` for `s`, and `r .map` for `r`, which brings us back to `A`.

2026-09-29 Wk 40 Tue - 13:58 +03:00

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

--                     ↓ —
--                â — — — — — — — — > â
--              / ^                 / ^
--        q → /   |    A          /   |
--          /     |             / ← p |
--        a̬ — — — — — — — — > a̬       |
--        ^       | — ↑       ^       |                    ^   j
--        |   ŝ → |           |       | ← —              k | /
--        |       |           |       |                    ∙ — >
--    s̬ → |       |    ↓ ŝ    |       |                      i
--        |       r̂ — — — — — | — — > â
--        | t → /         — → |     /
--        |   /               |   / ← ?
--        | /                 | /
--        r̬ — — — — — — — — > a̬
--                ↑ s̬
-- values:
-- r̬  = r .map (r .section .map a̬ )
--  r̂ = r .map (r .section .map  â)
-- lines:
-- s̬₁  = r .section .proof a̬
--  ŝ₁ = r .section .proof  â
-- t = ap (λ a → r .map (r .section .map a)) p
-- q = (sym s̬) ∙∙ t ∙∙ ŝ
````

Now we inferred the rest of the values.

`(Line îk̬)` being `p` would make the `(k = i0)` face hard to solve. Setting it to `q` makes the `(k = i0)` face easy to solve, but for the price that the face `(i = i1)` being hard to solve, and the shared line with the lid `Line (îk̂)` now would just be `p`, so we won't be able to not specify it for hcomp, with the expectation of it being the double comp of the face `(i = i1)`.

2026-09-29 Wk 40 Tue - 14:22 +03:00

Let's refactor

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

retract-≡ r .map pB =
  a̬                            ≡⟨ sym (r .section .proof a̬) ⟩
  r .map (r .section .map a̬ )  ≡⟨ ap (λ b → r .map b) pB  ⟩
  r .map (r .section .map  â)  ≡⟨ r .section .proof â ⟩
  â                            ∎
````

to

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda
retract-≡ r .map pB = (sym s̮) ∙∙ t ∙∙ ŝ
  where
    s̮ : r .map (r .section .map a̮) ≡ a̮
    s̮ = r .section .proof a̮
    ŝ : r .map (r .section .map â) ≡ â
    ŝ = r .section .proof â
    t : r .map (r .section .map a̮) ≡ r .map (r .section .map â)
    t = ap (λ b → r .map b) pB
````

There was a weird subtle difference in how I wrote the bot `a`s:

````
a̮ a̬
````

2026-09-29 Wk 40 Tue - 14:49 +03:00

Now this is what is remaining to be solved, which is now put in the same form as the square we need to solve for:

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda
retract-≡ r .section .proof p = h0
  where
    s̮ : r .map (r .section .map a̮) ≡ a̮
    s̮ = r .section .proof a̮
    ŝ : r .map (r .section .map â) ≡ â
    ŝ = r .section .proof â
    t : r .map (r .section .map a̮) ≡ r .map (r .section .map â)
    t = ap (λ a → r .map (r .section .map a)) p
    h0 : ((sym s̮) ∙∙ t ∙∙ ŝ) ≡ p
    h0 = {!!}
````

2026-09-29 Wk 40 Tue - 15:02 +03:00

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

retract-≡-squareTube :
    (r : B RetractsOnto A)
    (p : a̮ ≡ â)
  → (i j k : I)
  → Partial (∂ i ∨ ∂ j ∨ ~ k) A
retract-≡-squareTube r p i j k (i = i0) = {!!}
  where
    s̮ : r .map (r .section .map a̮) ≡ a̮
    s̮ = r .section .proof a̮
    ŝ : r .map (r .section .map â) ≡ â
    ŝ = r .section .proof â
    t : r .map (r .section .map a̮) ≡ r .map (r .section .map â)
    t = ap (λ a → r .map (r .section .map a)) p
    q : a̮ ≡ â
    q = (sym s̮) ∙∙ t ∙∙ ŝ
    Sq : Square A s̮ ŝ t q
    Sq j k = {!!}
retract-≡-squareTube r p i j k (i = i1) = {!!}
  where
    s̮ : r .map (r .section .map a̮) ≡ a̮
    s̮ = r .section .proof a̮
    ŝ : r .map (r .section .map â) ≡ â
    ŝ = r .section .proof â
    t : r .map (r .section .map a̮) ≡ r .map (r .section .map â)
    t = ap (λ a → r .map (r .section .map a)) p
    q : a̮ ≡ â
    q = (sym s̮) ∙∙ t ∙∙ ŝ
    Sq : Square A refl refl p p
    Sq j k = {!!}
-- [...]
````

This is not ideal, having to repeat all these definitions for each case split. I would have liked for them to be local though. I could make them global, but I don't want these very short names in the global namespace. Though I guess maybe declaring them private could help with that. But for now I'll do the duplication and focus on solving this.

Of course I could also only include the ones that are used in each one, but I'm including all in general now just to have flexibility as I may vary the square faces while solving for this. I do want all these symbols accessible.

--/ 2026-09-29 Wk 40 Tue - 15:51 +03:00

It doesn't seem that it's easy to put `p` into the `module _` automatically provided variables, because then it differs from the `p` being unpacked in the section proof. So this likely means we also can't put `t` which depends on it under `private` in the module, and so also not `q`.

We can turn `t` and `q` into maps out of `p` though.

That worked. So `private` inside a `module _` is useful for shared local definitions that will not be accessible outside that module, so we can give them very short names fitting of the context.

--/

2026-09-29 Wk 40 Tue - 15:16 +03:00

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

--                     ↓ —
--                â — — — — — — — — > â
--              / ^                 / ^
--        q → /   |    A          /   |
--          /     |             / ← p |
--        a̬ — — — — — — — — > a̬       |
--        ^       | — ↑       ^       |                    ^   j
--        |   ŝ → |           |       | ← —              k | /
--        |       |           |       |                    ∙ — >
--    s̬ → |       |    ↓ ŝ    |       |                      i
--        |       r̂ — — — — — | — — > â
--        | t → /         — → |     /
--        |   /               |   / ← p
--        | /                 | /
--        r̬ — — — — — — — — > a̬
--                ↑ s̬
-- values:
-- r̬  = r .map (r .section .map a̬ )
--  r̂ = r .map (r .section .map  â)
-- lines:
-- s̬₁  = r .section .proof a̬
--  ŝ₁ = r .section .proof  â
-- t = ap (λ a → r .map (r .section .map a)) p
-- q = (sym s̬) ∙∙ t ∙∙ ŝ
````

Ok so this choice requires us to solve the hard ware `Square A t p s̬ ŝ`.

This should be how far we got with this from our last attempt, before going to try many many cube configurations and not getting one we know how to solve for.

Before we try to search for a solution for this `(k = i0)` square, let's first try to set `(Line îk̬)` to `q` instead of `p`.  Here's first my hypothesis:

I assume the *shared line* between a missing face and the lid at `(k = i1)` would be set to a double composite. This would require that their shared line be a double composite for that technique to work. If my hypothesis is correct, then this attempt must fail, and we should see it try to create a double composite at `Line (îk̂)`, the shared line once we set `(Line îk̬)` to q.

My hypothesis can be wrong if somehow this solves it. But it can also be wrong if I'm incorrect about where the double composite (if any) will show up.

Let's save the current partial solution for tube for the above square first.

````sh
cat /dev/random | head -n1 | sha1sum | head -c8

# out
5a667d86
````

It's in https://codeberg.org/lan22h-experiments/note-files/src/branch/main/paste/5a667d86

2026-09-29 Wk 40 Tue - 15:35 +03:00

Now let's try to close the lid on this partial cube:

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-7-propositions.agda

--                     ↓ —
--                â — — — — — — — — > â
--              / ^                 / ^
--        q → /   |    A          /   |
--          /     |             / ← p |
--        a̬ — — — — — — — — > a̬       |
--        ^       | — ↑       ^       |                    ^   j
--        |   ŝ → |           |       | ← —              k | /
--        |       |           |       |                    ∙ — >
--    s̬ → |       |    ↓ ŝ    |       |                      i
--        |       r̂ — — — — — | — — > â
--        | t → /         — → |     /
--        |   /               |   / ← q
--        | /                 | /
--        r̬ — — — — — — — — > a̬
--                ↑ s̬
-- values:
-- r̬  = r .map (r .section .map a̬ )
--  r̂ = r .map (r .section .map  â)
-- lines:
-- s̬₁  = r .section .proof a̬
--  ŝ₁ = r .section .proof  â
-- t = ap (λ a → r .map (r .section .map a)) p
-- q = (sym s̬) ∙∙ t ∙∙ ŝ
````

Now `(k = i0)` is just another instance of `∙∙-filler`.

2026-09-29 Wk 40 Tue - 16:09 +03:00

````haskell
    retract-≡-square :
        (p : a̮ ≡ â)
      → Square A (q p) p refl refl
    retract-≡-square p i j = {!hcomp (~ i ∨ ∂ j) (retract-≡-squareTube p i j)!}
````

````haskell
Goal: A
———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ a̮
j = i1 ⊢ â
i = i0 ⊢ q p j
i = i1 ⊢ p j
Have: A
———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ hcomp i1 (λ k _ → lect-2-7-propositions.Sq p i k i k)
j = i1 ⊢ hcomp i1 (λ k _ → lect-2-7-propositions.Sq p i k i k)
i = i0 ⊢ hcomp i1 (λ k _ → lect-2-7-propositions.Sq p j k j k)
i = i1 ⊢ hcomp (∂ j) (retract-≡-squareTube p i1 j)
````

Yup, the `(i = i1)` face test of the lid face *is* the shared line `Line (îk̂)` we suspected. I would expect if we change the orientation of the lid and shared line that it would track a line on the lid square that would also be the one corresponding to the matching line on the partial cube.

But as you see, we expect `p j` in there, not a double composite of anything, so this attempt will not work.

Note that there's nothing special with it only filling one place here. It is precisely the missing lines from the lid square that get filled up.:

````haskell
    retract-≡-squareTube :
        (p : a̮ ≡ â)
        (i j k : I)
      → Partial (∂ j ∨ ~ k) A
	  
    retract-≡-square :
        (p : a̮ ≡ â)
      → Square A (q p) p refl refl
    retract-≡-square p i j = {!hcomp (∂ j) (retract-≡-squareTube p i j)!}
````

````haskell
Goal: A
———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ a̮
j = i1 ⊢ â
i = i0 ⊢ q p j
i = i1 ⊢ p j
Have: A
———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ hcomp (∂ i0) (λ k _ → lect-2-7-propositions.Sq p i k i k)
j = i1 ⊢ hcomp (∂ i1) (λ k _ → lect-2-7-propositions.Sq p i k i k)
i = i0 ⊢ hcomp (∂ j) (retract-≡-squareTube p i0 j)
i = i1 ⊢ hcomp (∂ j) (retract-≡-squareTube p i1 j)
````

Since I omitted both `i` faces, both were taken as hcomps in faces `(i = i0)` and `(i = i1)`. We can still think of this as shared lines with the lid for subcube faces we omitted.

2026-09-29 Wk 40 Tue - 16:21 +03:00

So anyway, back to the solution in https://codeberg.org/lan22h-experiments/note-files/src/branch/main/paste/5a667d86 although we'll just rewrite it. Because we made changed with the module private variables:

````haskell
module _
  {ℓ₁ ℓ₂ : Level}
  {A : Type ℓ₁}
  {B : Type ℓ₂}
  {a̮ â : A}
  {r : B RetractsOnto A}
  where
    private
      s̮ : (r .map (r .section .map a̮)) ≡ a̮
      s̮ = r .section .proof a̮
      ŝ : r .map (r .section .map â) ≡ â
      ŝ = r .section .proof â
      t : (p : a̮ ≡ â) → r .map (r .section .map a̮) ≡ r .map (r .section .map â)
      t p = ap (λ a → r .map (r .section .map a)) p
      q : (p : a̮ ≡ â) → a̮ ≡ â
      q p = (sym s̮) ∙∙ (t p) ∙∙ ŝ
````

2026-09-29 Wk 40 Tue - 17:14 +03:00

This almost works:

````haskell
      --               ŝ  
      --         r̂  — — — >  â        
      --         ^           ^              ^
      --       t |     A     | p          j |
      --         |           |              . — >
      --         r̮  — — — >  a̮                i
      --               s̮  
      -- can be decomposed to
      --               ŝ  
      --         r̂  — — — >  â        
      --         ^           ^               
      -- (sym ŝ) |     A     | —             
      --         |     —     |                   
      --         â  — — — >  â                 
      --         ^           ^               
      --       p |     A     | p             
      --         |     —     |                   
      --         a̮  — — — >  a̮                 
      --         ^           ^              ^
      --       s̮ |     A     | —          j |
      --         |           |              . — >
      --         r̮  — — — >  a̮                i
      --               s̮  
````

But because of how we defined `t`:

````haskell
t : (p : a̮ ≡ â) → r .map (r .section .map a̮) ≡ r .map (r .section .map â)
t p = ap (λ a → r .map (r .section .map a)) p
````

it does not resolve. So maybe we can redefine `t` like this instead and retry:

````haskell
t : (p : a̮ ≡ â) → r .map (r .section .map a̮) ≡ r .map (r .section .map â)
t p = s̮ ∙∙ p ∙∙ (sym ŝ)
````

````haskell
      sq-k0 :
          (p : a̮ ≡ â)
        → Square A (t p) ((refl ∙ p) ∙ refl) s̮ ŝ
      sq-k0 p = ∙-juxtapose-vertical (∙-juxtapose-vertical sq̮ sq) sq̂
        where
          sq̂ : Square A (sym ŝ) refl refl ŝ
          sq̂ i j = ŝ (i ∨ ~ j)
          sq : Square A p p refl refl
          sq i j = p j
          sq̮ : Square A s̮ refl s̮ refl
          sq̮ i j = s̮ (i ∨ j)
````

It works if we have

* `((refl ∙ p) ∙ refl)`

````haskell
t : (p : a̮ ≡ â) → r .map (r .section .map a̮) ≡ r .map (r .section .map â)
t p = (s̮ ∙ p) ∙ (sym ŝ)
````

So we need to simplify this square.

Further, this re-definition of `t` is problematic for defining `q`. Now this goal no longer resolves:

````haskell
retract-≡ .section .proof p = {!h0!}
  where
    h0 : q p ≡ p
    h0 i j = retract-≡-square p i j
````

Since with `t` defined itself via double composition, our definition of `q` could simplify back to `p`.

````haskell
t : (p : a̮ ≡ â) → r .map (r .section .map a̮) ≡ r .map (r .section .map â)
t p = ap (λ a → r .map (r .section .map a)) p
````

Might also be due to how I defined this:

````haskell
retract-≡ .map pB = (sym s̮) ∙∙ tB ∙∙ ŝ
  where
    tB : r .map (r .section .map a̮) ≡ r .map (r .section .map â)
    tB = ap (λ b → r .map b) pB
````

2026-09-29 Wk 40 Tue - 18:26 +03:00

We can use `∙-juxtapose-vertical` and `∙-juxtapose-horizontal` to substitute paths in a square too to simplify what we got, but this might increase in complexity because it goes through `_∙_`, forming more complexity, but we can try. It may simplify and just replace if the composition is just `refl ∙ refl`.

Though we also are only getting one refl in our case, like with `ŝ ∙ refl`.

````haskell
--               ŝ           —                     
--         r̂  — — — >  â  — — — >  â               
--         ^           ^           ^                     ^
--      t₁ |     A  p₁ |     A     | p₂                j |
--         |           |           |                     . — >
--         r̮  — — — >  a̮  — — — >  a̮                       i
--               s̮           —                        
-- p₁ = (refl ∙ p) ∙ refl
-- p₂ = (refl ∙ p)
-- t₁ = (s̮ ∙ p) ∙ (sym ŝ)
````

It's not reducing `ŝ ∙ refl` and `s̮ ∙ refl`.

````haskell
--               ŝ           —                     
--         r̂  — — — >  â  — — — >  â               
--         ^           ^           ^                     ^
--      t₁ |     A  p₁ |     A     | p₂                j |
--         |           |           |                     . — >
--         r̮  — — — >  a̮  — — — >  a̮                       i
--               s̮           —                        
-- p₁ = (refl ∙ p) ∙ refl
-- p₂ = (refl ∙ p)
-- t₁ = (s̮ ∙ p) ∙ (sym ŝ)
sq1 : Square A ((s̮ ∙ p) ∙ (sym ŝ)) (refl ∙ p) s̮ ŝ
sq1 i j = {!∙-juxtapose-horizontal sq0 sq̂ i j!}
  where
    sq̂ : Square A ((refl ∙ p) ∙ refl) (refl ∙ p) refl refl
    sq̂ i j = ∙-idr (refl ∙ p) (~ i) j
````

````haskell
Goal: A
———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ r .section .proof a̮ i
j = i1 ⊢ r .section .proof â i
i = i0 ⊢ hcomp (j ∨ ~ j)
         (double-comp-box (λ i₂ → r .map (r .section .map a̮))
          (λ i₂ →
             hcomp (i₂ ∨ ~ i₂)
             (double-comp-box (λ i₃ → r .map (r .section .map a̮))
              (r .section .proof a̮) p i₂))
          (λ i₂ → r .section .proof â (~ i₂)) j)
i = i1 ⊢ hcomp (j ∨ ~ j)
         (double-comp-box (λ i₂ → a̮) (λ i₂ → a̮) p j)
Have: A
———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ hcomp (i ∨ ~ i)
         (double-comp-box (λ i₂ → r .map (r .section .map a̮))
          (λ i₂ → r .section .proof a̮ i₂) (λ i₂ → a̮) i)
j = i1 ⊢ hcomp (i ∨ ~ i)
         (double-comp-box (λ i₂ → r .map (r .section .map â))
          (λ i₂ → r .section .proof â i₂) (λ i₂ → â) i)
i = i0 ⊢ hcomp (j ∨ ~ j)
         (double-comp-box (λ i₂ → r .map (r .section .map a̮))
          (λ i₂ →
             hcomp (i₂ ∨ ~ i₂)
             (double-comp-box (λ i₃ → r .map (r .section .map a̮))
              (r .section .proof a̮) p i₂))
          (λ i₂ → r .section .proof â (~ i₂)) j)
i = i1 ⊢ hcomp (j ∨ ~ j)
         (double-comp-box (λ i₂ → a̮) (λ i₂ → a̮) p j)
````

Of course if the square was `p p refl refl` or like that, then we could just reduce it to a path and use `subst`.

2026-09-29 Wk 40 Tue - 19:05 +03:00

Actually we can just use subst, since it works on any part of a type.

````haskell
sq1 : Square A ((s̮ ∙ p) ∙ (sym ŝ)) (refl ∙ p) s̮ ŝ
sq1 i j = subst (λ a' → Square A ((s̮ ∙ p) ∙ sym ŝ) a' s̮ ŝ)
           (sym (∙-idr (refl ∙ p))) sq0 i j
sq2 : Square A ((s̮ ∙ p) ∙ (sym ŝ)) p s̮ ŝ
sq2 i j = subst (λ a' → Square A ((s̮ ∙ p) ∙ sym ŝ) a' s̮ ŝ)
           (sym (∙-idl p)) sq1 i j
````

Like this!

2026-09-29 Wk 40 Tue - 20:25 +03:00

I proves these additional path properties:

````haskell
∙∙-refl-ends-same :
    (p : a ≡ b)
  → (q : b ≡ c)
  → (refl ∙∙ p ∙∙ q) ≡ (p ∙∙ q ∙∙ refl)
  
∙-two-nested-to-∙∙ :
    (p : a ≡ b)
  → (q : b ≡ c)
  → (r : c ≡ d)
  → (p ∙ q) ∙ r ≡ p ∙∙ q ∙∙ r
````

We needed `∙∙-refl-ends-same` to prove `∙-two-nested-to-∙∙`.

Now we can simplify the composition a bit:

````haskell
          sq3 : Square A (s̮ ∙∙ p ∙∙ (sym ŝ)) p s̮ ŝ
          sq3 i j =
            subst (λ a' → Square A a' p s̮ ŝ)
             (∙-two-nested-to-∙∙ s̮ p (sym ŝ)) sq2 i j
````

Though I guess this is a bit loaded, so maybe the authors expect a simpler way without all this. But we still have to deal with the `t` issues, so we don't have a solution.

2026-09-29 Wk 40 Tue - 20:37 +03:00

Next, maybe we should try to find a path from `(s̮ ∙∙ p ∙∙ (sym ŝ))` to `ap (λ a → r .map (r .section .map a)) p `.

2026-09-30 Wk 40 Wed - 08:10 +03:00

We did some more elaboration, finding out that the map is `q` and the section map is `pB`.

`q`  makes use of `t` which comes from when we don't have access to `p` necessarily, but do have access to `pB`. So it seems equating `t`  and `t₂`

````haskell
t : (pB : b̬ ≡ b̂) → r̬ ≡ r̂
t pB = ap (λ b → r .map b) pB
t₂ : (p : a̬ ≡ â) → r̬ ≡ r̂
t₂ p = s̬ ∙∙ p ∙∙ (sym ŝ)
````

So it's unclear how we can break down `t` here:

````haskell
--               —  
--         r̂  — — — >  r̂        
--         ^           ^              ^
--      t  |     A     | t₂         j |
--         |           |              . — >
--         r̮  — — — >  r̬                i
--               —  

--               —  
--         r̂  — — — >  r̂        
--         ^           ^               
--     ~ ŝ |     A     | ~ ŝ             
--         |     —     |                   
--         â  — — — >  â                 
--         ^           ^               
--       ? |     A     | p             
--         |     —     |                   
--         a̬  — — — >  a̬                 
--         ^           ^              ^
--       s̬ |     A     | s̬          j |
--         |           |              . — >
--         r̮  — — — >  r̬                i
--               —  
````

It's not going to be `q`, since `q` is built off of it, and our goal in the first place *is* `q (pB p) ≡ p`, so that would not constitute a subproblem.

We can't use `p`, of course, since then that's just `t₂`, not `t`. So either way it seems this attempt at breaking it down won't work, and we should try something else.

Partial work up until now is at https://codeberg.org/lan22h-experiments/note-files/src/branch/main/paste/69fc762b.

We save it so we can rewind and try something else.

2026-09-30 Wk 40 Wed - 08:54 +03:00

If we had `t≡t₂` we could just solve for this more directly:

````haskell
--                     ↓ —
--                â — — — — — — — — > â
--              / ^                 / ^
--        q → /   |               /   |
--          /     |             / ← p |
--        a̬ — — — — — — — — > a̬       |
--        ^       | — ↑       ^       |                    ^   j
--        |   ŝ → |           |       | ← ŝ              k | /
--        |       |           |       |                    ∙ — >
--    s̬ → |       |    ↓ —    |       |                      i
--        |       r̂ — — — — — | — — > r̂
--        | t → /         s̬ → |     /
--        |   /               |   / ← t₂
--        | /                 | /
--        r̬ — — — — — — — — > r̬
--                ↑ —
-- values:
-- r̬  = r .map (r .section .map a̬ )
--  r̂ = r .map (r .section .map  â)
-- lines:
-- s̬₁  = r .section .proof a̬
--  ŝ₁ = r .section .proof  â
-- t = ap (λ a → r .map (r .section .map a)) p
-- t₂ = s̬ ∙∙ p ∙∙ (sym ŝ)
-- q = (sym s̬) ∙∙ t ∙∙ ŝ
````

so could it be the hint suggested a different square so we take a different route from requiring `t≡t₂`? Because we ended up making the `(k = i0)` square in the hint require this for its construction.

````haskell
sq-k0 :
    (p : a̬ ≡ â)
  → Square A (t (pB p)) p s̬ ŝ
````

2026-09-30 Wk 40 Wed - 09:10 +03:00

This is the hint cube:

````
                y — — — — — — — — > y
              / ^                 / ^
      .map  /   |            p  /   |
          /     |             /     |
        x — — — — — — — — > x       |
        ^       |           ^       |                    ^   j
        |       |           |       |                  k | /
        |       |           |       |                    ∙ — >
        |       |           |       |                      i
        |    r (s y)  — — — | — — > y
        |     /             |     /
        |   /               |   /
        | /                 | /
     r (s x) — — — — — — —  x
````

They didn't use a double composition for q, but some `.map`. Could that mean something else?

I interpreted it as the double composite because of this hint:

````
-- Hint: Using ∙∙ gives the cleanest argument!
````

2026-09-30 Wk 40 Wed - 10:48 +03:00

````haskell
ĝ : B → A
ĝ = r .map
f : A → B
f = r .section .map
````

Could we make use of of the fact that ĝ and f are one-directional inverses to turn `ap (ĝ ∘ f) p` into `ap (idfun) p`?

The issue is doing that would no longer have it be in the path `r̬ ≡ r̂`.

In any case, instead of `t₂` maybe we can use `t₃` which builds from `p` but also computationally reduces like `t`

````haskell
t : (pB : b̬ ≡ b̂) → r̬ ≡ r̂
t pB = ap ĝ pB
t₂ : (p : a̬ ≡ â) → r̬ ≡ r̂
t₂ p = s̬ ∙∙ p ∙∙ (sym ŝ)
t₃ : (p : a̬ ≡ â) → r̬ ≡ r̂
t₃ p = ap (ĝ ∘ f) p
````

````haskell
t≡t₃ :
    (p : a̬ ≡ â)
  → t (pB p) ≡ t₃ p
t≡t₃ p = refl
````

The reason I'm calling `r .map` `ĝ` is because it's the retract map from the original equivalence.

Since `t₃` reduces like `t`, it doesn't seem to help. We're still unsure how to solve the `(k = i0)` face

````haskell
Sq : Square A (t₃ p) p s̬ ŝ
Sq i j = {!!}
````

2026-09-30 Wk 40 Wed - 11:35 +03:00

````haskell
retract-≡ .map pB i = {!subst (λ (f' : A → A) → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ pB) i!}
````

this is not resolving. It resolves when we undo

````haskell
retract-≡ .section .proof p = {!h0!}
````

I guess because this is a different solution from `q`.

Now our goal instead is

````haskell
Goal: (λ i →
         subst (λ f' → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ (pB p)) i)
      ≡ p
````

````haskell
retract-≡ .map pB = subst (λ (f' : A → A) → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ pB)
````

Removing the `i` simplified the goal here

````haskell
retract-≡ .section .proof p = {!!}
````

````haskell
Goal: subst (λ f' → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ (pB p)) ≡ p
````

````haskell
-- C-u C-u C-,
Goal: transport-fixing
      (λ i → r .section .proof a̬ i ≡ r .section .proof â i) i0
      (λ i → r .map (r .section .map (p i)))
      ≡ p
````

though this looks even more complicated to work with than `q`.

2026-09-30 Wk 40 Wed - 12:17 +03:00

It allows us to try to reverse the roles, where `t` depends on `q` instead:

````haskell
q₂ : (pB : b̬ ≡ b̂) → a̬ ≡ â
q₂ pB = subst (λ (f' : A → A) → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ pB)
t₄ : (pB : b̬ ≡ b̂) → r̬ ≡ r̂
t₄ pB = s̬ ∙∙ (q₂ pB) ∙∙ (sym ŝ)
````

Not that this would buy us much, since we still cannot equate `q₂` with `p`.

2026-09-30 Wk 40 Wed - 12:55 +03:00

````
The terms
  ((i ∨ ~ i) ∨ j ∨ ~ j) ∨ ~ k
and
  (i ∨ ~ i) ∨ j ∨ ~ j
are not equal at type I
when checking that the expression retract-≡-squareTube p i j has
type (k : I) → Partial ((∂ i ∨ ∂ j) ∨ ~ k) _A_803
````

Yup `(k = i0)` is mandatory in the cube to fill the lid at `(k = i1)`, since it is `(k = i0)` that slides across k to make it walled by the formula for the square faces between `(k = i0)` and `(k = i1)`.

2026-09-30 Wk 40 Wed - 17:03 +03:00

Okay, had help from ncf again who helped me some months back. It did not cross my mind to just try `proof (p j) i` for the `(k = i0)` case.

2026-10-01 Wk 40 Thu - 13:34 +03:00

Let's clean up, we don't need all the one letter variables for this. I was doing it to explore and draw cubes by hand faster but it can hinder readability. It might also have had effect in me not trying that solution since I was a bit fixated on drawing cubes and trying different paths in it.

````haskell
private
  ĝ : B → A
  ĝ = r .map
  f : A → B
  f = r .section .map
  s : (a : A) → (ĝ ∘ f) a ≡ a
  s = r .section .proof
  b̬ : B
  b̬ = f a̬
  b̂ : B
  b̂ = f â
  r̬ : A
  r̬ = ĝ b̬
  r̂ : A
  r̂ = ĝ b̂
  s̬ : r̬ ≡ a̬
  s̬ = s a̬
  ŝ : r̂ ≡ â
  ŝ = s â
  pB : (p : a̬ ≡ â) → b̬ ≡ b̂
  pB p = ap f p 
  t : (pB : b̬ ≡ b̂) → r̬ ≡ r̂
  t pB = ap ĝ pB
  t₂ : (p : a̬ ≡ â) → r̬ ≡ r̂
  t₂ p = s̬ ∙∙ p ∙∙ (sym ŝ)
  t₃ : (p : a̬ ≡ â) → r̬ ≡ r̂
  t₃ p = ap (ĝ ∘ f) p
  q : (pB : b̬ ≡ b̂) → a̬ ≡ â
  q pB = (sym s̬) ∙∙ (t pB) ∙∙ ŝ
  ĝ∘f≡idfun : ĝ ∘ f ≡ idfun
  ĝ∘f≡idfun i a = r .section .proof a i
  q₂ : (pB : b̬ ≡ b̂) → a̬ ≡ â
  q₂ pB = subst (λ (f' : A → A) → f' a̬ ≡ f' â) ĝ∘f≡idfun (ap ĝ pB)
````
