---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Unable to compile hello world with cubical agda flag GHC-49196]]'
context_type: entry
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Unable to compile hello world with cubical agda flag GHC-49196](../issue/000%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md)

Spawned in: [^spawn-entry-626ecf](../issue/000%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md#spawn-entry-626ecf)

# 1 Journal

2026-05-10 Wk 19 Sun - 11:52 +03:00

---

(updating)

````
# in /home/lan/src/tmp/repro
tree -a .

# out
.
├── _build
│   └── 2.8.0
│       └── agda
│           └── src
│               └── main.agdai
├── repro.agda-lib
└── src
    ├── main.agda
    └── MAlonzo
        ├── Code
        │   └── Agda
        │       ├── Builtin
        │       │   ├── Bool.hs
        │       │   ├── Char.hs
        │       │   ├── IO.hs
        │       │   ├── List.hs
        │       │   ├── Maybe.hs
        │       │   ├── Nat.hs
        │       │   ├── Sigma.hs
        │       │   ├── String.hs
        │       │   └── Unit.hs
        │       └── Primitive.hs
        ├── RTE
        │   └── Float.hs
        └── RTE.hs
# /out

agda --version

# out
Agda version 2.8.0
Built with flags (cabal -f)
 - optimise-heavily: extra optimisations
 - use-xdg-data-home: install and locate data files under $XDG_DATA_HOME/agda/$AGDA_VERSION by default instead of the location defined by Cabal
# /out

rm -rf src/MAlonzo _build && agda --compile ./src/main.agda

# out
Checking main (/home/lan/src/tmp/repro/src/main.agda).
Compiling Agda.Primitive in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Primitive.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Primitive.hs
Compiling Agda.Builtin.Unit in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Unit.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Unit.hs
Compiling Agda.Builtin.IO in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/IO.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/IO.hs
Compiling Agda.Builtin.Bool in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Bool.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Bool.hs
Compiling Agda.Builtin.Nat in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Nat.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Nat.hs
Compiling Agda.Builtin.Char in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Char.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Char.hs
Compiling Agda.Builtin.List in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/List.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/List.hs
Compiling Agda.Builtin.Maybe in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Maybe.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Maybe.hs
Compiling Agda.Builtin.Sigma in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/Sigma.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Sigma.hs
Compiling Agda.Builtin.String in /home/lan/.local/share/agda/2.8.0/lib/prim/_build/2.8.0/agda/Agda/Builtin/String.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/String.hs
Calling: ghc -O -o /home/lan/src/tmp/repro/src/main -Werror -i/home/lan/src/tmp/repro/src -main-is MAlonzo.Code.Qmain /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.hs --make -fwarn-incomplete-patterns
error: [CompilationError]
Compilation error:

<no location info>: error: [GHC-49196]
    Can't find /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.hs
# /out

lsb_release -a

# out
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 25.04
Release:        25.04
Codename:       plucky
# /out

ghc --version

# out
The Glorious Glasgow Haskell Compilation System, version 9.6.7
# /out


````

(/updating)

---

The issue template asks that we reproduce this with the latest version of Agda.
