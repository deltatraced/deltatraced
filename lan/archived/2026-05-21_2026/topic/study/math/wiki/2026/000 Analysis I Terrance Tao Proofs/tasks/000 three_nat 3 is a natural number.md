---
parent: '[[000 Analysis I Terrance Tao Proofs]]'
spawned_by: '[[000 Spawn Logs for Notes for Posts]]'
context_type: task
status: done
---

Parent: [000 Analysis I Terrance Tao Proofs](../000%20Analysis%20I%20Terrance%20Tao%20Proofs.md)

Spawned by: [000 Spawn Logs for Notes for Posts](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md)

Spawned in: [^spawn-task-461290](../../../../../../write/wiki/2026/000%20Notes%20for%20Posts/entries/000%20Spawn%20Logs%20for%20Notes%20for%20Posts.md#spawn-task-461290)

---

# 1 Proof

````haskell
three-nat : ∃ (a : ℕ) → a ≡ 3
three-nat .fst = suc (suc (suc 0))
three-nat .snd = refl
	where
		refl : suc (suc (suc 0) ≡ (suc (suc (suc 0)))
````

# 2 Argument

Here we are expressing the set-theoretic style proof in the textbook.

````haskell
three-nat : ∃ (a : ℕ) → a ≡ 3
````

First, expand the definition of `3` as noted in [000 Defn N > ^notation1](../concepts/000%20Defn%20N.md#notation1):

````haskell
three-nat : ∃ (a : ℕ) → a ≡ (suc (suc (suc 0)))
````

We are trying to prove a (constructive) existential proposition, and we do this by providing a pair `(a, p)` where `a` is the element of choice, and `p` is the proof that it satisfies the condition `three-nat`.

````haskell
three-nat = (?0 , ?1)
````

For hole `?1`, we expect a reflexivity proof on `?0` as a proof term, so we have:

````haskell
three-nat = (?0 , refl)
	where
		refl : ?0 ≡ (suc (suc (suc 0)))
````

Let's focus on finding the term in the hole `?0` by splitting this proof into cases:

````haskell
three-nat .fst = ?0
three-nat .snd = refl
	where
		refl : ?0 ≡ (suc (suc (suc 0)))
````

By utilizing [000 Defn N > ^ax1](../concepts/000%20Defn%20N.md#ax1), we can partially fill the hole `?0`.

````haskell
three-nat .fst = {! ?2 0 0!}
````

By [000 Defn N > ^ax2](../concepts/000%20Defn%20N.md#ax2) We know there exists an element `suc 0`:

````haskell
three-nat .fst = {! ?2 (suc 0) 0!}
````

And for that element `a`, there also exist an element `suc a`:

````haskell
three-nat .fst = {! ?2 (suc (suc 0)) 0!}
````

Then the hole `?2` is completely filled by `suc`, completing `?0`:

````haskell
three-nat .fst = suc (suc (suc 0))
````

Now we can finish `three-nat .snd` filling in the same `?0`:

````haskell
three-nat .fst = suc (suc (suc 0))
three-nat .snd = refl
	where
		refl : suc (suc (suc 0)) ≡ (suc (suc (suc 0)))
````

Since both sides of `refl` are the same, we know it is a correct typing judgment, and this concludes the proof.

In the end, we have

````haskell
three-nat .fst = suc (suc (suc 0))
three-nat .snd = refl
	where
		refl : suc (suc (suc 0)) ≡ (suc (suc (suc 0)))
````

# 3 Related

* [002 Chapter 1 Problems](../entries/002%20Chapter%201%20Problems.md)
