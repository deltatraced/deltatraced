---
parent: '[[000 Idris2 Website Tutorial]]'
spawned_by: '[[000 Installing Idris2 on ubuntu]]'
context_type: entry
---

Parent: [000 Idris2 Website Tutorial](../000%20Idris2%20Website%20Tutorial.md)

Spawned by: [000 Installing Idris2 on ubuntu](../tasks/000%20Installing%20Idris2%20on%20ubuntu.md)

Spawned in: [^spawn-entry-2ddadc](../tasks/000%20Installing%20Idris2%20on%20ubuntu.md#spawn-entry-2ddadc)

# 1 Journal

2026-01-27 Wk 5 Tue - 14:04 +03:00

````sh
sudo apt-get install chezscheme

git clone git@github.com:idris-lang/Idris2.git

# in /home/lan/src/cloned/gh/idris-lang/Idris2
make bootstrap SCHEME=chezscheme
````

````
make[3]: Entering directory '/home/lan/src/cloned/gh/idris-lang/Idris2/support/refc'
cc -Wall -fPIC -O2   -c -o casts.o casts.c
In file included from cBackend.h:8,
                 from casts.h:3,
                 from casts.c:1:
_datatypes.h:3:10: fatal error: gmp.h: No such file or directory
    3 | #include <gmp.h>
      |          ^~~~~~~
compilation terminated.
````

````sh
sudo apt update
sudo apt install libgmp-dev
````
