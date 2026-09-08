---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical#^spawn-entry-3cfc10|^spawn-entry-3cfc10]]

# What?

Comments and thoughts on CQTS Problems. One subheading under `# Journal` per problem or set of closely related problems.

If the effort and exploration for a given problem is long, it should be promoted to its own note file.

# Journal

## `_+pos_` and `_+negsuc_`

2026-09-06 Wk 36 Sun - 09:56 +03:00

This is in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lect-1-2-inductive-types.agda`.

```haskell
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
```

I figured instead of the commented out, it is shorter to solve this using our prior definitions of `predℤ` and `sucℤ`. Removing a suc from a `negsuc` results in rolling out one `predℤ` of the whole. And for the pos, it instead results in a `sucℤ`.

## `_·ℤ_`

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