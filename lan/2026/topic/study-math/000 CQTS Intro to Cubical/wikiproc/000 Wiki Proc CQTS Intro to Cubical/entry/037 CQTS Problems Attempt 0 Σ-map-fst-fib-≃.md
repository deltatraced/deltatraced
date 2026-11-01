---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-c27550](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-c27550)

# Journal

2026-10-11 Wk 41 Sun - 19:52 +03:00

Partial work dumps are in `/home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/c06dcc-mikan-problem-logs/partial-dumps/cqts/Σ-map-fst-fib-≃.agda`.

This problem looks like it really would benefit from a partial dependant pair composition, where the square only varies in one dimension.

Then we would be able to express

````haskell
--           B (f₁ a)
--             p0  
--      t b̂  — — — >  t̅ b        
--        ^           ^              ^
--   tf-b̂ |           | p3         j |
--        |           |              . — >
--        b̂  — — — >  b̂                i
--              —  
--             B â
-- t b̂ = transport (λ j → B (ap fst p j)) b̂
-- t̅ b = (transport (λ j → B (f₁ a)) b)
````

as

````haskell
--           B (f₁ a)
--             p0  
--      t b̂  — — — >  t̅ b        
--        ^           ^                  ^
--   tf-b̂ |           | tf-b̂ ∙j p0     j |
--        |           |                  . — >
--        b̂  — — — >  b̂                    i
--              —  
--             B â
-- t b̂ = transport (λ j → B (ap fst p j)) b̂
-- t̅ b = (transport (λ j → B (f₁ a)) b)
````

And this would be an instance of a `∙j-filler`.

But I resist making this, because the course promised that we can solve all problems by what we've prior built, and something as significant as a `∙j-filler` I think would be worth a mention as a hint at least.

I'm also calling one of the objects `t̅ b` because that transport is vacuous, the object is already at the `j = i1` end.

I guess as an analogy I've been calling being at the `i1` end here a saturated end. This came to me when we proved we could represent a `PathP` as a `Path` of transports, all held at the sink point of the cubical type instead. We proved working in the full cubical type, and also working in the sinks in that setting, were equivalent, so it seemed like it was a saturated version of that cube, where everything is projected towards the sink. Though this is not standard terminology or something I should use to communicate with others. Maybe better to call it sink-transported? I dunno.

I wanted to try to represent the problem SquareP as a square in a sink type `B (f₁ a)`, but I'm not sure what the representatives for `B â` would be. Or how I would align it such that it's the same square, if accepted by `Path→PathP`, would give me

````haskell
--           B (f₁ a)
--           p0 ∙ p1  
--      t b̂  — — — >  b        
--        ^           ^              ^
--   tf-b̂ |B          | snd p      j |
--        | (p j .fst)|              . — >
--        b̂  — — — >  b̂                 i
--              —  
--             B â
-- t b̂ = tranport (λ j → B (ap fst p j)) b̂
````

And the issue is that it would also seem to just collapse `snd p`, which is a `PathP` extending with `B (p j .fst)`.

2026-10-11 Wk 41 Sun - 20:28 +03:00

The course notes also mention that we could've alternatively used `J` somehow. but trying to turn this `p` of dependant pairs into a `refl` for the `refl-case` seems hard.

2026-10-12 Wk 42 Mon - 01:24 +03:00

````haskell
        fro-to ((a , b) , p) i .fst .snd = transport-fixing (λ j → B (ap fst p (i ∨ j))) (i) (p (i) .snd)
        --fro-to ((a , b) , p) i .fst .snd = (p0 ∙ p1) i
        --where
        --  p0 : Path (B (f₁ a)) (transport (λ j → B (ap fst p j)) b̂)
        --                       (transport (λ j → B (f₁ a)) b)
        --  p0 i = transport (λ j → B (ap fst p (i ∨ j))) ((ap snd p) i)
        --  p1 : Path (B (f₁ a)) (transport (λ j → B (f₁ a)) b) b
        --  p1 = sym (transport-filler (λ j → B (f₁ a)) b)
````

This should be more direct.

2026-10-12 Wk 42 Mon - 01:47 +03:00

Here it is...

````haskell
        fro-to ((a , b) , p) i .snd j .snd = transport-fixing (λ j' → B (ap fst p ((i ∧ j) ∨ (j ∧ j')))) (~ j ∨ i) (p (i ∧ j) .snd)
````

This took me trial and error, but it's important to keep in mind what we wanted to keep constant, which is the `(p j .snd)` to simplify in `(i = i1)` and the constant path at `(j = i0)`, which informs the interval formula `(~ j ∨ i)` After that, two bounds require a `p i` and a `p j` respectively, so our path over `B` had to include both.
