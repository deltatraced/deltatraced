---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md)

Spawned in: [^spawn-entry-8da0ae](005%20CQTS%20Problem%20Entries.md#spawn-entry-8da0ae)

# Journal

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
