---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md)

Spawned in: [^spawn-entry-99ae27](005%20CQTS%20Problem%20Entries.md#spawn-entry-99ae27)

# Journal

2026-09-06 Wk 36 Sun - 09:56 +03:00

This is in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda`.

````haskell
_+pos_ : ℤ → ℕ → ℤ
a +pos zero = a
a +pos suc b = sucℤ (a +pos b)

-- pos a    +pos b = pos (a + b)
-- negsuc a +pos zero  = negsuc a
-- negsuc a +pos suc b = (sucℤ (negsuc a)) +pos b

_+negsuc_ : ℤ → ℕ → ℤ
a +negsuc zero = predℤ a
a +negsuc suc b = predℤ (a +negsuc b)

-- pos a +negsuc zero = predℤ (pos a)
-- pos a +negsuc suc b = predℤ ((pos a) +negsuc b)
-- negsuc a +negsuc b = {!!}
````

I figured instead of the commented out, it is shorter to solve this using our prior definitions of `predℤ` and `sucℤ`. Removing a suc from a `negsuc` results in rolling out one `predℤ` of the whole. And for the pos, it instead results in a `sucℤ`.
