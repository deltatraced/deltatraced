---
context_type: task
status: todo
---

Parent: [000 Cubical Analysis I](../000%20Cubical%20Analysis%20I.md)

Spawned by: [005 Cubical Analysis I Thought Stream 26 Wk 43](../entry/005%20Cubical%20Analysis%20I%20Thought%20Stream%2026%20Wk%2043.md)

Spawned in: [^spawn-task-aef0f2](../entry/005%20Cubical%20Analysis%20I%20Thought%20Stream%2026%20Wk%2043.md#spawn-task-aef0f2)

# Journal

2026-10-19 Wk 43 Mon - 15:33 +03:00

There is a simple typo in https://1lab.dev/Cat.Base.html#835 and we have many precedents of typos directly fixed via PR. For example: https://github.com/the1lab/1lab/pull/56:

````
In a precategory, the condition that a≤b be a proposition is relaxed: A precategory has a [`type of objects`](https://1lab.dev/Cat.Base.html#1553) and, between each x,y, a **set** Hom(x,y) of relations (or maps). The name “Hom” is historical and it betrays the original context in which categories where employed: algebra(ic topology), where the maps in question are **hom**omorphisms.
````

should be

````
In a precategory, the condition that a≤b be a proposition is relaxed: A precategory has a [`type of objects`](https://1lab.dev/Cat.Base.html#1553) and, between each x,y, a **set** Hom(x,y) of relations (or maps). The name “Hom” is historical and it betrays the original context in which categories [were] employed: algebra(ic topology), where the maps in question are **hom**omorphisms.
````

--/ 2026-10-19 Wk 43 Mon - 16:40 +03:00

https://1lab.dev/Cat.Diagram.Coproduct.Indexed.html#properties also has a typo:

````
Let X:ΣAB→C be a family of objects in C. If the the indexed coproducts Σa,b:ΣAB​Xa,b​ and Σa:A​Σb:B(a)​Xa,b​ exists, then they are isomorphic.
````

should be

````
Let X:ΣAB→C be a family of objects in C. If ~the~ the indexed coproducts Σa,b:ΣAB​Xa,b​ and Σa:A​Σb:B(a)​Xa,b​ exist~s~, then they are isomorphic.
````

Also this page shows we can define exponentiation directly as superscript possibly.

--/
