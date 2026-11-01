---
parent: '[[000 Wk 37 Addressing shi PR 10]]'
spawned_by: '[[000 Wk 37 Addressing shi PR 10]]'
context_type: investigation
status: done
---

Parent: [000 Wk 37 Addressing shi PR 10](../000%20Wk%2037%20Addressing%20shi%20PR%2010.md)

Spawned by: [000 Wk 37 Addressing shi PR 10](../000%20Wk%2037%20Addressing%20shi%20PR%2010.md)

Spawned in: [^spawn-invst-dfab20](../000%20Wk%2037%20Addressing%20shi%20PR%2010.md#spawn-invst-dfab20)

# 1 Journal

2025-09-16 Wk 38 Tue - 16:26 +03:00

````sh
# in /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
rg 'tarpaulin'  

# out 
Cargo.toml
29:unexpected_cfgs = { level = "warn", check-cfg = ['cfg(tarpaulin_include)'] }

src/command/help.rs
179:        #[cfg(not(tarpaulin_include))]
184:        #[cfg(not(tarpaulin_include))]

src/command_set.rs
158:        #[cfg(not(tarpaulin_include))]
163:        #[cfg(not(tarpaulin_include))]

src/parser.rs
328:        #[cfg(not(tarpaulin_include))]
368:        #[cfg(not(tarpaulin_include))]
````

It might help recalling some of the git investigative commands we used when debugging changes to dbml py:

In `Invst what happened to table.refs for pydbml`,

Some commands we used:

````sh
# in what commit was it changed
git log --all -- pydbml/classes/table.py
# tracks changes across files
git log --follow --patch pydbml/_classes/table.py

git log -L '/the line from your file/,+1:path/to/your/file.txt'

git log -L 15,+1:'path/to/your/file.txt'

# diff between two tags
git diff tag1 tag2

# the commits between tags
git log tag1..tag2

# files changed between tags
git diff tag1 tag2 --stat

# diffing on a specific file
git diff tag1 tag2 -- some/file/name
````

2025-09-16 Wk 38 Tue - 16:41 +03:00

But currently I want to grep commits by changes content for a string.

Spawn [000 grep git commits by changes content for a string](../howtos/000%20grep%20git%20commits%20by%20changes%20content%20for%20a%20string.md) ^spawn-howto-0988ee

2025-09-16 Wk 38 Tue - 16:50 +03:00

````sh
# /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
git log --grep=tarpaulin

# out 
[nothing]
````

No commits explicitly mention it in message.

````sh
# /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
git log -Gtarpaulin

# out 
commit 32d2944dc4954f8aa587e5058c844e9dde6567a9
Author: Mohammed Alzakariya <lanhikarixx@gmail.com>
Date:   Fri Aug 29 11:27:12 2025 +0300

    small modifications to pass all clippy lints

commit 3a17c539113b0c7584d487615e78c75cb8f9e027
Author: may h <mehrabhoque@gmail.com>
Date:   Thu Apr 15 17:47:10 2021 -0400

    Add tests for help and include the leaf arguments

commit ecb258b7d3b0919faef34166201288165fb315e5
Author: may h <mehrabhoque@gmail.com>
Date:   Sat Jan 2 17:46:26 2021 -0500

    Add tests for the command set
````

````sh
# /home/lan/src/cloned/gh/LanHikari22/forked/Utagai/shi
git log -Gtarpaulin --patch
````

2025-09-16 Wk 38 Tue - 18:18 +03:00

There's not much said about it in git history. Let's just remove it.
