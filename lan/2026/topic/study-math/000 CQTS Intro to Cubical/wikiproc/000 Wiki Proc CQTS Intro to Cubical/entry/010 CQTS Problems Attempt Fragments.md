---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned in: [^spawn-entry-a14d59](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md#spawn-entry-a14d59)

# What?

* [005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md)
  * Here we are commenting on a solution that is complete, but providing commentary to supplement it.
* $\to$ [CQTS Problems Attempt Entries](CQTS%20Problems%20Attempt%20Entries.md)
  * Here we have journals for each problem as we're working through them.
* $\to$ here

This is yet another level for the journals. To keep them clean and record partial attempt fragments, each entry here can have some partial work, errata, deadends, etc.

Just like [005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md), it is organized with one heading-2 per entry under `# Journal`. It then includes things changed in the journal, abandoned paths, side comments, etc.

The reason for this distinction is that a journal can itself be interpreted as a journey from problem to solution, which means that dead ends can fragment its explanatory trajectory. But these are process notes, so we want to preserve the bad experiments. So we just reduce them to single lines pointed by the original journal, to keep the main thread focused and not lose the non-linear aspect of exploring the problem.

We do not want to go on forever with this, making subentries of subentries. This entry is explicitly fragmented, so we will accomodate non-linear connections of records more here.

Partial source code can also be preserved in the form of pastes.

Also, the journal can accommodate *some detours* to reach the correct solution, if it's judged to be a primary step towards the eventual answer. But in that case, it still would not be too fragmented. Note that *different attempts from scratch* are still also within scope for [CQTS Problems Attempt Entries](CQTS%20Problems%20Attempt%20Entries.md).

Of course, if an effort gets very involved, then it can be its own dedicated note with its own note architecture.

# Journal

## `J-ump-≃` - Tried to extend the type of Q

2026-09-20 Wk 38 Sun - 21:17 +03:00

\[\[CQTS Problems Attempt Entries#`J-ump-≃`\]\]

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ : A}
  where
    J-ump-≃ :
        (Q : {A : Type ℓ} {a̬ : A} → (a : A) → a̬ ≡ a → Type ℓ)
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
        fro-to f i a p = h0 i
          where
            r : Q a̬ refl
            r = f a̬ refl
            Q₁ : (a' : Q a p) → (p' : Path (Q a p) (J Q r p) a') → Type ℓ
            Q₁ a' p' = Q {A = Q a p} {a̬ = (J Q r p)} a' p' 
            r₁ : Q₁ (J Q r p) refl
            r₁ =  r₁'
              where
                r₁' : Q {A = Q a p} {a̬ = (J Q r p)} (J Q r p) refl
                r₁' = {!f!}
            h1 : Path (Q a̬ refl) (J Q r refl) (f a̬ refl)
            h1 i = J-refl Q (f a̬ refl) i
            h0 : Path (Q a p) (J Q r p) (f a p)
            h0 j = {!J Q₁!}
````

But this changes the problem if I allow the type to vary.

It should suffice that `J` isn't within the same module, so its `Q` can be varied.

## `J-ump-≃` - Q might already take into account path and refl variance

2026-09-20 Wk 38 Sun - 23:11 +03:00

\[\[CQTS Problems Attempt Entries#`J-ump-≃`\]\]

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ : A}
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
        fro-to f i a p = h0 i
          where
            r : Q a̬ refl
            r = f a̬ refl
            Q₁ : (a' : Q a p) → (p' : Path (Q a p) (J Q r p) a') → Type ℓ
            Q₁ a' p' = {!Path _ (Path (Q a̬ refl) (J Q r refl) (f a̬ refl)) (Path (Q a' p') (J Q r p) (f a' p'))!}
            Q₂ : (a' : A) → a̬ ≡ a' → Type ℓ
            Q₂ a' p' = {!Q (p i)!}
            Q₂ a' p' = {!PathP (λ j → Path (Q (p' j) p') (J Q r p') (f (p' i) p')) !}
            Q a' p' = {!PathP (λ i → Path (Q (p' i) p') (J Q r p') (f (p' i) p')) (Path (Q a̬ refl) (J Q r refl) (f a̬ refl)) (Path (Q a' p') (J Q r p') (f a' p'))!}
            h1 : Path (Q a̬ refl) (J Q r refl) (f a̬ refl)
            h1 i = J-refl Q (f a̬ refl) i
            h0 : Path (Q a p) (J Q r p) (f a p)
            h0 j = {!J Q₁!}
````

In Q₂ I was trying to define a path that accommodates both a̬ and a', so I was trying to use `(p j) p'` but running into issues that `(p j)` is not being computationally reduced, and thus not in the form `a̬ ≡ a` expected by `Q`.

But also I don't think I need to do this.

````haskell
module _
  {ℓ : Level}
  {A : Type ℓ}
  {a̬ â : A}
  where
    J-line : 
        (Q : (a : A) → a̬ ≡ a → Type ℓ)
      → (p : a̬ ≡ â)
      → Q a̬ refl ≡ Q â p
    J-line Q p i = Q (p i) (connection∧ p i)
      -- connection∧ lets us construct a path refl → p
````

You see here that `Q` already accommodates both `a̬ â` and `refl p`. So long as the left is a̬, the right can be whatever is the first arg of `Q`.

The point of this though is I'm trying to find a path from `h1` to `h0`.

Still I want to `Q a̬ refl` and `Q a p` are different types, even if `refl` and `p` are settled by choice of `a`.

2026-09-21 Wk 39 Mon - 01:25 +03:00

Having issue using this which is at universe level `(ℓ-suc ℓ)` when universe level `ℓ` is expected.

````haskell
Q₁ : (a' : A) → a̬ ≡ a' → Type (ℓ-suc ℓ)
Q₁ a' p' = (Path (Q a̬ refl) (J Q r refl) (f a̬ refl)) ≡ (Path (Q a' p') (J Q r p') (f a' p'))
r₁ : Q₁ a̬ refl
r₁ = r₁-1
  where
	r₁-1 : (Path (Q a̬ refl) (J Q r refl) (f a̬ refl)) ≡ (Path (Q a̬ refl) (J Q r refl) (f a̬ refl))
	r₁-1 = refl
````

2026-09-21 Wk 39 Mon - 01:55 +03:00

\[\[\#`J-ump-≃` - Errata Specified value f a p and J Q r p as path\]\]

Since we want a path  `h0 : Path (Q a p) (J Q r p) (f a p)` we could try to derive this from some square

````haskell
--                  ? 
--            ?  — — — >  f a p    
--            ^           ^          ^
--          ? |     ?     | h0     j |
--            |           |          . — >
--            ?  — — — >  J Q r p      i
--                  ? 
````

````haskell
h1 : Path (Q a̬ refl) (J Q r refl) r
h0 : Path (Q a p) (J Q r p) (f a p)
````

Gives us four values:

````
Q a̬ refl : (J Q r refl)  r
Q a p    : (J Q r p)     (f a p)
````

````haskell
--                  ? 
--      f a̬ refl — — — >  f a p    
--            ^           ^          ^
--         h1 | Q (p i) p | h0     j |
--            |           |          . — >
--    J Q r refl — — — >  J Q r p      i
--                  ? 
-- r = f a̬ refl
````

Though we can't make this, because we don't have the side `h0`, and we shouldn't be solving this with filling a subcube.

## `J-ump-≃` - Errata: Specified value f a p and J Q r p as path

2026-09-21 Wk 39 Mon - 02:02 +03:00

In \[\[\#`J-ump-≃` - Q might already take into account path and refl variance\]\] I wrote

(quote)

Since we want a path  `h0 : Path (Q a p) (J Q r p) (f a p)` we could try to derive this from some square

````haskell
--                  ? 
--            ?  — — — >  ?        
--            ^           ^          ^
--    J Q r p |     ?     | f a p  j |
--            |           |          . — >
--            ?  — — — >  ?            i
--                  ? 
````

(/quote)

But `f a p` is not a path. It's a value of type `Q a p`.  Might be better to say we need a square like this:

````haskell
--                  ? 
--            ?  — — — >  f a p    
--            ^           ^          ^
--          ? |     ?     | h0     j |
--            |           |          . — >
--            ?  — — — >  J Q r p      i
--                  ? 
````

if we've had a square `Square _ p q refl refl` for example, and we had the path `p`, we could derive the path `q`. But also at this point we haven't reached subcube filling.

## `J-ump-≃` - Side Notes

2026-09-21 Wk 39 Mon - 02:21 +03:00

Reminder,

````haskell
J :
	(Q : (a : A) → a̬ ≡ a → Type ℓ)
  → (r : Q a̬ refl)
  → (p : a̬ ≡ â)
  → Q â p
J Q r p = transport (J-line Q p) r
````

2026-09-21 Wk 39 Mon - 02:34 +03:00

Didn't end up needing this. I made it since for every `Q` we make, we need a refl version to use with `J`, but that was already what I was desigining when making `h0` and `h1`, so it was exactly `h1`.

````haskell
r₂ : Q₂ a̬ refl
r₂ = r₂-1
  where
	r₂-1 : Path (Q a̬ refl) (J Q r refl) (f a̬ refl)
	r₂-1 = h1
````
