---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries#^spawn-entry-81da03|^spawn-entry-81da03]]

# Journal

2026-10-06 Wk 41 Tue - 17:58 +03:00

```haskell
isHLevel-suc :
    {A : Type ℓ}
  → (n : ℕ)
  → (hA : isHLevel n A)
  → isHLevel (suc n) A
isHLevel-suc zero hA x y = IsContr→IsContr≡ hA x y
isHLevel-suc (suc n) hA x y p q = isHLevel-suc {A = x ≡ y} n (hA x y) p q
```

This one is an interesting use of induction. It works both horizontally and vertically. The crucial work of showing that contraction applies to the path space always happens in the zero case, but it's always a different zero case, because in each step what we're doing is transferring the type itself, and we already `hA` which describes that horizontal transfer.

```
0   0   0   0   0
^   ^   ^   ^   ^
1   | v |   |   |
2 > 1   |   |   |
  h     |   |   |
3 > 2 > 1   |   |
4 > 3 > 2 > 1   |
5 > 4 > 3 > 2 > 1
```

So in the horizontal move `h`, we prepare a term of `isHLevel n A'` for a different `A'`: the path space of `A`. `hA` allows us exactly to build the `(hA x y)` the new term's `hA`. 

The vertical move `v` always happens between case 1 and case 0, and is identical to the final `h`., once we've built the nth path space being contractible, then we get via `IsContr→IsContr≡` that its `n+1`nth path space also is contractible.

So the two cases gave us the ability to perform two actions, either of morphing the type, or of handling the final `IsContr→IsContr≡`. It's pretty different from when they coincide, and instead induction looks like it's building a chain. This would be true if all cases talked about the same `0` case, but here they do not. each `0` case talks about `A`, or `x ≡ y` or `p ≡ q` and so on.

```
0 0 0 0 0
  ^ ^ ^ ^
  1 1 1 1
    ^ ^ ^
    2 2 2
	  ^ ^
      3 3
        ^
        4
```

The horizontal-vertical is a bit misleading, because they're one and the same. It's more important to see that the initial choice of `n` leads to a family of chains rather than a continuation of the same chain.

It's not enough to say these chains are not identical because there are many terms at `h-level 0`, there are in fact many terms at each level.

We can also look at at it like this, the initial `n` determines a whole new chain. There is no sharing at all.

Consider if each type `A`, the next letter is the path space in it. so `B` is the path space in `A`, and `C` is the path space in `B`, etc.

The numbers denote the complexity or h-level at that point. `(A0 = contractible, A1 = proposition, A2 = set, A3 = groupoid, ...)`

```
A0 B0 C0 D0 E0
   ^  ^  ^  ^
   A1 B1 C1 D1
      ^  ^  ^
      A2 B2 C2
	     ^  ^
         A3 B3
            ^
            A4
```

Now this shows primarily the inductive case action, the final zero case action happens at the ends `A0`, `B0`, `C0`, ... to prove that since they are contractible, you can always show their path space also is.

But the inductive case makes use of the prior case for its path space, which also decrements the h-level.

Do be careful not to assume `A2 > A1`, if `A2` said that the type of natural numbers is a set, to assume `A2` leads to `A1` is to say it is also a proposition, and thus has only (at most) one point... This is false! the natural numbers is a set and NOT a proposition. This is why we need to be referring to the path space of `A`: `B`,  path space of `B`: `C`, and so on.

When I realized to try to change the type `A` for the `isHLevel-suc` then I realized this, otherwise it was asking for the impossible if we do not update `A` to `B`.