---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-3186e4](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-3186e4)

# Journal

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
