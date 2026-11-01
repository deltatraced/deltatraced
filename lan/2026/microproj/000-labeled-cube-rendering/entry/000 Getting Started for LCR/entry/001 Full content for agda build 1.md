---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Compile cubical agda to an executable using ctqs sources]]'
context_type: entry
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Compile cubical agda to an executable using ctqs sources](../task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md)

Spawned in: [^spawn-entry-7d7dae](../task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md#spawn-entry-7d7dae)

2026-05-10 Wk 19 Sun - 00:54 +03:00

# 1 Important part

````
[...]
Compiling Relation.Nullary.Reflects in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Relation/Nullary/Reflects.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Relation/Nullary/Reflects.hs
An internal error has occurred. Please report this as a bug.
Location of the error: __IMPOSSIBLE_VERBOSE__, called at src/full/Agda/TypeChecking/Monad/Signature.hs:846:32 in Agda-2.6.4.3-H9LUHq9qpxB9HnxihlaGmY:Agda.TypeChecking.Monad.Signature
````

# 2 Full

````
Checking main (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/main.agda).
Compiling Agda.Primitive in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Primitive.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Primitive.hs
Compiling Agda.Builtin.IO in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/IO.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/IO.hs
Compiling Level in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Level.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Level.hs
Compiling IO.Primitive.Core in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/IO/Primitive/Core.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/IO/Primitive/Core.hs
Compiling Agda.Builtin.Coinduction in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Coinduction.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Coinduction.hs
Compiling Codata.Musical.Notation in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Codata/Musical/Notation.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Codata/Musical/Notation.hs
Compiling Function.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Function/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Function/Base.hs
Compiling Agda.Builtin.Bool in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Bool.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Bool.hs
Compiling Agda.Builtin.Unit in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Unit.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Unit.hs
Compiling Data.Unit.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Unit/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Unit/Base.hs
Compiling Data.Irrelevant in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Irrelevant.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Irrelevant.hs
Compiling Data.Empty in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Empty.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Empty.hs
Compiling Data.Bool.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Bool/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Bool/Base.hs
Compiling Data.Sum.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Sum/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Sum/Base.hs
Compiling Data.Unit.Polymorphic.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Unit/Polymorphic/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Unit/Polymorphic/Base.hs
Compiling Agda.Builtin.Maybe in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Maybe.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Maybe.hs
Compiling IO.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/IO/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/IO/Base.hs
Compiling Agda.Builtin.Equality in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Equality.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Equality.hs
Compiling Relation.Nullary.Negation.Core in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Relation/Nullary/Negation/Core.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Relation/Nullary/Negation/Core.hs
Compiling Agda.Builtin.Sigma in /usr/share/libghc-agda-dev/lib/prim/_build/2.6.4.3/agda/Agda/Builtin/Sigma.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Agda/Builtin/Sigma.hs
Compiling Data.Product.Base in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Data/Product/Base.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Data/Product/Base.hs
Compiling Relation.Nullary.Recomputable in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Relation/Nullary/Recomputable.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Relation/Nullary/Recomputable.hs
Compiling Relation.Nullary.Reflects in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Relation/Nullary/Reflects.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Relation/Nullary/Reflects.hs
An internal error has occurred. Please report this as a bug.
Location of the error: __IMPOSSIBLE_VERBOSE__, called at src/full/Agda/TypeChecking/Monad/Signature.hs:846:32 in Agda-2.6.4.3-H9LUHq9qpxB9HnxihlaGmY:Agda.TypeChecking.Monad.Signature
````
