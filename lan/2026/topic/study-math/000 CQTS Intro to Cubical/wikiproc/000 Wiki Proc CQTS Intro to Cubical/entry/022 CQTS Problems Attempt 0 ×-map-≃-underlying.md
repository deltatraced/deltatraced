---
context_type: entry
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/009 CQTS Problems Attempt Entries](009%20CQTS%20Problems%20Attempt%20Entries.md)

Spawned in: [^spawn-entry-0bfb7d](009%20CQTS%20Problems%20Attempt%20Entries.md#spawn-entry-0bfb7d)

# Journal

2026-09-25 Wk 39 Fri - 13:02 +03:00

In `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect2/src/lect-2-6-univalence.agda`,

````haskell
-- Hint: Use ua-comp on both sides to clear the transports
×-map-≃-underlying :
    (ea : A̬ ≃ Â)
  → (eb : B̬ ≃ B̂)
  → (×-map-≃-ua ea eb) .map ≡ ×-map (ea .map) (eb .map)
×-map-≃-underlying ea eb i (a̬ , b̬) .fst = ua-comp ea a̬ i
×-map-≃-underlying ea eb i (a̬ , b̬) .snd = ua-comp eb b̬ i
````

Just the correct pattern matching causes this to be solved very simply.

For some reason my previous attempt I didn't unpack it?

* [001 Elaboration Notes for x-map-eqv-underlying - inefficient](../../../../../../../archived/2026-05-21_2026/topic/practice/tutorials/lang/cubical-agda/wiki/2026/000%20introduction-to-cubical%20archive/entries/001%20Elaboration%20Notes%20for%20x-map-eqv-underlying%20-%20inefficient.md)
* [000 Proving x map equiv underlying - inefficient](../../../../../../../archived/2026-05-21_2026/topic/practice/tutorials/lang/cubical-agda/entries/2026/000%20introduction-to-cubical/tasks/000%20Proving%20x%20map%20equiv%20underlying%20-%20inefficient.md)
