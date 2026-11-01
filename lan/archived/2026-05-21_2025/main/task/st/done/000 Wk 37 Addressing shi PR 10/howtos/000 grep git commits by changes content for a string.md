---
parent: '[[000 Wk 37 Addressing shi PR 10]]'
spawned_by: '[[000 Look into tarpaulin use]]'
context_type: howto
status: done
---

Parent: [000 Wk 37 Addressing shi PR 10](../000%20Wk%2037%20Addressing%20shi%20PR%2010.md)

Spawned by: [000 Look into tarpaulin use](../investigations/000%20Look%20into%20tarpaulin%20use.md)

Spawned in: [^spawn-howto-0988ee](../investigations/000%20Look%20into%20tarpaulin%20use.md#spawn-howto-0988ee)

# 1 Journal

2025-09-16 Wk 38 Tue - 16:43 +03:00

This [stackoverflow answer](https://stackoverflow.com/a/1340245/6944447) explains:

(1)

That we can grep the commit names themselves with

````sh
git log --grep=word
````

(2)

That we can grep for content with `word` using

````sh
git log -Sword
````

(3)

We can look for differences that add or remove `word` with

````sh
git log -Gword
````

`-G` takes a regex while `-S` a string. `-S` gives us what commits changed the word while `-G` shows the where.

(4)

This shows the changes and not just the commits

````sh
git log -G"something" --patch
````

(5)

since and including {commit}

````sh
git log {commit}^.. -G"something" --patch
````
