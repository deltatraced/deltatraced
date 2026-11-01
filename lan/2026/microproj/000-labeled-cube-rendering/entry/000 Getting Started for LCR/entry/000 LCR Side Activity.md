---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Getting Started for LCR]]'
context_type: entry
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned in: [^spawn-entry-feb170](../000%20Getting%20Started%20for%20LCR.md#spawn-entry-feb170)

# 1 Journal

2026-05-09 Wk 19 Sat - 22:04 +03:00

I would like to show the current directory with a tree format. This article (https://www.geeksforgeeks.org/linux-unix/tree-command-unixlinux/) seems to have the right tool.

````sh
sudo apt-get install tree
tree -a .
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda
tree -a . | grep 'agda'

# out
├── agda.agda-lib
│   │   ├── 1-1--Types-and-Functions.lagda.md
│   │   ├── 1-2--Inductive-Types.lagda.md
│   │   ├── 1-3--Universes-and-More-Inductive-Types.lagda.md
│   │   ├── 1-4--Record-Types-and-Copatterns.lagda.md
│   │   └── 1-5--Propositions-as-Types.lagda.md
│   │   ├── 2-1--Paths.lagda.md
│   │   ├── 2-2--Equivalences-and-Path-Algebra.lagda.md
│   │   ├── 2-3--Substitution-and-J.lagda.md
│   │   ├── 2-4--Composition-and-Filling.lagda.md
│   │   ├── 2-5--Transport.lagda.md
│   │   ├── 2-6--Univalence.lagda.md
│   │   ├── 2-7--Propositions.lagda.md
│   │   ├── 2-8--Sets-and-Higher-Types.lagda.md
│   │   └── 2-9--Contractible-Maps.lagda.md
│   │   ├── 3-1--Structure-Identity-Principle.lagda.md
│   │   ├── 3-1--Structure-Identity-Principle-old.lagda.md
│   │   ├── 3-2--Modalities.lagda.md
│   │   ├── 3-3--Constructive-Logic.lagda.md
│   │   └── Lemmas.lagda.md
│   │       └── agda
│   │           │   ├── 1-1--Types-and-Functions.agdai
│   │           │   ├── 1-2--Inductive-Types.agdai
│   │           │   ├── 1-3--Universes-and-More-Inductive-Types.agdai
│   │           │   └── 1-5--Propositions-as-Types.agdai
│   │           │   ├── 2-1--Paths.agdai
│   │           │   ├── 2-2--Equivalences-and-Path-Algebra.agdai
│   │           │   ├── 2-3--Substitution-and-J.agdai
│   │           │   ├── 2-4--Composition-and-Filling.agdai
│   │           │   ├── 2-5--Transport.agdai
│   │           │   └── 2-6--Univalence.agdai
│   │               ├── Prelude.agdai
│   │               ├── Primitive.agdai
│   │               └── Univalence.agdai
│   ├── cqts-course.agda-lib
│   ├── everything.lagda.md
├── hello-world-prog.agda
├── hello-world-prog.agdai
├── .main.agda.swp
    │   ├── Prelude.lagda.md
    │   ├── Primitive.lagda.md
    │   └── Univalence.agda
    └── main.agda
````
