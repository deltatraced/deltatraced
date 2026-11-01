---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Compile cubical agda to an executable using ctqs sources]]'
context_type: issue
status: done
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Compile cubical agda to an executable using ctqs sources](../task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md)

Spawned in: [^spawn-issue-0fbdb6](../task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md#spawn-issue-0fbdb6)

# 1 Journal

2026-05-10 Wk 19 Sun - 11:51 +03:00

Spawn [002 Updates for Unable to compile hello world with cubical agda flag GHC-49196](../entry/002%20Updates%20for%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md) ^spawn-entry-626ecf

````haskell
-- in ./src/main.agda
open import Agda.Builtin.IO using (IO)
open import Agda.Builtin.Unit using (⊤)
open import Agda.Builtin.String using (String)

postulate putStrLn : String → IO ⊤
{-# FOREIGN GHC import qualified Data.Text as T #-}
{-# COMPILE GHC putStrLn = putStrLn . T.unpack #-}

main : IO ⊤
main = putStrLn "We wanna compile with --cubical!"
````

In `repro.agda-lib`:

````
name: repro
include:
  src
  wip
  _build
flags:
  --guardedness
  --postfix-projections
  --rewriting
  --two-level
  -W noUnsupportedIndexedMatch

  --cubical
````

We need to install the latest version of Agda.

````sh
# in ~/Downloads
wget https://github.com/agda/agda/releases/download/nightly/Agda-92b09bb-linux.tar.xz
````

[How to Extract tar.xz Files in Linux](https://linuxize.com/post/how-to-extract-unzip-tar-xz-file/)

````sh
tar --help | grep one-top-level

# out
      --one-top-level[=DIR]  create a subdirectory to avoid having loose files
````

````sh
# in ~/Downloads
tar -xf ./Agda-92b09bb-linux.tar.xz --one-top-level
````

Update the path I am using in `~/.shellrc.local`:

````diff
-export PATH=$PATH:/home/lan/Downloads/Agda-v2.8.0-linux/
+export PATH=$PATH:/home/lan/Downloads/Agda-92b09bb-linux/
````

````sh
agda --version

# out
Agda version 2.9.0
Built with flags (cabal -f)
 - optimise-heavily: extra optimisations
 - use-xdg-data-home: install and locate data files under $XDG_DATA_HOME/agda/$AGDA_VERSION by default instead of the location defined by Cabal
````

The error we get is different now:

````sh
# in /home/lan/src/tmp/repro
rm -rf src/MAlonzo _build && agda --compile ./src/main.agda

# out
/home/lan/.local/share/agda/2.9.0/lib/prim/Agda/Primitive/Cubical.agda:60.65-66: error: [VariableIsErased]
Variable ℓ is declared erased, so it cannot be used here
when checking that the expression ℓ has type Agda.Primitive.Level
````

2026-05-10 Wk 19 Sun - 23:29 +03:00

So the cubical agda v2.9.0 [library](https://github.com/agda/cubical) only supports up to Agda v2.8.0. We have to revert back to Agda v2.8.0.

````haskell
-- ./src/main.agda
open import Agda.Builtin.IO using (IO)
open import Agda.Builtin.Unit using (⊤)
open import Agda.Builtin.String using (String)

postulate putStrLn : String → IO ⊤
{-# FOREIGN GHC import qualified Data.Text as T #-}
{-# COMPILE GHC putStrLn = putStrLn . T.unpack #-}

main : IO ⊤
main = putStrLn "We wanna compile with --cubical!"
````

`repro.agda-lib`:

````
name: repro
include:
  src
  wip
  _build
flags:
  --guardedness

  --cubical
````

````
# in /home/lan/src/tmp/repro

# while including --cubical in repro.agda-lib:

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

# while removing --cubical in repro.agda-lib:

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
Compiling main in /home/lan/src/tmp/repro/_build/2.8.0/agda/src/main.agdai to /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.hs
Calling: ghc -O -o /home/lan/src/tmp/repro/src/main -Werror -i/home/lan/src/tmp/repro/src -main-is MAlonzo.Code.Qmain /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.hs --make -fwarn-incomplete-patterns
[1 of 6] Compiling MAlonzo.RTE      ( /home/lan/src/tmp/repro/src/MAlonzo/RTE.hs, /home/lan/src/tmp/repro/src/MAlonzo/RTE.o )
[2 of 6] Compiling MAlonzo.Code.Agda.Builtin.Unit ( /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Unit.hs, /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/Unit.o )
[3 of 6] Compiling MAlonzo.Code.Agda.Builtin.String ( /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/String.hs, /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/String.o )
[4 of 6] Compiling MAlonzo.Code.Agda.Builtin.IO ( /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/IO.hs, /home/lan/src/tmp/repro/src/MAlonzo/Code/Agda/Builtin/IO.o )
[5 of 6] Compiling MAlonzo.Code.Qmain ( /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.hs, /home/lan/src/tmp/repro/src/MAlonzo/Code/Qmain.o )
[6 of 6] Linking /home/lan/src/tmp/repro/src/main [Objects changed]
# /out

./src/main

# out
We wanna compile with --cubical!
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

tree -a .

# out
.
├── _build
│   └── 2.8.0
│       └── agda
│           └── src
│               └── main.agdai
├── repro.agda-lib
├── .repro.agda-lib.swp
└── src
    ├── main
    ├── main.agda
    ├── .main.agda.swp
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

11 directories, 18 files
# /out
````

2026-05-10 Wk 19 Sun - 23:55 +03:00

Actually, if you include the `--cubical` flag explicitly in the CLI, it says this:

````sh
# in /home/lan/src/tmp/repro/src
rm -rf src/MAlonzo _build && agda --compile --cubical ./src/main.agda
Checking main (/home/lan/src/tmp/repro/src/main.agda).
error: [CubicalCompilationNotSupported]
Compilation of code that uses --cubical is not supported.
````

I found communication logs around this not being supported pointing to [gh agda/agda #3753](https://github.com/agda/agda/issues/3753) which is still open. Seems it will take research for cubical agda to compile, so this is not feasible at this time.

2026-05-11 Wk 20 Mon - 01:11 +03:00

There is actually an `--erased-cubical` flag.

````sh
rm -rf src/MAlonzo _build && agda --compile --erased-cubical ./src/main.agda
````

This works!

Though the diagnostics could be improved. Ideally we should be pointed to `--erased-cubical`, and the `*.agda-lib` should be warning that `--cubical` with `--compile` is not supported: [000 agda-lib incl cubical should warn](../../../../../../archived/2026-05-21_2026/topic/contribute/open%20source/possibly/gh%20agda%20agda/issues/2026/000%20cubical%20agda%20diagnostics%20agda-lib/000%20agda-lib%20incl%20cubical%20should%20warn.md)
