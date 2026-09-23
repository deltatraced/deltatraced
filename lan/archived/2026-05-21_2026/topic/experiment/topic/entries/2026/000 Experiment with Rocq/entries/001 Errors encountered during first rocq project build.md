---
parent: '[[000 Experiment with Rocq]]'
spawned_by: '[[000 Install ROCQ on Ubuntu]]'
context_type: entry
---

Parent: [000 Experiment with Rocq](../000%20Experiment%20with%20Rocq.md)

Spawned by: [000 Install ROCQ on Ubuntu](../tasks/000%20Install%20ROCQ%20on%20Ubuntu.md)

Spawned in: [^spawn-entry-28a456](../tasks/000%20Install%20ROCQ%20on%20Ubuntu.md#spawn-entry-28a456)

# 1 Journal

2026-01-29 Wk 5 Thu - 06:47 +03:00

Now we need to build a simple project to see how this fits together. There's documentation (although likely old for coq) in [Setup for working on your own projects](https://rocq-prover.org/doc/V8.18.0/refman/practical-tools/utilities.html#setup-for-working-on-your-own-projects).

We expect a folder with a `_CoqProject` file in it. You can see they have an example `-Q . Lib` in [Q option docs](https://rocq-prover.org/doc/V8.18.0/refman/practical-tools/coq-commands.html#q-option)

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
From Rocq Require Import Stdlib.
Definition hello : string := "Hello, Rocq!".
Theorem hello_length :
  String.length hello = 12.
Proof.
  reflexivity.
Qed.
EOF
) > Hello.v
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/002-rocq-experiments/hello
coq_makefile -f _CoqProject -o CoqMakefile
make -f CoqMakefile install
````

````
ROCQ DEP VFILES
Warning: in file Hello.v, library Stdlib is required
         from root Rocq and has not been found in the loadpath!
         [module-not-found,filesystem,default]
Hello.vo does not exist
Hello.glob does not exist
make: *** [CoqMakefile:586: install] Error 1
````

2026-01-29 Wk 5 Thu - 06:49 +03:00

This should be the updated ROCQ docs: [rocq docs v9.1.0](https://rocq-prover.org/doc/V9.1.0/refman/index.html)

Here is the updated [Setup for working on your own projects](https://rocq-prover.org/doc/V9.1.0/refman/practical-tools/utilities.html#setup-for-working-on-your-own-projects).

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
From Rocq Require Import Stdlib.

Definition hello : string := "Hello, Rocq!".
Theorem hello_length :
  String.length hello = 12.
Proof.
  reflexivity.
Qed.
EOF
) > Hello.v
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/002-rocq-experiments/hello
rocq makefile -f _CoqProject -o CoqMakefile
make -f CoqMakefile
````

````
ROCQ DEP VFILES
Warning: in file Hello.v, library Stdlib is required
         from root Rocq and has not been found in the loadpath!
         [module-not-found,filesystem,default]
ROCQ compile Hello.v
File "./Hello.v", line 3, characters 25-31:
Error: Cannot find a physical path bound to logical path
Stdlib with prefix Rocq.

make[1]: *** [CoqMakefile:818: Hello.vo] Error 1
make[1]: *** [Hello.vo] Deleting file 'Hello.glob'
make: *** [CoqMakefile:416: all] Error 2
````

2026-01-29 Wk 5 Thu - 06:58 +03:00

Removing the Stdlib import:

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
Definition hello : string := "Hello, Rocq!".

Theorem hello_length :
  String.length hello = 12.
Proof.
  reflexivity.
Qed.
EOF
) > Hello.v
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/002-rocq-experiments/hello
rocq makefile -f _CoqProject -o CoqMakefile
make -f CoqMakefile
````

````
ROCQ DEP VFILES
ROCQ compile Hello.v
File "./Hello.v", line 4, characters 19-25:
Error: The reference string was not found in the current environment.

make[1]: *** [CoqMakefile:818: Hello.vo] Error 1
make[1]: *** [Hello.vo] Deleting file 'Hello.glob'
make: *** [CoqMakefile:416: all] Error 2
````
