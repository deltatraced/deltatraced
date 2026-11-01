---
parent: '[[000 Getting Started for LCR]]'
spawned_by: '[[000 Getting Started for LCR]]'
context_type: task
status: todo
---

Parent: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned by: [000 Getting Started for LCR](../000%20Getting%20Started%20for%20LCR.md)

Spawned in: [^spawn-task-5385cc](../000%20Getting%20Started%20for%20LCR.md#spawn-task-5385cc)

# 1 Journal

2026-05-09 Wk 19 Sat - 21:17 +03:00

I'm trying to see if I can also include the code I had from the [cqts exercises](https://cqts.github.io/introduction-to-cubical), but running into an error:

````
A .agda-lib file for cqts.Library.Prelude must not be located in
the directory
````

From https://agda.readthedocs.io/en/latest/tools/package-system.html,

 > 
 > Note also that there must not be any `.agda-lib` files below the root, on the path to the Agda file. For instance, if the top-level module in the Agda file is called `A.B.C`, and it is in the directory `root/A/B`, then there must not be any `.agda-lib` files in `root/A` or `root/A/B`.

Moving the `Library` folder outside `cqts` at the same root as `main.agda` helped. Now we can ~~compile~~ pass the issue with a `main.agda` with the contents

````haskell
open import Library.Prelude
````

using

````
agda --compile main.agda
````

````
Duplicate binding for built-in thing TYPE, previous binding to Set
when checking the pragma BUILTIN TYPE Type
````

One issue is that we're trying to build a cubical agda project now.

It might be useful to use the same flags as in 1lab [here](https://github.com/the1lab/1lab/blob/main/1lab.agda-lib). We're adding a `agda.agda-lib`  that has similar content:

````haskell
name: agda
include:
  src
  wip
  _build
flags:
  --cubical
  --no-load-primitives
  --postfix-projections
  --rewriting
  --guardedness
  --two-level
  -W noUnsupportedIndexedMatch
  -W noRewriteVariablesBoundInSingleton
  --experimental-lazy-instances
````

Removing unrecognized flags `--experimental-lazy-instances`, `RewriteVariablesBoundInSingleton.`

Spawn [000 LCR Side Activity](../entry/000%20LCR%20Side%20Activity.md) ^spawn-entry-feb170

2026-05-09 Wk 19 Sat - 22:19 +03:00

new issue:

````
Checking main (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda/src/main.agda).
 Checking Library.Prelude (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda/src/Library/Prelude.lagda.md).
  Checking Library.Primitive (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda/src/Library/Primitive.lagda.md).
/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda/src/Library/Prelude.lagda.md:41,13-56
Could not parse the left-hand side
primHComp {ℓ} {A} {φ} (hcomp-sys.sys _ u) _
Problematic expression: ((hcomp-sys.sys _) u)
Operators used in the grammar:
  None
when scope checking the left-hand side
primHComp {ℓ} {A} {φ} (hcomp-sys.sys _ u) _ in the definition of
primHComp
````

This code is associated with https://github.com/the1lab/1lab/pull/468,

2026-05-10 Wk 19 Sun - 00:45 +03:00

It seems we're unable to get the hello world running now,

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj
agda --compile src/main.agda # has hello world code

# out
Checking main (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/main.agda).
/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/main.agda:17,1-15
Failed to find source of module IO in any of the following
locations:
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/IO.agda
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/IO.lagda
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/wip/IO.agda
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/wip/IO.lagda
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/_build/IO.agda
  /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/_build/IO.lagda
  /usr/share/libghc-agda-dev/lib/prim/IO.agda
  /usr/share/libghc-agda-dev/lib/prim/IO.lagda
when scope checking the declaration
  open import IO
````

Maybe because we need to specify it explicitly in in the `.agda-lib` file. Here is some [documentation](https://agda.readthedocs.io/en/v2.6.0.1/tools/package-system.html) about that file.

2026-05-10 Wk 19 Sun - 00:55 +03:00

Spawn [001 Full content for agda build 1](../entry/001%20Full%20content%20for%20agda%20build%201.md) ^spawn-entry-7d7dae

Adding

````
depend:
  standard-library-2.1
````

to the `.agda-lib` file and then compiling with `agda --compile src/main.agda` resulted in

([full content of log](../entry/001%20Full%20content%20for%20agda%20build%201.md))

````
[...]
Compiling Relation.Nullary.Reflects in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/Relation/Nullary/Reflects.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/agda-pj/src/MAlonzo/Code/Relation/Nullary/Reflects.hs
An internal error has occurred. Please report this as a bug.
Location of the error: __IMPOSSIBLE_VERBOSE__, called at src/full/Agda/TypeChecking/Monad/Signature.hs:846:32 in Agda-2.6.4.3-H9LUHq9qpxB9HnxihlaGmY:Agda.TypeChecking.Monad.Signature
````

~~Even removing all the flags and compiling just the hello world example, this persists, although the log is shorter now, including only the problem bit:~~

No, I ended up saving two `.agda-lib` files accidentally. It does build once we remove all flags. ~~It also builds if we only include `--cubical`~~.

Turns out I had to do this command for a full re-build:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag
rm -rf src/MAlonzo _build && agda --compile ./src/main.agda
````

Adding the flag `--no-load-primitives` leads to the `Relation.Nullary.Reflects` error. Without it, we get:

````
[...]
Compiling IO.Primitive.Infinite in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/IO/Primitive/Infinite.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/IO/Primitive/Infinite.hs
Compiling IO.Infinite in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/IO/Infinite.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/IO/Infinite.hs
Compiling IO in /home/lan/.config/agda/agda-stdlib-2.1/_build/2.6.4.3/agda/src/IO.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/IO.hs
Calling: ghc -O -o /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/main -Werror -i/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src -main-is MAlonzo.Code.Qmain /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/Qmain.hs --make -fwarn-incomplete-patterns
Compilation error:

<no location info>: error: [GHC-49196]
    Can't find /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/Qmain.hs
````

This error persists when using only `--guardedness` and `--cubical`, and is gone when removing `--cubical`.

Also, communication logs show that `--no-load-primitives` should not be used outside 1lab.

There is an Agda wiki: https://wiki.portal.chalmers.se/agda/Main/HomePage

Already the wiki mentions version 2.8.0. Also [cb 1lab/mikan](https://codeberg.org/1lab/mikan/issues) is a fork of verion 2.9.0. Being on version 2.6.4 is too old.

````sh
agda --version

# out
Agda version 2.6.4.3
# /out

whereis agda

# out
agda: /usr/bin/agda
# /out
````

In vscode, we are instead using `/home/lan/Downloads/Agda-v2.8.0-linux/agda`.

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag
rm -rf src/MAlonzo _build && /home/lan/Downloads/Agda-v2.8.0-linux/agda --compile ./src/main.agda

# out
error: [LibraryError]
Library 'standard-library-2.1' not found.
Add the path to its .agda-lib file to
  '/home/lan/.config/agda/libraries'
to install.
Installed libraries:
  (none)
````

Let's remove the other agda.

````sh
sudo apt-get remove agda
sudo apt autoremove
````

Now `agda` no longer works.

Still we need access to the standard library.

````sh
sh -c "$(curl --proto '=https' --tlsv1.2 -s https://raw.githubusercontent.com/agda/agda-stdlib/refs/heads/master/stdlib-install.sh)"
````

This requires `agda` in-path.

````sh
echo "export PATH=\$PATH:/home/lan/Downloads/Agda-v2.8.0-linux/" >> ~/.shellrc.local
````

````sh
agda --version

# out
Agda version 2.8.0
Built with flags (cabal -f)
 - optimise-heavily: extra optimisations
 - use-xdg-data-home: install and locate data files under $XDG_DATA_HOME/agda/$AGDA_VERSION by default instead of the location defined by Cabal
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag
rm -rf src/MAlonzo _build && agda --compile ./src/main.agda
````

Instead of `standard-library-2.1` we now use `standard-library-2.3`.

````
Compiling IO in /home/lan/.config/agda/agda-stdlib-2.3/_build/2.8.0/agda/src/IO.agdai to /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/IO.hs
Calling: ghc -O -o /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/main -Werror -i/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src -main-is MAlonzo.Code.Qmain /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/MAlonzo/Code/Qmain.hs --make -fwarn-incomplete-patterns
ghc: createProcess: posix_spawnp: does not exist (No such file or
directory)
````

[man7 posix_spawnp](https://www.man7.org/linux/man-pages/man3/posix_spawn.3.html),  [manpages.ubuntu posix_spawnp (api)](https://manpages.ubuntu.com/manpages/noble/man3/posix_spawnp.3.html)

````
apt-cache search posix-dev
libfixposix-dev - Replacement for inconsistent parts of POSIX (development)
libghc-regex-posix-dev - GHC library of the POSIX regex backend for regex-base
lua-posix-dev - posix development files for the Lua language
lua-rex-posix-dev - POSIX regex development files for the Lua language
manpages-posix-dev - Manual pages about using a POSIX system for development
````

This might be relevant [gh agda #7655](https://github.com/agda/agda/issues/7655). It is a similar error about `posix_spawnp`.

Installing haskell in case it helps.

````sh
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
````

It says

````
If you are new to Haskell, check out https://www.haskell.org/ghcup/steps/  
````

Now we're back to the error of not finding `Qmain.hs` when using `--cubical`.

There's a hello world example without the standard library [here](https://agda.readthedocs.io/en/latest/getting-started/hello-world.html#hello-world).

The issue with Qmain.hs persists evenby using the built in code directly, and even when not including the standard library.

Not finding anything on Qmain in https://github.com/agda/agda/issues

The error code is `GHC-49196` which should be found in https://errors.haskell.org/ but it is not.

2026-05-10 Wk 19 Sun - 11:39 +03:00

Let's make a small reproducible example of this issue to open an issue.

Spawn [000 Unable to compile hello world with cubical agda flag GHC-49196](../issue/000%20Unable%20to%20compile%20hello%20world%20with%20cubical%20agda%20flag%20GHC-49196.md) ^spawn-issue-0fbdb6

2026-05-11 Wk 20 Mon - 14:33 +03:00

Now we can do the standard library hello world also with `--cubical-erased`. But we still run into issues trying to include CQTS's `Library.Prelude`:

````sh
# in /home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag
rm -rf src/MAlonzo _build && agda --compile ./src/main.agda

# out
Checking main (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/main.agda).
 Checking Library.Prelude (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/Library/Prelude.lagda.md).
  Checking Library.Primitive (/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/Library/Primitive.lagda.md).
/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/Library/Primitive.lagda.md:28.1-48: warning: -W[no]BuiltinDeclaresIdentifier
BUILTIN STRICTSET declares an identifier (no longer expects an
already defined identifier)
when scope checking the declaration
  {-# BUILTIN STRICTSET SSet #-}

/home/lan/src/cloned/gh/LanHikari22/lan-exp-scripts/microproj/2026/000-LabeledCubeRendering/ag/src/Library/Primitive.lagda.md:28.1-48: error: [ClashingDefinition]
Multiple definitions of SSet. Previous definition at
/home/lan/.local/share/agda/2.8.0/lib/prim/Agda/Primitive.agda:15.28-32
when scope checking the declaration
  {-# BUILTIN STRICTSET SSet #-}
````

It seems CTQS duplicates part of `/home/lan/.local/share/agda/2.8.0/lib/prim/Agda/Primitive.agda` to narrate why we need the declerations, so maybe we should disable those duplications.

2026-05-11 Wk 20 Mon - 20:15 +03:00

Let's try instead to not use the cqts reworked primitives and rework our existing work in cqts that we want to bring to this project.

We also want to be able to write this comfortably in vim.

Spawn [001 Enable agda-mode in vim and any other intellisense](001%20Enable%20agda-mode%20in%20vim%20and%20any%20other%20intellisense.md) ^spawn-task-207da8
