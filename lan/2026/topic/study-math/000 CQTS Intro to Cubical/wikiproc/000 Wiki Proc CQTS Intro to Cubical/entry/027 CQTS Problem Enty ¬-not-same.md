---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/005 CQTS Problem Entries](005%20CQTS%20Problem%20Entries.md)

Spawned in: [^spawn-entry-4e4f6a](005%20CQTS%20Problem%20Entries.md#spawn-entry-4e4f6a)

# Journal

2026-09-13 Wk 37 Sun - 17:48 +03:00

In `/home/lan/src/cloned/gh/CQTS/branches/introduction-to-cubical@solve/lectures/1--Type-Theory/1-5--Propositions-as-Types.lagda.md`,

````haskell
    ¬-not-same : ¬ (P iffP (¬ P))
    ¬-not-same (f , g) = f p p
      where
        p : P
        p = g (λ p → f p p)
````

I wasn't sure how to solve this for a bit, but it can be automatically solved in emacs with `C-c C-a`.

I was thinking I wasn't sure where to supply `p` to either `f` or `g`, but I don't have to. `g` takes a *function* so we can build one. That is, assuming `p`, produce a contradiction.
