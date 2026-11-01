---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Notes for Posts]]'
context_type: task
status: done
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Spawn Logs for Notes for Posts](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md)

Spawned in: [^spawn-task-1f1d99](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md#spawn-task-1f1d99)

# 1 Proof

````haskell
four-nz : 4 ≠ 0
four-nz = (ℕ-ax3 3)
````

# 2 Argument

````haskell
four-nz : 4 ≠ 0
four-nz (h₀ : 4 ≡ 0) = ?0
````

[000 Defn N > ^ax3](../concepts/000%20Defn%20N.md#ax3) Allows us to generate `⊥`:

````haskell
ℕ-ax3 = ∀ (a : ℕ) → suc a ≠ 0
````

````haskell
_ = test-identical-type (ℕ-ax3 3) (4 ≠ 0)
````

````haskell
four-nz : 4 ≠ 0
four-nz (h₀ : 4 ≡ 0) = (ℕ-ax3 3) h₀
````

Or just

````haskell
four-nz : 4 ≠ 0
four-nz = (ℕ-ax3 3)
````

# 3 Related

* [002 Chapter 1 Problems](../entries/002%20Chapter%201%20Problems.md)
