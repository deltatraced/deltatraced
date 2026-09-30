---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[009 CQTS Problems Attempt Entries]]

Spawned in: [[009 CQTS Problems Attempt Entries#^spawn-entry-2f20be|^spawn-entry-2f20be]]

# Journal

2026-09-22 Wk 39 Tue - 07:03 +03:00

in `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-4-composition-and-filling.agda`,

in `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/2--Paths-and-Identifications/2-4--Composition-and-Filling.lagda.md`,

```haskell
 _∙∙_∙∙_ :
     (r : a ≡ b)
   → (p : b ≡ c)
   → (q : c ≡ d)
   → (a ≡ d)
 (r ∙∙ p ∙∙ q) i = hcomp (∂ i) ((double-comp-box r p q i))    diamond-tube-alt p q i j k (i = i0) = {!!}
    diamond-tube-alt p q i j k (i = i1) = {!!}

```

Why is it that hcomp for this only takes an interval formula that is i1 at (i=i0) and (i=i1), when the open box also has (j=i0) as a side?

CQTS Authors explain:

```
Writing more formally, ``hcomp`` takes in two arguments:

* A formula `φ : I`, specifying which sides of the box are going to be
  present in the next argument.
  
  For the double composite, we are specifying the left and right sides
  of the square, that is, the places where where `∂ i` holds.
  
* An open box `box : (j : I) → Partial (φ ∨ ~ j) A`. We can think of
  `box` as specifying a vertical slice of the open box as `j` runs
  from ``i0`` to ``i1``. Each vertical slice will be defined in more
  or fewer places depending on the formula `φ`. When `j` is ``i0``,
  the formula `φ ∨ ~ j` always holds and so these vertical slices are
  all defined when `j = i0`.

  For the double composite, `box j` is totally defined when `i = i0`,
  because we are on the left side of the square in that case, and
  similarly when `i = i1` and we are on the right side of the square.
  Otherwise, we only know for sure that `box j` is defined when `j =
  i0`, in which case we are on the bottom of the square.
```


Well box is `box : (i j : I) → Partial (open-box i j) A`, it seems they mean this when evaluated at `i` to the first argument of hcomp `∂ a`:

- `box : (i j : I) → Partial ((~ i) ∨ i ∨ (~ j)) A`
- $\to$ `box : (j : I) → Partial (∂ i ∨ (~ j)) A`

The other thing is that ` _∙∙_∙∙_` is a 1-cube path. In its very definition, it could not supply us with more than one interval. 

```
--                  
--         a  .......  d        
--         ^           ^              ^
--   sym r |     A     | q          j |
--         |           |              . — >
--         b  — — — >  c                i
--              p   
```

So maybe we should interpret the the first argument of `hcomp` as specifying the endpoints of the lid 1-cube to fill in, which runs from the face (i = i0) to the face (i = i1).

They did mention that `box` slides from `j = i0` to `j = i1`...

At `i = i0` `box j` should resolve to `(sym r) j`, and at `i = i1` `box j` should resolve to `q j`.

So it seems for any partial n-cube open box, in order to perform hcomp on it (close the lid), we need to specify as the first argument of hcomp a formula for an (n-1) cube face sliding in the direction of the open (n-1) cube face.

In our case this is a formula of 1-cube faces, sliding in the direction of the open lid (i = i0 $\to$ i = i1). So one variable is always left unspecified in the first argumen: the one that is defined as the upper surface of the sweep. 

Then because only one missing variable remains in `box j` (no matter the n-dimension of the subcube), its formula should always be `(φ ∨ ~ j)`, the second argument expects our third defined case already as the only remaining pattern `(j = i0)` we need to define to close the lid and perform the sweep. 

If we omit the j case with this bad `double-comp-box'`, it will complain about it:

```haskell
    double-comp-box' :
        (r : a ≡ b)
      → (p : b ≡ c)
      → (q : c ≡ d)
      → (i j : I)
      → Partial (∂ i) A
    double-comp-box' r p q i j (i = i0) = sym r j
    double-comp-box' r p q i j (i = i1) = q j

    -- We start with the double composition as it readily forms closing the lid
    -- on an open box.
    -- hcomp takes a formula for the *sides* 
    _∙∙_∙∙_ :
        (r : a ≡ b)
      → (p : b ≡ c)
      → (q : c ≡ d)
      → (a ≡ d)
    (r ∙∙ p ∙∙ q) i = hcomp (∂ i) ((double-comp-box' r p q i))
							--      ~~~~~~~~~~~~~~~~~~~~~~~~ problem
```

```
The terms
  (i ∨ ~ i) ∨ ~ j
and
  i ∨ ~ i
are not equal at type I
when checking that the expression double-comp-box' r p q i has type
(j : I) → Partial (∂ i ∨ ~ j) _A_301
```

Another way to state it is that the formula φ specifies the boundaries of the lid subcube. For a line, those are the endpoints, but for a square it would require 2 interval variables to get its boundaries within the cube.

And because the result is a face, the number of intervals we have access to is only as much as (n-1)-cube, hence we can't specify the extra bottom face there anyway.

2026-09-22 Wk 39 Tue - 12:17 +03:00

Seeing it as specifying the boundaries also helps explain why for `∙∙-filler` we do not include all sides, only `~ i ∨ ∂ j`, since we do not specify a face at (i = i1), leaving it to hcomp to fill it by inferring the shared line between `(k = i1)` and `(i = i1)`, so that boundary is missing in the formula as a result.

2026-09-22 Wk 39 Tue - 08:45 +03:00

https://mzhang.io/posts/2024-09-18-hcomp/indexlagda/
