---
context_type: issue
status: done
---

Parent: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned by: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical]]

Spawned in: [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/000 Wiki Proc CQTS Intro to Cubical#^spawn-issue-373c33|^spawn-issue-373c33]]

# Reproduction

```haskell
-- in ~/a.agda
open import Agda.Primitive using (
    LevelUniv; 
    Level) renaming (
    lzero to ℓ-zero;
    lsuc to ℓ-suc;
    _⊔_ to ℓ-max)

open import Agda.Primitive.Cubical using (
    I; 
    i0; 
    i1; 
    Partial) renaming (
        primIMin to infixr 20 _∧_;
        primIMax to infixr 20 _∨_;
        primINeg to infix 30 ~_)

∂ : I → I
∂ i = i ∨ (~ i)

-- Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
Repro i j k (i = i0) = {!!}
```

`Repro` gives us an error:

```sh
mikan ~/a.agda

# out
Checking a (/home/lan/a.agda).
/home/lan/a.agda:22.1-28: error: [UnequalTerms]
The terms
  ~ i ∨ (j ∨ ~ j) ∨ ~ k
and
  ~ i
are not equal at type I
when checking the definition of Repro
```

# Resolution

We explored a simple case and found the same error:

```haskell
-- in ~/a.agda
data Bool : Type where
  true  : Bool
  false : Bool

true-false-Partial : (i : I) → Partial (i ∨ ~ i) Bool
--true-false-Partial i (i = i0) = true
--true-false-Partial i (i = i1) = false
true-false-Partial i (i = i0) = {!!}
```

```
/home/lan/a.agda:31.1-37: error: [UnequalTerms]
The terms
  i ∨ ~ i
and
  ~ i
are not equal at type I
when checking the definition of true-false-Partial
```

Turns out the real problem is that this won't type check unless you pattern mach all the points covered by the formula.

So this works:

```haskell
-- in ~/a.agda
true-false-Partial : (i : I) → Partial (i ∨ ~ i) Bool
--true-false-Partial i (i = i0) = true
--true-false-Partial i (i = i1) = false
true-false-Partial i (i = i0) = {!!}
true-false-Partial i (i = i1) = {!!}
```

For `Repro`, this resolves the error:

```haskell
-- in ~/a.agda
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
Repro i j k (i = i0) = {!!}
Repro i j k (j = i0) = {!!}
Repro i j k (j = i1) = {!!}
Repro i j k (k = i0) = {!!}
```

Find this at `/home/lan/src/cloned/cb/lan22h-experiments/code-examples/lang/mkn/mkn/ex000_partial_fn_definition_cases_must_be_complete`

∎
# Journal

2026-06-22 Wk 26 Mon - 11:25 +03:00

Basic example:

```haskell
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro i j k (i = i0) = {!!}
```

I thought this could not be reproduced in other files like 2-4 (I found it fails in 2-7) but it can, so not sure what happened there.

In 2-4 this works:

```haskell
∙∙-filler-tube : {w x y z : A}
    (r : w ≡ x) (p : x ≡ y) (q : y ≡ z)
  → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
∙∙-filler-tube r p q i j k (j = i0) = Sq0 i k
```

Let's decouple the basic example from our shared context so that we can directly check it with `agda`:

```haskell
-- in ~/tmp/del/a.agda
import Agda.Primitive.Cubical

LevelUniv = Agda.Primitive.LevelUniv
Level = Agda.Primitive.Level
ℓ-zero = Agda.Primitive.lzero
ℓ-suc = Agda.Primitive.lsuc
ℓ-max = Agda.Primitive._⊔_

I = Agda.Primitive.Cubical.I
i0 = Agda.Primitive.Cubical.i0
i1 = Agda.Primitive.Cubical.i1

Partial = Agda.Primitive.Cubical.Partial

_∧_ : I → I → I
_∧_ = Agda.Primitive.Cubical.primIMin

_∨_ : I → I → I
_∨_ = Agda.Primitive.Cubical.primIMax

~_ : I → I
~_ = Agda.Primitive.Cubical.primINeg

infix  30 ~_
infixr 20 _∧_ _∨_

∂ : I → I
∂ i = i ∨ ~ i

Repro : {ℓ : Level} → {A : Set ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro i j k (i = i0) = {!!}
```

```sh
# in /home/lan/tmp/del
agda --cubical a.agda

# out
Checking a (/home/lan/tmp/del/a.agda).
/home/lan/tmp/del/a.agda:31.1-28: error: [UnequalTerms]
Agda.Primitive.Cubical.primIMax (Agda.Primitive.Cubical.primINeg i)
(Agda.Primitive.Cubical.primIMax
 (Agda.Primitive.Cubical.primIMax j
  (Agda.Primitive.Cubical.primINeg j))
 (Agda.Primitive.Cubical.primINeg k))
!= Agda.Primitive.Cubical.primINeg i of type
Agda.Primitive.Cubical.I
when checking the definition of Repro
```

1. https://agda.readthedocs.io/en/v2.6.0.1/getting-started/what-is-agda.html
2. https://github.com/agda/agda

It's likely built like mikan:

```sh
cabal install -foptimise-heavily exe:agda
```

2026-06-23 Wk 26 Tue - 10:40 +03:00

We can see this is the case when we do `agda --version` and it mentions that it uses the flags `optimnise-heavily`. Though it also uses `use-xdg-data-home`.

That was `Agda version 2.8.0`. mikan, and also `Agda version 2.9.0`, instead give this error:

```sh
# in /home/lan/tmp/del
agda --cubical a.agda

# out
error: [LibraryError]
Library 'standard-library' not found.
Add the path to its .agda-lib file to
  '/home/lan/.config/agda/libraries'
to install.
Installed libraries:
  (none)
```

```sh
# in /home/lan/tmp/del
mikan a.agda

# out
error: [LibraryError]
Library 'standard-library' not found.
Add the path to its .agda-lib file to
  '/home/lan/.config/agda/libraries'
to install.
Installed libraries:
  (none)
```

We have `~/.config/agda/agda-stdlib-2.3`,`~/.config/agda/agda-stdlib-2.1`, `~/.local/share/agda/2.8.0/lib`,

```sh
# in /home/lan/tmp/del
mv ~/.config/agda/agda-stdlib-2.1 ~/.config/agda/agda-stdlib-2.1-unused
mv ~/.config/agda/agda-stdlib-2.3 ~/.config/agda/agda-stdlib-2.3-unused
~/Downloads/Agda-v2.8.0-linux/agda --cubical a.agda

# out
error: [LibraryError]
/home/lan/.config/agda/libraries-2.8.0:1:
Failed to read library file /home/lan/.config/agda/agda-stdlib-2.3/standard-library.agda-lib.
Reason: /home/lan/.config/agda/agda-stdlib-2.3/standard-library.agda-lib: openBinaryFile: does not exist (No such file or directory)
```

So it uses `~/.config/agda/agda-stdlib-2.3`.

In the source, this error is mentioned in `test/Fail/Installed.err`

1. https://agda.readthedocs.io/en/latest/getting-started/installation.html
2. https://github.com/agda/agda-stdlib
3. https://wiki.portal.chalmers.se/agda/pmwiki.php?n=Libraries.StandardLibrary

```sh
sh -c "$(curl --proto '=https' --tlsv1.2 -s https://raw.githubusercontent.com/agda/agda-stdlib/refs/heads/master/stdlib-install.sh)"
```

This installs an `agda-stdlib-experimental` rather than `3.1` which `~/Downloads/Agda-v2.8.0-linux/agda` looks for. This can be found directly in https://github.com/agda/agda-stdlib/releases/tag/v2.3.

```sh
cd ~/Downloads
wget https://github.com/agda/agda-stdlib/archive/refs/tags/v2.3.tar.gz
tar -xvf v2.3.tar.gz
rm v2.3.tar.gz
mv agda-stdlib-2.3 ~/.config/agda/
```

```sh
# in /home/lan/tmp/del
agda --cubical a.agda

# out
Checking a (/home/lan/tmp/del/a.agda).
/home/lan/tmp/del/a.agda:31.1-28: error: [UnequalTerms]
The terms
  Agda.Primitive.Cubical.primIMax (Agda.Primitive.Cubical.primINeg i)
  (Agda.Primitive.Cubical.primIMax
   (Agda.Primitive.Cubical.primIMax j
    (Agda.Primitive.Cubical.primINeg j))
   (Agda.Primitive.Cubical.primINeg k))
and
  Agda.Primitive.Cubical.primINeg i
are not equal at type Agda.Primitive.Cubical.I
when checking the definition of Repro
```

```sh
# in /home/lan/tmp/del
mikan a.agda
Checking a (/home/lan/tmp/del/a.agda).
/home/lan/tmp/del/a.agda:30.28-31: warning: -W[no]UserWarning
DEPRECATED: Use Type instead of Set
when scope checking Set

/home/lan/tmp/del/a.agda:31.1-28: error: [UnequalTerms]
The terms
  Agda.Primitive.Cubical.primIMax (Agda.Primitive.Cubical.primINeg i)
  (Agda.Primitive.Cubical.primIMax
   (Agda.Primitive.Cubical.primIMax j
    (Agda.Primitive.Cubical.primINeg j))
   (Agda.Primitive.Cubical.primINeg k))
and
  Agda.Primitive.Cubical.primINeg i
are not equal at type Agda.Primitive.Cubical.I
when checking the definition of Repro
```

Hmm. Which library do they use?

They're using `agda-stdlib-experimental`, while `~/Downloads/Agda-v2.8.0-linux/agda` uses `agda-stdlib-2.3`. 

```sh
# in /home/lan/tmp/del
mikan a.agda

# out
error: [LibraryError]
/home/lan/.config/agda/libraries-2.9.0:1:
Failed to read library file /home/lan/.config/agda/agda-stdlib-experimental/standard-library.agda-lib.
Reason: /home/lan/.config/agda/agda-stdlib-experimental/standard-library.agda-lib: openBinaryFile: does not exist (No such file or directory)

# in /home/lan/tmp/del
agda --cubical a.agda

# out
error: [LibraryError]
/home/lan/.config/agda/libraries-2.9.0:1:
Failed to read library file /home/lan/.config/agda/agda-stdlib-experimental/standard-library.agda-lib.
Reason: /home/lan/.config/agda/agda-stdlib-experimental/standard-library.agda-lib: openBinaryFile: does not exist (No such file or directory)
```

But interestingly now they're complaining about the exact path once I temporarily renamed the experimental lib. And the reason is because it was registered:

```sh
cat ~/.config/agda/libraries-2.9.0

# out
/home/lan/.config/agda/agda-stdlib-experimental/standard-library.agda-lib
```

```sh
# in /home/lan/tmp/del
mikan a.agda

# out
Checking a (/home/lan/tmp/del/a.agda).
/home/lan/tmp/del/a.agda:30.28-31: warning: -W[no]UserWarning
DEPRECATED: Use Type instead of Set
when scope checking Set

/home/lan/tmp/del/a.agda:31.1-28: error: [UnequalTerms]
The terms
  Agda.Primitive.Cubical.primIMax (Agda.Primitive.Cubical.primINeg i)
  (Agda.Primitive.Cubical.primIMax
   (Agda.Primitive.Cubical.primIMax j
    (Agda.Primitive.Cubical.primINeg j))
   (Agda.Primitive.Cubical.primINeg k))
and
  Agda.Primitive.Cubical.primINeg i
are not equal at type Agda.Primitive.Cubical.I
when checking the definition of Repro
```

So they both give this error now, bar the deprecated message for Set to Type.

2026-06-23 Wk 26 Tue - 12:28 +03:00

What does this error trace to?

Agda

1. `src/full/Agda/Interaction/Options/Errors.hs > data ErrorName::UnequalTerms_`
2. `src/full/Agda/TypeChecking/Errors/Names.hs > fn typeErrorName`
	- note
		```haskell
		  -- in here:
		  ConversionError_ ConversionError{convErrTys = cmp} -> case cmp of
		    FailAsTypes{}   -> UnequalTypes_
		    FailAsTermsOf{} -> UnequalTerms_
		```
3. `src/full/Agda/TypeChecking/Errors/Names.hs > fn typeErrorString`
4. `src/full/Agda/TypeChecking/Errors.hs > fn <TCErr as impl PrettyTCM>::prettyTCM`
	- in case
		- `TypeError loc s e`
	- trace to main

Mikan

2026-06-23 Wk 26 Tue - 13:02 +03:00

Where is the haskell specification? Needed to interpret the syntax in Mikan and Agda.

- Haskell
	1. https://www.haskell.org/TCErr
		- note
			1. https://www.haskell.org/documentation/
			2. Links to a free course https://www.engineering.upenn.edu/~cis1940/spring13/lectures.html
		- source
			1. website managed in https://github.com/haskell-infra/www.haskell.org/
	2. https://www.haskell.org/onlinereport/haskell2010/
	3. https://www.haskell.org/ghc/
		- extends haskell 2010: https://downloads.haskell.org/ghc/latest/docs/users_guide/exts.html

```sh
ghc --version

# out
The Glorious Glasgow Haskell Compilation System, version 9.10.3
```

```sh
whereis ghc

# out
ghc: /home/lan/.ghcup/bin/ghc
```

2026-06-23 Wk 26 Tue - 14:02 +03:00

Spawn [[lan/2026/topic/study-math/000 CQTS Intro to Cubical/wikiproc/000 Wiki Proc CQTS Intro to Cubical/investigation/001 What is the haskell instance x where syntax?]] ^spawn-invst-6f5d8f

2026-06-23 Wk 26 Tue - 23:27 +03:00

`src/full/Agda/TypeChecking/Errors.hs > fn <TCErr as impl PrettyTCM>::prettyTCM`

There are many callers for this. It's hard to trace further. Instead of searching `UnequalTerms`, let's search for the form of this string:

```
/home/lan/tmp/del/a.agda:31.1-28: error: [UnequalTerms]
```

`{path}:{line_ctx}: error: [{errorname}]`

2026-06-23 Wk 26 Tue - 23:31 +03:00

Spawn [[002 Agda What code prints the error message of the compiler? 7273757e5e]] ^spawn-invst-7bbbd7

2026-06-24 Wk 26 Wed - 12:51 +03:00

1. https://wiki.haskell.org/Debugging
2. https://wiki.haskell.org/index.php?title=GHC/GHCi_debugger

2026-07-17 Wk 29 Fri - 14:05 +03:00

Added to [[002 Inbox]]

2026-08-22 Wk 34 Sat - 04:08 +03:00

Getting back to this. I am on a new gentoo system now so I need to reproduce the issue. And let's see if we can just fix it simply to bypass the need to reverse engineer compiler code and continue with [[000 Wiki CQTS Intro to Cubical]].

[[002 Quick new Installs for Gentoo System#1lab/Mikan]]

2026-08-22 Wk 34 Sat - 19:23 +03:00

Precedence changes the error.

```haskell
-- Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
```

```sh
mikan ~/a.agda

# out (error)
Checking a (/home/lan/a.agda).
/home/lan/a.agda:28.7-8: error: [NotInScope]
Not in scope:
  i at /home/lan/a.agda:28.7-8
    (did you mean
       'Agda.Primitive.Cubical.I' or
       'I'?)
when scope checking i
```

Is `I = Agda.Primitive.Cubical.I` insufficient?

https://agda.readthedocs.io/en/latest/getting-started/a-taste-of-agda.html

Apparently mikan looks for `.mkn` files too now: 

```
where .AGDA denotes a legal extension for an Agda file
(i.e., one of .mkn .agda .lagda .lagda.rst .lagda.tex .lagda.md
```

https://agda.readthedocs.io/en/latest/language/module-system.html

2026-08-22 Wk 34 Sat - 19:46 +03:00

Changing the script to use imports

```haskell
-- in ~/a.agda
open import Agda.Primitive using (
    LevelUniv; 
    Level) renaming (
    lzero to ℓ-zero;
    lsuc to ℓ-suc;
    _⊔_ to ℓ-max)

open import Agda.Primitive.Cubical using (
    I; 
    i0; 
    i1; 
    Partial) renaming (
        primIMin to infixr 20 _∧_;
        primIMax to infixr 20 _∨_;
        primINeg to infix 30 ~_)

∂ : I → I
∂ i = i ∨ (~ i)

-- Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial (~ i ∨ ∂ j ∨ ~ k) A
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
Repro i j k (i = i0) = {!!}
```

```sh
mikan ~/a.agda

# out
Checking a (/home/lan/a.agda).
/home/lan/a.agda:22.1-28: error: [UnequalTerms]
The terms
  ~ i ∨ (j ∨ ~ j) ∨ ~ k
and
  ~ i
are not equal at type I
when checking the definition of Repro
```

So not precedence now.

2026-08-23 Wk 34 Sun - 11:11 +03:00

We have agda-mode for emacs setup for mikan now. So we can `emacs ~/a.agda` and do the usual actions we're used to in vscode before, like `C-c C-l` for agda load.

We can do `Repro = {!!}` and do `C-c C-r` to fill in introduction forms, and `C-c C-.` to see the context at a hole.

Many other commands at https://agda.readthedocs.io/en/latest/tools/emacs-mode.html

2026-08-23 Wk 34 Sun - 18:39 +03:00

Okay we get a similar error here:

```haskell
-- in ~/a.agda
data Bool : Type where
  true  : Bool
  false : Bool

true-false-Partial : (i : I) → Partial (i ∨ ~ i) Bool
--true-false-Partial i (i = i0) = true
--true-false-Partial i (i = i1) = false
true-false-Partial i (i = i0) = {!!}
```

```
/home/lan/a.agda:31.1-37: error: [UnequalTerms]
The terms
  i ∨ ~ i
and
  ~ i
are not equal at type I
when checking the definition of true-false-Partial
```

So it seems the reason is because we have to pattern match the formula to cover both `(i = i0)` and `(i = i1)` with the holes, or it complains because the formula does not match the signature.

This is valid:

```haskell
-- in ~/a.agda
true-false-Partial : (i : I) → Partial (i ∨ ~ i) Bool
--true-false-Partial i (i = i0) = true
--true-false-Partial i (i = i1) = false
true-false-Partial i (i = i0) = {!!}
true-false-Partial i (i = i1) = {!!}
```

So this works:

```haskell
-- in ~/a.agda
Repro : {ℓ : Level} → {A : Type ℓ} → (i j k : I) → Partial ((~ i) ∨ ∂ j ∨ (~ k)) A
Repro i j k (i = i0) = {!!}
Repro i j k (j = i0) = {!!}
Repro i j k (j = i1) = {!!}
Repro i j k (k = i0) = {!!}
```

This resolves the problem!