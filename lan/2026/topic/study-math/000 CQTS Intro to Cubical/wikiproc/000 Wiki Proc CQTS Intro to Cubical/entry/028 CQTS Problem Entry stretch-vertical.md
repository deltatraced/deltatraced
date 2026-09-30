---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries#^spawn-entry-8bbe2e|^spawn-entry-8bbe2e]]

# Journal

2026-09-16 Wk 38 Wed - 06:16 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-1-paths.agda`,

```haskell
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
```

--/  2026-09-16 Wk 38 Wed - 06:17 +03:00

How do we interpret that this ignores the `j`? We can solve it by following the type checker, but how do we intuit it must be the case without it?

No matter what the value, a change in `j` will do nothing. Since we're only sensitive to horizontal change but not vertical, we see `p` down and below for `p i`.

Similarly `p j` would suggest that horizontal change is ignored, but vertical is the endpoints of `p`, so we would expect the flip:

```haskell
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
```

Since `Square` is defined via a square sweep, we can also think of `i` as picking the type of vertical slice paths, these are different in the case of `stretch-vertical` by the `p` endpoints, but nothing else differentiates them further, so then the vertical slice paths are refl.
--/
