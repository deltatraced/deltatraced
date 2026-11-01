---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/issue/005 Error installing idris2-pack unable to read tree](../issue/005%20Error%20installing%20idris2-pack%20unable%20to%20read%20tree.md)

Spawned in: [^spawn-entry-3a3d26](../issue/005%20Error%20installing%20idris2-pack%20unable%20to%20read%20tree.md#spawn-entry-3a3d26)

# Journal

2026-09-05 Wk 36 Sat - 05:00 +03:00

(quote)

2026-09-05 Wk 36 Sat - 04:51 +03:00

````sh
rm -rf /tmp/ex && mkdir /tmp/ex
git clone --depth=1 https://github.com/stefan-hoeck/idris2-pack-db.git "/tmp/ex/idris2-pack-db"

# in /tmp/ex/idris2-pack-db
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out
fatal: unable to read tree (74b33730b6fea649b15e8d01ab099df9effcc7a0)
````

````sh
rm -rf /tmp/ex && mkdir /tmp/ex
git clone https://github.com/stefan-hoeck/idris2-pack-db.git "/tmp/ex/idris2-pack-db"

# in /tmp/ex/idris2-pack-db
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out
fatal: unable to read tree (74b33730b6fea649b15e8d01ab099df9effcc7a0)
````

Same outcome for `git checkout 15a3e4e70843f7a34100f6470c04b791330788df` [link](https://github.com/idris-lang/Idris2/commit/15a3e4e70843f7a34100f6470c04b791330788df)

Oh oops my bad, that was the `idris2-pack-db`, not the idris2 proper repo.

````sh
rm -rf /tmp/ex && mkdir /tmp/ex
git clone --depth=1 https://github.com/idris-lang/Idris2.git "/tmp/ex/Idris2"

# in /tmp/ex/Idris2
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out
fatal: unable to read tree (74b33730b6fea649b15e8d01ab099df9effcc7a0)
````

(/quote)
