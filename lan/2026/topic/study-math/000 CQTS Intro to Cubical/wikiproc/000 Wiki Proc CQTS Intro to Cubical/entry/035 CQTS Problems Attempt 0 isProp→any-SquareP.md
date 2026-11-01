---
context_type: entry
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries#^spawn-entry-99effc|^spawn-entry-99effc]]
# Journal

2026-10-03 Wk 40 Sat - 14:11 +03:00

```haskell
module _
  {ℓ : Level}
  {A : I → I → Type ℓ}
  {a : A i0 i0}
  {b : A i0 i1}
  {c : A i1 i0}
  {d : A i1 i1}
  where
    isProp→any-SquareP :
        (prA : (i j : I) → isProp (A i j))
      → (pac : PathP (λ j → A j i0) a c)
      → (pbd : PathP (λ j → A j i1) b d)
      → (pab : PathP (λ j → A i0 j) a b)
      → (pcd : PathP (λ j → A i1 j) c d)
      → SquareP A pab pcd pac pbd
    isProp→any-SquareP prA pac pbd pab pcd = {!!}
```

Maybe we can reuse `isProp→Square` for this, but it only has one type. So maybe we need to transport everything to the sink at `A i1 i1`. 

2026-10-03 Wk 40 Sat - 14:40 +03:00

Well it doesn't get us to the goal, we just get this, but how would we use it?

```haskell
a₀₁ : A i0 i1
a₀₁ = transport (λ j → A i0 j) a
a₁₁ : A i1 i1
a₁₁ = transport (λ i → A i i1) a₀₁
b₁₁ : A i1 i1
b₁₁ = transport (λ i → A i i1) b
c₁₁ : A i1 i1
c₁₁ = transport (λ j → A i1 j) c
pac₁ : a₁₁ ≡ c₁₁
pac₁ = prA i1 i1 a₁₁ c₁₁
pbd₁ = prA i1 i1 b₁₁ d
pab₁ = prA i1 i1 a₁₁ b₁₁
pcd₁ = prA i1 i1 c₁₁ d
sq0 : Square (A i1 i1) pab₁ pcd₁ pac₁ pbd₁
sq0 = isProp→Square (prA i1 i1) pab₁ pbd₁ pac₁ pcd₁
```

2026-10-03 Wk 40 Sat - 21:12 +03:00

Can we even hcomp this? What would the face `(k = i0)` be?

It seems we also need `PartialP`, which we've not used to this point (and never suggested in these notes) so it is very unlikely we need it.

2026-10-03 Wk 40 Sat - 21:50 +03:00

Not sure if these will be useful

```haskell
prac : isProp (PathP (λ i → A i i0) a c)
prac = isProp→isProp-PathP (λ i → prA i i0) a c
prbd : isProp (PathP (λ i → A i i1) b d)
prbd = isProp→isProp-PathP (λ i → prA i i1) b d
prab : isProp (PathP (λ j → A i0 j) a b)
prab = isProp→isProp-PathP (λ j → prA i0 j) a b
prcd : isProp (PathP (λ j → A i1 j) c d)
prcd = isProp→isProp-PathP (λ j → prA i1 j) c d
```

2026-10-03 Wk 40 Sat - 21:52 +03:00

We didn't need the double transport for `a₁₁`:

```haskell
a₁₁ : A i1 i1
a₁₁ = transport (λ i → A i i) a
```

2026-10-04 Wk 40 Sun - 11:58 +03:00

If we construct all the paths ourselves, we can make a similar `SquareP`:

```haskell
pac' : PathP (λ i → A i i0) a c
pac' i = isProp→any-PathP (λ j → prA j i0) a c i 
pbd' : PathP (λ i → A i i1) b d
pbd' i = isProp→any-PathP (λ j → prA j i1) b d i
pab' : PathP (λ j → A i0 j) a b
pab' i = isProp→any-PathP (λ j → prA i0 j) a b i
pcd' : PathP (λ j → A i1 j) c d
pcd' i = isProp→any-PathP (λ j → prA i1 j) c d i
sq0 : SquareP A pab' pcd' pac' pbd'
sq0 i j = isProp→any-PathP (λ k → prA i k) (pac' i) (pbd' i) j  
```

2026-10-04 Wk 40 Sun - 12:03 +03:00

More terms not sure if they will be helpful:

```haskell
pr-pab≡pcd : (j : I) → isProp (PathP (λ i → A i j) (pab j) (pcd j))
pr-pab≡pcd j = isProp→isProp-PathP (λ i → prA i j) (pab j) (pcd j)
pr-pac≡pbd : (i : I) → isProp (PathP (λ j → A i j) (pac i) (pbd i))
pr-pac≡pbd i = isProp→isProp-PathP (prA i) (pac i) (pbd i)
```

