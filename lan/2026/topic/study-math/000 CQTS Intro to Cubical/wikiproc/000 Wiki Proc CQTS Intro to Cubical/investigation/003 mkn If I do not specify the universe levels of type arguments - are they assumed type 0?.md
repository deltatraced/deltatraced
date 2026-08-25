---
context_type: investigation
status: todo
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/task/000 Reproduce how far we got in cqts but in problems-mkn module format]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/task/000 Reproduce how far we got in cqts but in problems-mkn module format#^spawn-invst-756aad|^spawn-invst-756aad]]

# Resolution

Yes any `{A : Type}` should be interpreted as `{A : Type ℓ-zero}`. This is supposed by the exploration below which is also to be found in `/home/lan/src/cloned/cb/lan22h-experiments/code-examples/lang/mkn/mkn/ex001_type_defaults_to_type_ell_zero`.

# Journal

2026-08-25 Wk 35 Tue - 09:05 +03:00

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A : Type ℓ₁} {B : Type ℓ₂} -> A -> B -> B
f2 a b = f1 a b
--       ~~~~~~ error
```

```
error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₂
are not equal
when checking that the expression b has type _B_163
```

Even this fails:

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A B : Type ℓ₁} → A → B → B
f2 a b = f1 a b
--       ~~~~~~ error
```

```
Checking cqts-lect1 (/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda).
/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.10-16: error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₁
are not equal
when checking that the expression f1 a b has type B

/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.13-14: error: [UnequalTerms]
The terms
  _A_162 : Type
and
  A : Type ℓ₁
are not equal
when checking that the expression a has type _A_162

/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda:133.15-16: error: [UnequalTerms]
The terms
  _B_163 : Type
and
  B : Type ℓ₁
are not equal
when checking that the expression b has type _B_163
```

```haskell
f1 : {A B : Type} -> A -> B -> B
f1 a b = b

f2 : {ℓ₁ ℓ₂ : Level} {A B : Type ℓ-zero} → A → B → B
f2 a b = f1 a b
```

This succeeds. So we are to interpret `Type` as strictly `Type ℓ-zero`