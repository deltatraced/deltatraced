---
parent: '[[000 Experiment with Rocq]]'
spawned_by: '[[000 Experiment with Rocq]]'
context_type: task
status: done
---

Parent: [000 Experiment with Rocq](../000%20Experiment%20with%20Rocq.md)

Spawned by: [000 Experiment with Rocq](../000%20Experiment%20with%20Rocq.md)

Spawned in: [^spawn-task-808318](../000%20Experiment%20with%20Rocq.md#spawn-task-808318)

# 1 Journal

2026-01-27 Wk 5 Tue - 14:21 +03:00

[ROCQ prover install](https://rocq-prover.org/install#linux-vscode)

[Compiling from sources using scripts v2025.01.0](https://github.com/rocq-prover/platform/blob/2025.01.0/doc/README_Linux.md#installation-by-compiling-from-sources-using-scripts--opam).

Spawn [000 Encountered errors while installing ROCQ on Ubuntu](../entries/000%20Encountered%20errors%20while%20installing%20ROCQ%20on%20Ubuntu.md) ^spawn-entry-1603ff

````sh
sudo apt-get install build-essential

# in /home/lan/Downloads
wget https://github.com/coq/platform/archive/refs/tags/2025.08.0.zip
mkdir rocq-2025.08.0/
mv 2025.08.0.zip rocq-2025.08.0

# in /home/lan/Downloads/rocq-2025.08.0
unzip 2025.08.0.zip

# in /home/lan/Downloads/rocq-2025.08.0/platform-2025.08.0
./coq_platform_make.sh
Install full (f), extended (x), base (b) or IDE (i)? (f/x/b/i/c=cancel) f

(1): Rocq 9.0.1 (released March 2025) with the preview package pick from July 2025
(2): Coq 8.20.1 (released Jan 2025) with the first package pick from Jan 2025
Select package list (number in 1..18, c=cancel) 1

Build opam packages parallel (p) or sequential (s)? (p/s/c=cancel) p
Number of parallel make jobs (number in 1..16, c=cancel) 16
Install non open source SW CompCert (y) or (n)? (y/n/c=cancel) y
Include (i) exclude (e) or select (s) large packages? (i/e/s/c=cancel) i

coq is now pinned to version 9.0.1
rocq-stdlib is now pinned to version 9.0.0
============================== CLOSING REMARKS ===============================
The Coq Platform installation script finished successfully!

A new opam switch with name "CP.2025.08.0~9.0~2025.08" has been created.
You can list all available opam switches with "opam switch".
You can change the default opam switch to the newly created switch with:

    opam switch CP.2025.08.0~9.0~2025.08
    eval $(opam env)
============================== CLOSING REMARKS ===============================
Set the new opam switch as default now (y/n)? (y/n/c=cancel) y
````

````sh
opam switch CP.2025.08.0~9.0~2025.08
eval $(opam env)

rocq --version

# out
The Rocq Prover, version 9.0.1
compiled with OCaml 4.14.2
````

2026-01-29 Wk 5 Thu - 05:57 +03:00

Now to install the language server and vscode extension. [gh rocq-prover/vsrocq](https://github.com/rocq-prover/vsrocq)

````sh
opam switch CP.2025.08.0~9.0~2025.08
opam pin add rocq-core 9.0.1
opam install vsrocq-language-server

The following actions will be performed:
=== remove 1 package
  ⊘ vscoq-language-server  2.2.5 [conflicts with vsrocq-language-server]
=== install 1 package
  ∗ vsrocq-language-server 2.3.4

Proceed with ⊘ 1 removal and ∗ 1 installation? [Y/n] y

<><> Processing actions <><><><><><><><><><><><><><><><><><><><><><><><><><><><>
⊘ removed   vscoq-language-server.2.2.5
⬇ retrieved vsrocq-language-server.2.3.4  (https://opam.ocaml.org/cache)
∗ installed vsrocq-language-server.2.3.4
Done.
# To update the current shell environment, run: eval $(opam env)
````

````sh
opam switch CP.2025.08.0~9.0~2025.08
eval $(opam env)
which vsrocqtop

# out
/home/lan/.opam/CP.2025.08.0~9.0~2025.08/bin/vsrocqtop
````

Now installing the vscode extension `vsrocq` and setting the full path to `vsrocqtop` in its setting as recommended in the [README](https://github.com/rocq-prover/vsrocq).

2026-01-29 Wk 5 Thu - 06:32 +03:00

Spawn [001 Errors encountered during first rocq project build](../entries/001%20Errors%20encountered%20during%20first%20rocq%20project%20build.md) ^spawn-entry-28a456

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/002-rocq-experiments/hello
opam switch CP.2025.08.0~9.0~2025.08
eval $(opam env)

cat <(cat << 'EOF'
-Q . Hello
.
EOF
) > _CoqProject

cat <(cat << 'EOF'
From Stdlib Require Import String.

Definition hello : string := "Hello, Rocq!".

Theorem hello_length :
  String.length hello = 12.
Proof.
  reflexivity.
Qed.

Module M.
  Inductive Nat : Set :=
    | O : Nat
    | S : Nat -> Nat.
End M.

Fixpoint myplus (n m:M.Nat) {struct n} : M.Nat :=
  match n with
  | M.O => m
  | M.S p => M.S (myplus p m)
  end.

Fixpoint nat_to_Nat (n : nat) : M.Nat :=
  match n with
  | O => M.O
  | S n' => M.S (nat_to_Nat n')
  end.

Fixpoint Nat_to_nat (n : M.Nat) : nat :=
  match n with
  | M.O => O
  | M.S n' => S (Nat_to_nat n')
  end.

Eval compute in (Nat_to_nat (myplus (nat_to_Nat 48) (nat_to_Nat 56))).

EOF
) > Hello.v
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/002-rocq-experiments/hello
opam switch CP.2025.08.0~9.0~2025.08
eval $(opam env)
rocq makefile -f _CoqProject -o CoqMakefile
make -f CoqMakefile
````

2026-01-29 Wk 5 Thu - 07:09 +03:00

`Hello.v` is trivial so far. Whether we build or not, vscode intellisense seems to be responsive to `Hello.v` in this case of a single file.
