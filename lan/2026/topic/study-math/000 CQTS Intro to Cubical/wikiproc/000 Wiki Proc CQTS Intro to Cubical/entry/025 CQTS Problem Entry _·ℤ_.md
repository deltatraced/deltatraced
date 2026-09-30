---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries#^spawn-entry-6c6520|^spawn-entry-6c6520]]

# Journal

2026-09-06 Wk 36 Sun - 11:14 +03:00

This is in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda`.

```lua
-- Hint: case split on only one of the sides 
_·ℤ_ : ℤ → ℤ → ℤ
pos zero ·ℤ b = pos zero
pos (suc a) ·ℤ b = b +ℤ ((pos a) ·ℤ b)
negsuc a ·ℤ b = - ((pos (suc a)) ·ℤ b)
```

This gives us an error.

```lua
-- Hint: case split on only one of the sides 
_·ℤ_ : ℤ → ℤ → ℤ
pos zero ·ℤ b = pos zero
pos (suc a) ·ℤ b = b +ℤ ((pos a) ·ℤ b)
negsuc a ·ℤ b = - ((pos (suc a)) ·ℤ b)
```

```sh
# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1
mikan src/lect-1-2-inductive-types.agda

# out (relevant)
error: [TerminationIssue]
Termination checking failed for the following function:
  _·ℤ_
Problematic call:
  pos (suc a) ·ℤ b
    (at /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda:392.34-36)
```

I guess there is no structural decrement in the definition of `negsuc a ·ℤ b`, so it rejects it.
