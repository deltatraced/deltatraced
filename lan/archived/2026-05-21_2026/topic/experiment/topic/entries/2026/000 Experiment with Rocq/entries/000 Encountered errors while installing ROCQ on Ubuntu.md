---
parent: '[[000 Experiment with Rocq]]'
spawned_by: '[[000 Install ROCQ on Ubuntu]]'
context_type: entry
---

Parent: [000 Experiment with Rocq](../000%20Experiment%20with%20Rocq.md)

Spawned by: [000 Install ROCQ on Ubuntu](../tasks/000%20Install%20ROCQ%20on%20Ubuntu.md)

Spawned in: [^spawn-entry-1603ff](../tasks/000%20Install%20ROCQ%20on%20Ubuntu.md#spawn-entry-1603ff)

# 1 Journal

2026-01-27 Wk 5 Tue - 14:39 +03:00

````sh
sudo apt-get install build-essential

# in /home/lan/Downloads
wget https://github.com/coq/platform/archive/refs/tags/2025.01.0.zip
mkdir rocq-2025.01.0/
mv 2025.01.0.zip rocq-2025.01.0

# in /home/lan/Downloads/rocq-2025.01.0
unzip 2025.01.0.zip

# in /home/lan/Downloads/rocq-2025.01.0/platform-2025.01.0
./coq_platform_make.sh
Install full (f), extended (x), base (b) or IDE (i)? (f/x/b/i/c=cancel) f
(1): Coq 8.20.1 (released Jan 2025) with the first package pick from Jan 2025
Select package list (number in 1..17, c=cancel) 1
Build opam packages parallel (p) or sequential (s)? (p/s/c=cancel) p
Number of parallel make jobs (number in 1..16, c=cancel) 16
Install non open source SW CompCert (y) or (n)? (y/n/c=cancel) y
Include (i) exclude (e) or select (s) large packages? (i/e/s/c=cancel) i
Apparently this switch already exists. It is recommended to delete the switch,
so that you get a clean and well defined result.
Shall the existing switch be kept (k) or deleted (d) ? (k/d/c=cancel) d
````

````
===== FINAL OPAM SANITY CHECKS =====
[NOTE] These are the repositories in use by the current switch. Use '--all' to see all configured repositories.
[NOTE] These are the repositories in use by the current switch. Use '--all' to see all configured repositories.
[NOTE] These are the repositories in use by the current switch. Use '--all' to see all configured repositories.
[NOTE] These are the repositories in use by the current switch. Use '--all' to see all configured repositories.
===== INSTALL PREREQUISITES (PARALLEL) =====
[ERROR] Package dune-configurator has no version 3.16.1.
````

2026-01-27 Wk 5 Tue - 14:42 +03:00

[\#474 comment](https://github.com/rocq-prover/platform/issues/474#issuecomment-3095520722) mentions that we should try to update the tag, and that no new installer is used for this.

Let's try over `2025.08.0`.

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
````
