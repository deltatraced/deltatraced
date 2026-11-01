---
parent: '[[000 mikan-88]]'
spawned_by: '[[000 Install mikan and run a hello world]]'
context_type: entry
---

Parent: [000 mikan-88](../000%20mikan-88.md)

Spawned by: [000 Install mikan and run a hello world](../tasks/000%20Install%20mikan%20and%20run%20a%20hello%20world.md)

Spawned in: [^spawn-entry-618136](../tasks/000%20Install%20mikan%20and%20run%20a%20hello%20world.md#spawn-entry-618136)

# 1 Journal

2026-05-10 Wk 19 Sun - 11:14 +03:00

---

(updating)

2026-05-10 Wk 19 Sun - 10:56 +03:00

https://codeberg.org/1lab/mikan/issues/88

````sh
# in /home/lan/src/cloned/cb/1lab
git clone ssh://git@codeberg.org/1lab/mikan.git
````

From `HACKING.md` instructions,

````sh
cabal install --package-env mikan --lib Mikan ieee754
````

(/updating)

---

Actually,  in `HACKING.md` it is mentioned that we should be cloning the submodules also:

````sh
git clone --recurse-submodules https://codeberg.org/1lab/mikan.git
````

So we should have

````sh
# in /home/lan/src/cloned/cb/1lab
git clone --recurse-submodules ssh://git@codeberg.org/1lab/mikan.git
````