They want entire paths supplied to them, and then they can make `SquareP`s.

2026-10-04 Wk 40 Sun - 12:10 +03:00

This works, though it looks sort of overengineered:

```haskell
isProp→any-SquareP :
    (prA : (i j : I) → isProp (A i j))
  → (pac : PathP (λ j → A j i0) a c)
  → (pbd : PathP (λ j → A j i1) b d)
  → (pab : PathP (λ j → A i0 j) a b)
  → (pcd : PathP (λ j → A i1 j) c d)
  → SquareP A pab pcd pac pbd
isProp→any-SquareP prA pac pbd pab pcd i j = sq4 i j
  where
    pac' : PathP (λ i → A i i0) a c
    pac' i = isProp→any-PathP (λ j → prA j i0) a c i 
    pbd' : PathP (λ i → A i i1) b d
    pbd' i = isProp→any-PathP (λ j → prA j i1) b d i
    pab' : PathP (λ j → A i0 j) a b
    pab' i = isProp→any-PathP (λ j → prA i0 j) a b i
    pcd' : PathP (λ j → A i1 j) c d
    pcd' i = isProp→any-PathP (λ j → prA i1 j) c d i
    sq0 : SquareP A pab' pcd' pac' pbd'
    sq0 i j = isProp→any-PathP (λ k → prA i k) (pac' i) (pbd' i) j  
    prac : isProp (PathP (λ i → A i i0) a c)
    prac = isProp→isProp-PathP (λ i → prA i i0) a c
    prbd : isProp (PathP (λ i → A i i1) b d)
    prbd = isProp→isProp-PathP (λ i → prA i i1) b d
    prab : isProp (PathP (λ j → A i0 j) a b)
    prab = isProp→isProp-PathP (λ j → prA i0 j) a b
    prcd : isProp (PathP (λ j → A i1 j) c d)
    prcd = isProp→isProp-PathP (λ j → prA i1 j) c d
    sq1 : SquareP A pab pcd' pac' pbd'
    sq1 i j = subst (λ s → SquareP A s pcd' pac' pbd') (prab pab' pab) sq0 i j
    sq2 : SquareP A pab pcd pac' pbd'
    sq2 i j = subst (λ s → SquareP A pab s pac' pbd') (prcd pcd' pcd) sq1 i j
    sq3 : SquareP A pab pcd pac pbd'
    sq3 i j = subst (λ s → SquareP A pab pcd s pbd') (prac pac' pac) sq2 i j
    sq4 : SquareP A pab pcd pac pbd
    sq4 i j = subst (λ s → SquareP A pab pcd pac s) (prbd pbd' pbd) sq3 i j
```

But since I can derive a square `SquareP A pab' pcd' pac' pbd'`, then I can use the prior facts like `prac` to turn each edge via subst into the one I want.

Hopefully `subst` doesn't come and make issues for me later if I end up having to unpack this definition and see a bunch of nested transports.

2026-10-04 Wk 40 Sun - 12:32 +03:00

We can at least simplify it, `sq0` can already be half complete:

```haskell
isProp→any-SquareP :
    (prA : (i j : I) → isProp (A i j))
  → (pac : PathP (λ j → A j i0) a c)
  → (pbd : PathP (λ j → A j i1) b d)
  → (pab : PathP (λ j → A i0 j) a b)
  → (pcd : PathP (λ j → A i1 j) c d)
  → SquareP A pab pcd pac pbd
isProp→any-SquareP prA pac pbd pab pcd i j = sq2 i j
  where
    pab' : PathP (λ j → A i0 j) a b
    pab' i = isProp→any-PathP (λ j → prA i0 j) a b i
    pcd' : PathP (λ j → A i1 j) c d
    pcd' i = isProp→any-PathP (λ j → prA i1 j) c d i
    sq0 : SquareP A pab' pcd' pac pbd
    sq0 i j = isProp→any-PathP (λ k → prA i k) (pac i) (pbd i) j  
    prab : isProp (PathP (λ j → A i0 j) a b)
    prab = isProp→isProp-PathP (λ j → prA i0 j) a b
    prcd : isProp (PathP (λ j → A i1 j) c d)
    prcd = isProp→isProp-PathP (λ j → prA i1 j) c d
    sq1 : SquareP A pab pcd' pac pbd
    sq1 i j = subst (λ s → SquareP A s pcd' pac pbd) (prab pab' pab) sq0 i j
    sq2 : SquareP A pab pcd pac pbd
    sq2 i j = subst (λ s → SquareP A pab s pac pbd) (prcd pcd' pcd) sq1 i j
```

