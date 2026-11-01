---
parent: '[[000 mikan-88]]'
spawned_by: '[[000 mikan-88]]'
context_type: task
status: todo
---

Parent: [000 mikan-88](../000%20mikan-88.md)

Spawned by: [000 mikan-88](../000%20mikan-88.md)

Spawned in: [^spawn-task-cdf129](../000%20mikan-88.md#spawn-task-cdf129)

# 1 Journal

2026-05-10 Wk 19 Sun - 10:56 +03:00

https://codeberg.org/1lab/mikan/issues/88

````sh
# in /home/lan/src/cloned/cb/1lab
git clone --recurse-submodules ssh://git@codeberg.org/1lab/mikan.git
````

From `HACKING.md` instructions,

````sh
cabal install --package-env mikan --lib Mikan ieee754
````

2026-05-10 Wk 19 Sun - 11:12 +03:00

Spawn [000 Updates to install mikan and run a hello world](../entries/000%20Updates%20to%20install%20mikan%20and%20run%20a%20hello%20world.md) ^spawn-entry-618136

executable packages installed with `cabal` are in `~/.cabal/bin`. `mikan` is not in `~./.cabal`.

````sh
cabal install --help | grep package-env

# out
 --package-env=ENV              Set the environment file that may be modified.
````

Running the install command again shows us where the environment lives:

````sh
cabal install --package-env mikan --lib Mikan ieee754

# out
Wrote tarball sdist to
/home/lan/src/cloned/cb/1lab/mikan/dist-newstyle/sdist/Mikan-2.9.0.tar.gz
Wrote tarball sdist to
/home/lan/src/cloned/cb/1lab/mikan/dist-newstyle/sdist/mikan-bisect-0.1.tar.gz
Error: [Cabal-7145]
Packages requested to install already exist in environment file at /home/lan/.ghc/x86_64-linux-9.6.7/environments/mikan. Overwriting them may break other packages. Use --force-reinstalls to proceed anyway. Packages: Mikan, ieee754
````

We can use `GHC_ENVIRONMENT=mikan`. Doing `GHC_ENVIRONMENT=mikan cabal install mikan` installed the executable in `Symlinking 'mikan' to '/home/lan/.cabal/bin/mikan'`.

2026-05-10 Wk 19 Sun - 15:09 +03:00

`mikan` does not seem to have a GHC backend. But `agda` does. Was it removed?
