---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-3c1981](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-3c1981)

# Journal

2026-10-02 Wk 40 Fri - 02:03 +03:00

````haskell
      --                     ↓ pbd
      --                b — — — — — — — — > d
      --              / ^                 / ^
      --      pab → /   |    A          /   |
      --          /     |             / ← --|-- pcd
      --        a — — — — — — — — > c       |
      --        ^       |   ↑ pac   ^       |                    ^   j
      --        | pab → |           |       | ← pac ∙ pcd      k | /
      --        |       |           |       |                    ∙ — >
      --    — → |       |    ↓ —    |       |                      i
      --        |       a — — — — — | — — > a
      --        | — → /       pac → |     /
      --        |   /               |   / ← —
      --        | /                 | /
      --        a — — — — — — — — > a
      --                ↑ —
````

First cube I tried, but then we have issues with the face `(j = i1)`.

But now I'm trying to use `prA a a`, etc for all the sides other than the top ones. Due to this observation that they allow me to equate paths:

````haskell
sq4 : Square A (pab ∙ pbd) (pac ∙ pcd) (prA a a) (prA d d)
sq4 i j = prA ((pab ∙ pbd) j) ((pac ∙ pcd) j) i
sq3 : Square A pab (prA a b) (prA a a) (prA b b)
sq3 i j = prA (pab j) (prA a b j) i
````

````haskell
      --                        ↓ pbd
      --                   b — — — — — — — — > d
      --                 / ^                 / ^
      --         pab → /   |    A          /   |
      --             /     |             / ← --|-- pcd
      --           a — — — — — — — — > c       |
      --           ^       |   ↑ pac   ^       |                    ^   j
      -- prA a b --|---- → |           |       | ← prA a d        k | /
      --           |       |           |       |                    ∙ — >
      -- prA a a → |       | ↓ PrA a a |       |                      i
      --           |       a — — — — — | — — > a
      -- prA a a --|-- → /   prA a c → |     /
      --           |   /               |   / ← prA a a
      --           | /                 | /
      --           a — — — — — — — — > a
      --                   ↑ prA a a
````

2026-10-02 Wk 40 Fri - 02:16 +03:00

It worked!
