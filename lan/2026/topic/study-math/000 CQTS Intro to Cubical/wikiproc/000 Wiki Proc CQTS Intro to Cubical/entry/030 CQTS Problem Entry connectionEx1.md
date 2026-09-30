---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries#^spawn-entry-037cd2|^spawn-entry-037cd2]]

# Journal

2026-09-19 Wk 38 Sat - 03:28 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-2-equivalences-and-path-algebra.agda`,

```haskell
    --           p⁻¹ 
    --      b  — — — >  a        
    --      ^           ^          ^
    --    p |     A     | refl   j |
    --      |           |          . — >
    --      a  — — — >  a            i
    --          refl
    connectionEx1 :
      (p : a ≡ b)
      → Square A p refl refl (sym p)
    connectionEx1 p i j = p ((~ i) ∧ (j))
```

This could be solved by looking for short circuits:

```
———— Boundary (wanted) —————————————————————————————————————
j = i0 ⊢ a
j = i1 ⊢ p (~ i)
i = i0 ⊢ p j
i = i1 ⊢ a
```

We want `j=i0` and `i=i1` to short to a constant. `∧` shorts for i0, so `∧ j` gets us that for `j=i0`. It would short for `i0`, but we do `(~ i)` so it shorts for `i1` instead.

Similarly for

```haskell
    --              p   
    --        a  — — — >  b        
    --        ^           ^          ^
    --  sym p |     A     | refl   j |
    --        |           |          . — >
    --        b  — — — >  b            i
    --            refl
    connectionEx2 :
      (p : a ≡ b)
      → Square A (sym p) refl refl p
    connectionEx2 p i j = {!p (i ∨ (~ j))!}
```

```
———— Boundary (actual) —————————————————————————————————————
j = i0 ⊢ b
j = i1 ⊢ p i
i = i0 ⊢ p (~ j)
i = i1 ⊢ b
```

We're shorting at `j=i0` and `i=i1` (read those as the formula for the faces, so `i=i0` is the vertical left, `i=i1` is vertical right, `j=i0` is horizontal bottom, and `j=10` is horizontal top)

`∨` shorts for i1, so it takes cae of `i=i1`, and we flip `(~ j)` so it shorts `j=i0` also.