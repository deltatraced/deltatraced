---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [000 How do we interpret retract and section? CQTS](../investigation/000%20How%20do%20we%20interpret%20retract%20and%20section%3F%20CQTS.md)

Spawned in: [^spawn-entry-71d2d9](../investigation/000%20How%20do%20we%20interpret%20retract%20and%20section%3F%20CQTS.md#spawn-entry-71d2d9)

# Journal

2026-06-17 Wk 25 Wed - 12:12 +03:00

````
`isRetract f g'` can be interpreted as saying `f is a retract because g' has it as a section`. So the quote is a bit confusing when it says `f` is a retract because it has a section. `f` having a section `g`, is what `isSection f g` tells us. But `f` is a retract because it *is* a section for some `g'` in `isRetract f g'`. That is, `g'` has a section `f`. This inversion is because `f: A → B` having a section `g : B → A` and *being* a section for a `g' : B → A ` tells us that both spaces `A` and `B` are continuous shrinking of one another; so that they must in fact be equivalent.

But `RetractOf f` and `SectionOf f` makes it seem that we are always talking about another map in relation to `f`, and now the quote makes more sense. For `isRetract f g'` we say that `g'` is a retract (of `f`), because it has a section `f`:
````

This is a misinterpretation, `isRetract f g'` means that f is a section of `g'`, and hence has the retract g'. Similarly, `isSection g' f` means that `g'` has a section `f`. I interpreted `isRetract f g'` as `f` being the retract, but it is `g'`. The correction of this mistake explains how we can refer to `g'` via `RetractOf f`.

^errata1
