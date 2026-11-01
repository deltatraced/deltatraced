---
context_type: task
status: done
---

Parent: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical](../000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical.md)

Spawned by: [lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/task/000 Reproduce how far we got in cqts but in problems-mkn module format](000%20Reproduce%20how%20far%20we%20got%20in%20cqts%20but%20in%20problems-mkn%20module%20format.md)

Spawned in: [^spawn-task-ae85d1](000%20Reproduce%20how%20far%20we%20got%20in%20cqts%20but%20in%20problems-mkn%20module%20format.md#spawn-task-ae85d1)

# What?

Here we're interested in establishing an organization of many agda projects under a single repository `problems-mkn`. Many mathematical problems are very interconnected, so it can be
insightful to have each problem or problem subdomain depend on an exact set of dependencies and no more than what is required to solve it.

We investigate setting this up through the git root of the `problems-mkn` repository so that anyone could easily run any project within it.

# Journal

2026-08-24 Wk 35 Mon - 03:20 +03:00

We need a project where we can host many of these problems.  Creating `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn` for mikan and cubical agda problems. The idea is that the repository host multiple projects in a flat hierarchy that can reference one another.

Also to review, we should gather the context from CQTS first. Let's put the code under `/home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/lecture1-type-theory`.

I want all projects in `problems-mkn` to be able to reference one another, from the the `git root`.

* https://agda.readthedocs.io/en/latest/tools/command-line-options.html#imports-and-libraries
* $\to$ https://agda.readthedocs.io/en/latest/tools/package-system.html#package-system

2026-08-24 Wk 35 Mon - 02:47 +03:00

It seems we should use `include` in `.agda-lib` to define where the module we're loading is supposed to be found relative to the top level, so like if we have a `src/` folder, we'd put `src`. If it's directly in the library top level, we can just put `.`.

Library paths are supposed to be defined in a `libraries` file in envvar `AGDA_DIR`. Luckily, paths there are able to expand environmental variables, so we can do

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1
GITROOT=$(git root) AGDA_DIR=$(pwd) mikan src/cqts-lect1.agda
````

Note that we cannot name those some standard name like `src/lib.agda` because then agda has issues resolving the different paths:

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/.agda-lib { 
	name:
	    cqts-lect1
	depend:
	    cqts-bg
	include:
	    src
	flags:
# }

# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/libraries {
	$GIT_ROOT/proj/gh/cqts/introduction-to-cubical/cqts-bg/.agda-lib
# }

# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-bg/.agda-lib {
	name: cqts-bg
	depend:
	include:
	    src
	flags:
# }

# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1
GIT_ROOT=$(git root) AGDA_DIR=$(pwd) mikan src/lib.agda

# out
1.1-4: error: [AmbiguousTopLevelModuleName]
Ambiguous module name. The module name lib could refer to any of
the following files:
  /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-bg/src/lib.agda
  /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/lib.agda
````

But this is no issue if we instead call them `cqts-bg/src/cqts-bg.agda` and `cqts-lect1/src/cqts-lect1.agda`.

Now we are able to have many modules.

````haskell
-- in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn/proj/gh/cqts/introduction-to-cubical/cqts-lect1/src/cqts-lect1.agda
open import cqts-bg using (Bool)

ident-bool : Bool → Bool
ident-bool = λ x → x
````

2026-08-24 Wk 35 Mon - 03:07 +03:00

One current cost of this solution is that we have to prefix every `vi`, `emacs`, `mikan` and `agda` command with `GIT_ROOT=$(git root) AGDA_DIR=$(pwd)`. Could we improve on this?

I at least added an `.env.sh`. So one can just do `source .env.sh` once when they enter a project directory.

2026-08-24 Wk 35 Mon - 03:23 +03:00

OK

````sh
# in /home/lan/src/idea/cb/lan22h-experiments/problems-mkn
git commit # out (relevant) { [main f038feb] created two agda modules with dependency based on git root }
````
