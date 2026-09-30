---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[009 CQTS Problems Attempt Entries]]

Spawned in: [[009 CQTS Problems Attempt Entries#^spawn-entry-3a9b6e|^spawn-entry-3a9b6e]]

# Journal

2026-09-18 Wk 38 Fri - 08:28 +03:00

Pushed attempt to `/home/lan/src/cloned/cb/lan22h-experiments/note-files/paste/cc8a4d38`. 
(Name is originally based on `sha1sum | head -c8`, but content can change afterwards. We can also just use `cat /dev/random | head -n10 | sha1sum | head -c8`.)

Expanding `fro-to` doesn't seem to help much here. Resetting back to doing `fro-to` without trying to expand the type out.

2026-09-18 Wk 38 Fri - 08:36 +03:00

```haskell
fro-to : to hasRetract ret
fro-to = {!!}
```

```
Goal: (b : B → C) →
      (λ a →
         ecd .proof .retract .map
         (ecd .map (b (eab .map (eab .proof .retract .map a)))))
      ≡ b
```

2026-09-18 Wk 38 Fri - 09:19 +03:00

```haskell
fro-to : to hasRetract ret
fro-to mbc i b = (ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i
```

```
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-2-equivalences-and-path-algebra.agda:566.9-23: error: [UnequalTerms]
The terms
  eab .proof .retract .map b
and
  eab .proof .section .map b
are not equal at type A
when checking that a clause of fro-to has the correct boundary.

Specifically, the terms
  ret eab ecd (to eab ecd mbc) b
and
  ecd .proof .retract .proof (mbc (eab .proof .section .proof b i0))
  i0
must be equal, since fro-to eab ecd mbc i0 b could reduce to
either.

```

It gives a similar goal, but actually uses `eab .proof .section` instead of `eab .proof .retract`:

```haskell
fro-to : to hasRetract ret
fro-to mbc i b = {!(ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i!}
```

```
Goal: C
———— Boundary (wanted) —————————————————————————————————————
i = i0 ⊢ ecd .proof .retract .map
         (ecd .map (mbc (eab .map (eab .proof .retract .map b))))
i = i1 ⊢ mbc b
Have: C
———— Boundary (actual) —————————————————————————————————————
i = i0 ⊢ ecd .proof .retract .map
         (ecd .map (mbc (eab .map (eab .proof .section .map b))))
i = i1 ⊢ mbc b
———— Context ———————————————————————————————————————————————
```

So this can be an issue with how we wrote `ret`:

```haskell
ret : (A → D) → (B → C)
ret mad = (ecd .proof .retract .map) ∘ mad ∘ (eab .proof .retract .map)
```

This works:

```haskell
ret : (A → D) → (B → C)
ret mad = (ecd .proof .retract .map) ∘ mad ∘ (eab .proof .section .map)

fro-to : to hasRetract ret
fro-to mbc i b = (ecd .proof .retract .proof (mbc (eab .proof .section .proof b i))) i
```

