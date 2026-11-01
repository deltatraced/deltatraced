---
context_type: entry
---

Parent: [[000 Cubical Analysis I]]

Spawned by: [[002 Cubical Analysis I Problem Attempt Entries]]

Spawned in: [[002 Cubical Analysis I Problem Attempt Entries#^spawn-entry-16b623|^spawn-entry-16b623]]

# Journal

2026-10-18 Wk 42 Sun - 10:27 +03:00

```haskell
ℕ-strong-ind :
    (m₀ : ℕ)
  → (f : (m : ℕ) → (m₀ ≤ℕ m) → ((m' : ℕ) → (m₀ ≤ℕ₃ m' <ℕ₃ m) → A m') → A m)
  → ((m : ℕ) → (m₀ ≤ℕ m) → A m)
```

Partial attempt snapshots at `/home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/c06dcc-mikan-problem-logs/partial-dumps/terrance-tao/ℕ-strong-ind.agda`.

Previously partially solved in RocQ in `gh/LanHikari22/lan-exp-scripts/2026/files/moved_out/003-analysis-I-terece-tao/analysis_coq/Nat.v` for the case `m₀ = zero`.

2026-10-18 Wk 42 Sun - 10:32 +03:00

This RocQ part:

```rocq
    {
      intros m H1.
      apply ge_implies_eq_or_gt in H1.
      destruct H1 as [H1l | H1r].
      {
        assert (H2 : forall k : nat, S n' > k -> p k).
        {
          intros k H2.
          apply gt_as_ge_succ in H2.
          apply ge_is_succ_invariant in H2.
          specialize (IHn' k H2).
          exact IHn'.
        }
        specialize (H0 (S n') H2).
        rewrite -> H1l in H0.
        exact H0.
      }
      {
        apply gt_succ_as_ge in H1r.
        specialize (IHn' m H1r).
        exact IHn'.
      }
    }
```

seems to correspond to this in agda:

```haskell
        f'' (suc m) m' m'≤sm = {!!}
          where
            h0 : (m' ≡ (suc m)) → A m'
            h0 m'≡sm = subst (λ s → A s) (sym m'≡sm) (f (suc m) (≤ℕ-zero≤ (suc m)) f''') 
              where
                f''' : (m'' : ℕ) → zero ≤ℕ₃ m'' <ℕ₃ (suc m) → A m''
                f''' m'' (0≤m'' , m''<sm) = f'' m'' m'' (≤ℕ-refl m'')
            h1 : (m' <ℕ (suc m)) → A m'
            h1 m'<sm = {!f'' m m'!}
              where
                m'<m : m' ≤ℕ m
                m'<m =
                  (λ s → s)
                    subst⟨ {!!} ⟩
                  ((suc m') ≤ℕ (suc m))
                    subst-refl
                  (λ s → s)
                    subst⟨ {!!} ⟩
                  (m' <ℕ (suc m))
                    ∎subst m'<sm
```

But `f'' m'' m''` is problematic. There is no such prior case! Agda will complain about this use. The prior case is only `f'' m m'`. 

But then how were we able to do `specialize (IHn' k H2)` in RocQ? Specializing for `k` is the same as calling `f'' m''`, which again here we shouldn't be able to do!

```haskell
        f'' (suc m) m' m'≤sm = h2 (Dec≡-ℕ m' (suc m))
		                --     ~~
          where
            h0 : (m' ≡ (suc m)) → A m'
            h0 m'≡sm = subst (λ s → A s) (sym m'≡sm) (f (suc m) (≤ℕ-zero≤ (suc m)) f''') 
																			--     ~~~~
              where
                f''' : (m'' : ℕ) → zero ≤ℕ₃ m'' <ℕ₃ (suc m) → A m''
                f''' m'' (0≤m'' , m''<sm) = f'' m'' m'' (≤ℕ-refl m'')
				                        --  ~~~
            h1 : (m' <ℕ (suc m)) → A m'
			h1 = {- ... -}
            h2 : (Dec (m' ≡ suc m)) → A m'
            h2 (yes m'≡sm) = h0 m'≡sm
            h2 (no m'≠sm) = h1 (m'≤sm , m'≠sm)
```

```
/home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/idea/proj/book/terence-tao/analysis-I/tt-analysis-I/src/tt-analysis-I-index.agda:859.5-896.43: error: [TerminationIssue]
Termination checking failed for the following functions:
Problematic calls:
 tt-analysis-I-index.h2 (Dec≡-ℕ m' (suc m))
    (at /home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/idea/proj/book/terence-tao/analysis-I/tt-analysis-I/src/tt-analysis-I-index.agda:867.32-34)
  tt-analysis-I-index.f'''
    (at /home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/idea/proj/book/terence-tao/analysis-I/tt-analysis-I/src/tt-analysis-I-index.agda:870.84-88)
  f'' m'' m'' (≤ℕ-refl m'')
    (at /home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/idea/proj/book/terence-tao/analysis-I/tt-analysis-I/src/tt-analysis-I-index.agda:873.45-48)
  tt-analysis-I-index.h0 m'≡sm
    (at /home/lan/src/cloned/cb/lan22h-experiments/problems-mkn/idea/proj/book/terence-tao/analysis-I/tt-analysis-I/src/tt-analysis-I-index.agda:888.30-32)
```

The case for h1, where we do `f'' m m'` is fine though, but `f'' m'' m''` in h0 is problematic.

2026-10-18 Wk 42 Sun - 12:28 +03:00

```haskell
        f'' (suc m) m' m'≤sm = h2 (Dec≡-ℕ m' (suc m))
          where
            h0 : (m' ≡ (suc m)) → A m'
            h0 m'≡sm = subst (λ s → A s) (sym m'≡sm) (f (suc m)
                             (≤ℕ-zero≤ (suc m)) f''') 
              where
                f''' : (m'' : ℕ) → zero ≤ℕ₃ m'' <ℕ₃ (suc m) → A m''
                f''' m'' (0≤m'' , m''<sm) = f'' m m'' m''≤m
                  where
                    m''≤m : m'' ≤ℕ m
                    m''≤m = ≤ℕ≃<ℕ-suc-r m'' m .proof .section .map m''<sm
            h1 : (m' <ℕ (suc m)) → A m'
            h1 m'<sm = f'' m m' m'≤m
              where
                m'≤m : m' ≤ℕ m
                m'≤m = ≤ℕ≃<ℕ-suc-r m' m .proof .section .map m'<sm
            h2 : (Dec (m' ≡ suc m)) → A m'
            h2 (yes m'≡sm) = h0 m'≡sm
            h2 (no m'≠sm) = h1 (m'≤sm , m'≠sm)
```

This resolves that issue by instead using `m` for both.

2026-10-19 Wk 43 Mon - 10:33 +03:00

The `ℕ-strong-ind (suc m₀)` case was almost identical to `ℕ-strong-ind zero`, it's just that this solves differently because now we are carrying an impossible condition:

```haskell
f₁ : (m m' : ℕ) → suc m₀ ≤ℕ₃ m' ≤ℕ₃ m → A m'
f₁ zero m' (sm₀≤m' , m'≤0) = ∅-rec (≤ℕ-suc≰zero m₀ sm₀≤0)
```

The strategy has been mostly to solving range conditions in the inductive hypothesis for the map `f'` we have to build to `f`, and sometimes using `f` to solve it for an even lesser `m''` than `m'`. 

We also weaken `<ℕ₃` into `≤ℕ₃` to be able to pass through the induction, which gives us less obligation to answer to. Once we established the weaker and more general map, we can trivially strengthen it back.

