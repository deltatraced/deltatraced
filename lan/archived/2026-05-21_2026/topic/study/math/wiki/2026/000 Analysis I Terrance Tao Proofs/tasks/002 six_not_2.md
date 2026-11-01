---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Notes for Posts]]'
context_type: task
status: done
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Spawn Logs for Notes for Posts](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md)

Spawned in: [^spawn-task-716649](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md#spawn-task-716649)

# 1 Proof

````haskell
six-not-two : 6 ≠ 2
six-not-two (h₀ : 6 ≡ 2) = (p₀ .fst) h₀
  where
    p₀ : (6 ≡ 2) ↔ ⊥
    p₀ = ((    6) ≡ (    2)) ↔⟨ ↔-refl {- unfold 6, 2 -} ⟩ 
         ((suc 5) ≡ (suc 1)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    5) ≡ (    1)) ↔⟨ ↔-refl {- unfold 5, 1 -} ⟩ 
         ((suc 4) ≡ (suc 0)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    4) ≡ (    0)) ↔⟨ ↔-refl {- unfold 4 -} ⟩ 
         ((suc 3) ≡ (    0)) ↔⟨ ↔-⊥ ℕ-ax3 3 ⟩ 
         ⊥                   ∎↔
````

# 2 Argument

We want to observe a contradiction.

````haskell
six-not-two : 6 ≠ 2
six-not-two (h₀ : 6 ≡ 2) = ?1
````

Let's elaborate `6 ≡ 2`

````haskell
_ = test-identical-type (6 ≡ 2) ?0
````

Recall [000 Defn N > ^ax4](../concepts/000%20Defn%20N.md#ax4):

````haskell
ℕ-ax4 = ∀ (a b : ℕ) → suc a ≡ suc b → a ≡ b
````

Then we have the following

````haskell
-- Unfold definitions of 6 and 2
_ = test-identical-type (6 ≡ 2) ((suc 5) ≡ (suc 1))

h₁ : 5 ≡ 1
h₁ = ℕ-ax4 h₀

-- Unfold definitions of 5 and 1
_ = test-identical-type (5 ≡ 1) ((suc 4) ≡ (suc 0))

h₂ : 4 ≡ 0
h₂ = ℕ-ax4 h₁
````

Now recall [000 Defn N > ^ax3](../concepts/000%20Defn%20N.md#ax3):

````haskell
ℕ-ax3 = ∀ (a : ℕ) → suc a ≠ 0
````

With this we can produce our contradiction.

````haskell
contr : ⊥
contr = (ℕ-ax3 3) h₂
````

All in all,

````haskell
six-not-two : 6 ≠ 2
six-not-two (h₀ : 6       ≡ 2) = contr
		     {- : (suc 5) ≡ (suc 1) -- unfold defn -} 
  where
    h₁ : 5       ≡ 1
	{- : (suc 4) ≡ (suc 0) -- unfold defn -}
    h₁ = ℕ-ax4 h₀
    h₂ : 4 ≡ 0
    h₂ = ℕ-ax4 h₁
    contr : ⊥
    contr = (ℕ-ax3 3) h₂
````

We can rewrite this as an explicit chain composition of implications, which should be more in alignment with how we reasoned about this. So observing

````haskell
ℕ-ax4'' = ∀ (a b : ℕ) → suc a ≡ suc b ↔ a ≡ b
````

We have:

````haskell
six-not-two : 6 ≠ 2
six-not-two (h₀ : 6 ≡ 2) = (p₀ .fst) h₀
  where
    p₀ : (6 ≡ 2) ↔ ⊥
    p₀ = ((    6) ≡ (    2)) ↔⟨ ↔-refl {- unfold 6, 2 -} ⟩ 
         ((suc 5) ≡ (suc 1)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    5) ≡ (    1)) ↔⟨ ↔-refl {- unfold 5, 1 -} ⟩ 
         ((suc 4) ≡ (suc 0)) ↔⟨ ℕ-ax4'' ⟩ 
         ((    4) ≡ (    0)) ↔⟨ ↔-refl {- unfold 4 -} ⟩ 
         ((suc 3) ≡ (    0)) ↔⟨ ↔-⊥ ℕ-ax3 3 ⟩ 
         ⊥                   ∎↔
````

Learn more about this syntax in [003 Syntax for dual implication composition](../entries/003%20Syntax%20for%20dual%20implication%20composition.md).
