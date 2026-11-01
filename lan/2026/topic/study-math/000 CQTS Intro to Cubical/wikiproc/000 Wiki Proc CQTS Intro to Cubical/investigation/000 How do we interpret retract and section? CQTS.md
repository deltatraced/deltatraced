---
context_type: investigation
status: done
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/entry/001 Proc CQTS retract equiv](../entry/001%20Proc%20CQTS%20retract%20equiv.md)

Spawned in: [^spawn-invst-acfdc3](../entry/001%20Proc%20CQTS%20retract%20equiv.md#spawn-invst-acfdc3)

Errata: [003 Errata CQTS How do we interpret retract and section?](../entry/003%20Errata%20CQTS%20How%20do%20we%20interpret%20retract%20and%20section%3F.md)

Overview: [000 Overview Wiki Proc CQTS Intro to Cubical](../entry/000%20Overview%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Iterations: [004 Iterations for Proc CQTS retract equiv](../entry/004%20Iterations%20for%20Proc%20CQTS%20retract%20equiv.md)

# Journal

2026-06-17 Wk 25 Wed - 12:30 +03:00

Spawn [003 Errata CQTS How do we interpret retract and section?](../entry/003%20Errata%20CQTS%20How%20do%20we%20interpret%20retract%20and%20section%3F.md) ^spawn-entry-71d2d9

2026-06-17 Wk 25 Wed - 11:26 +03:00

Some reminders on sections and retracts from the text,
A → B

````haskell
isSection : {A : Type ℓ} {B : Type ℓ'}
  → (f : A → B) 
  → (g : B → A)
  → Type ℓ'
isSection {B = B} f g = (b : B) → f (g b) ≡ b

record SectionOf {A : Type ℓ} {B : Type ℓ'} (f : A → B) : Type (ℓ-max ℓ ℓ') where
  constructor sectionData
  field
    map : B → A
    proof : isSection f map
	
record _RetractOnto_ (A : Type ℓ) (B : Type ℓ') : Type (ℓ-max ℓ ℓ') where
  constructor retractOntoData
  field
    map : A → B
    section : SectionOf map
````

````
This is a common situation, where a function `f : A → B` has a
one-sided inverse `g : B → A` so that `f (g b) ≡ b`. The technical
name for this is that `g` is a *section* of `f`.
````

````
A function is a section if it picks out a small part of `A` (a small
"section" of `A`) that has the shape of `B`.
````

````
Recall that a map *is* a retract when it *has* a section.
````

````haskell
isRetract : {A : Type ℓ} {B : Type ℓ'}
  → (f : A → B) 
  → (g : B → A)
  → Type ℓ
isRetract f g = isSection g f

record RetractOf {A : Type ℓ} {B : Type ℓ'} (f : A → B) : Type (ℓ-max ℓ ℓ') where
  constructor retractData
  field
    map : B → A
    proof : isRetract f map

record isEquiv {A : Type ℓ} {B : Type ℓ'} (f : A → B) : Type (ℓ-max ℓ ℓ') where
  constructor isEquivData
  field
    section : SectionOf f
    retract : RetractOf f

record Equiv (A : Type ℓ) (B : Type ℓ') : Type (ℓ-max ℓ ℓ') where
  constructor equiv
  field
    map : A → B
    proof : isEquiv map
````

````
For this reversed situation, we say that `f : A → B` is a *retract*
when it *has* a section.
````

This is a bit confusing, but `isSection f g` can be interpreted as saying `f has the section g`, where g being a `section`  (of `f`) means that `g` maps the space `B` to a section in the space `A`, so that it can be recovered via `f` (hence being a one-sided inverse).

[errata1](../entry/003%20Errata%20CQTS%20How%20do%20we%20interpret%20retract%20and%20section%3F.md#errata1)

`isRetract f g'` can be interpreted as saying `g' is a retract of f because f is a section of g'.`. Similarly, because we know `isSection f g` (`g is a section of f`), we know that `f` is a retract of `g`: (`isRetract g f`).

So we can refer to `g` as `SectionOf f`. and `g'` as `RetractOf f` :

````mermaid
graph TD

%% Settings
classDef note fill:#f9f9a6,stroke:#333,stroke-width:1px,color:#000,font-style:italic;

%% Nodes
B1[f]
B2[g]
B3[g']

%% Connections
B1 --> |sectionOf| B3
B3 --> |retractOf| B1
B2 --> |sectionOf| B1
B1 --> |retractOf| B2
````

````mermaid
graph TD

%% Nodes
A1[store]
A2[0]
A3[1]
A4[load]
A5[fence]

%% Connections
A1 --> A3
A3 --> A4
A3 --> A5
A4 ==> A5
````

2026-06-17 Wk 25 Wed - 11:57 +03:00

Now let's interpret

````
If `B' retracts onto `A`, then in some sense `A` is a continuous
shrinking of `B`.
````

````haskell
record _RetractOnto_ (A : Type ℓ) (B : Type ℓ') : Type (ℓ-max ℓ ℓ') where
  constructor retractOntoData
  field
    map : A → B
    section : SectionOf map
````

This time `RetractOnto` is a statement about types. If we have a map `A → B` and a section for that map `B → A` that means that we can continuously map the space `B` to a section of the space `A`, which is what the section tells us. So `A RetractOnto B` is true because B sections onto A (there is a section sectioning B in A).

If `A RetractOnto B`, then `B` must  be as large as `A` or smaller, because `B` fits in a section of `A`.
