---
context_type: issue
status: todo
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-issue-1fedcc|^spawn-issue-1fedcc]]

# Journal

2026-09-05 Wk 36 Sat - 04:59 +03:00

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/013 Errata Error Installing idris2-pack unable to read tree]] ^spawn-entry-3a3d26

2026-09-04 Wk 36 Fri - 11:10 +03:00

After installing ChezScheme

```sh
chezscheme --version

# out
10.4.1
```

and running

```sh
bash -c "$(curl -fsSL https://raw.githubusercontent.com/stefan-hoeck/idris2-pack/main/install.bash)"
```

we get

```
Resolving deltas: 100% (248/248), done.
+ pushd /home/lan/.cache/pack/clones/Idris2
~/.cache/pack/clones/Idris2 ~/src/cloned/gh
+ git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0
fatal: unable to read tree (74b33730b6fea649b15e8d01ab099df9effcc7a0)
```

In a new terminal session,

```sh
rm -rf ~/.local/state/pack/
rm -rf ~/.cache/pack
bash -c "$(curl -fsSL https://raw.githubusercontent.com/stefan-hoeck/idris2-pack/main/install.bash)"
```

yields the same issue.

2026-09-04 Wk 36 Fri - 11:18 +03:00

```sh
# in /home/lan/.cache/pack/clones/Idris2
git pull origin 74b33730b6fea649b15e8d01ab099df9effcc7a0
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0
```

This works so what's the problem?

Just doing `git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0` would not.

2026-09-04 Wk 36 Fri - 12:26 +03:00

https://github.com/idris-lang/Idris2/issues?q=is%3Aissue%20unable%20to%20read%20tree yields nothing.

- https://github.com/idris-lang/Idris2
	- .$\to$ https://github.com/idris-lang/Idris2/blob/main/INSTALL.md
	- .$\to$ https://github.com/stefan-hoeck/idris2-pack
		- $\to$ https://github.com/stefan-hoeck/idris2-pack/blob/main/install.bash

```sh
# in https://github.com/stefan-hoeck/idris2-pack/blob/main/install.bash (relevant) {
	STATE_HOME="${XDG_STATE_HOME:-$HOME/.local/state}"
	STATE_DIR="${PACK_STATE_DIR:-$STATE_HOME/pack}"
	DB_DIR="$STATE_DIR/db"
	CLONES_DIR="$CACHE_DIR/clones"
	
	git clone --depth=1 https://github.com/stefan-hoeck/idris2-pack-db.git "$CLONES_DIR/idris2-pack-db"
	cp "$CLONES_DIR/idris2-pack-db/collections/"* "$DB_DIR"
	
	LATEST_DB="$(find "$DB_DIR" -name 'nightly-*' | sort | tail -1)"
	PACKAGE_COLLECTION="$(basename -s .toml "$LATEST_DB")"
	IDRIS2_COMMIT=$(sed -ne '/^\[idris2\]/,/^commit/{/^commit/s/commit *= *"\([a-f0-9]*\)"/\1/p;}' "$DB_DIR/$PACKAGE_COLLECTION.toml")
	
	git clone --depth=1 https://github.com/idris-lang/Idris2.git "$CLONES_DIR/Idris2"
	pushd "$CLONES_DIR/Idris2"
	git checkout "$IDRIS2_COMMIT"
# }
```

2026-09-05 Wk 36 Sat - 04:11 +03:00

```sh
ls ~/.local/state/pack/db/nightly-2* | tail -1

# out
/home/lan/.local/state/pack/db/nightly-260903.toml
```

```sh
# in /home/lan/.local/state/pack/db/nightly-260903.toml {
	[idris2]
	url     = "https://github.com/idris-lang/Idris2"
	version = "0.8.0"
	commit  = "74b33730b6fea649b15e8d01ab099df9effcc7a0"
# }
```

https://git-scm.dev/docs/git-checkout

2026-09-05 Wk 36 Sat - 04:51 +03:00

```sh
rm -rf /tmp/ex && mkdir /tmp/ex && cd /tmp/ex
git clone --depth=1 https://github.com/idris-lang/Idris2.git "/tmp/ex/Idris2"

# in /tmp/ex/Idris2
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out
fatal: unable to read tree (74b33730b6fea649b15e8d01ab099df9effcc7a0)
```

Same outcome for `git checkout 15a3e4e70843f7a34100f6470c04b791330788df` [link](https://github.com/idris-lang/Idris2/commit/15a3e4e70843f7a34100f6470c04b791330788df)

```sh
rm -rf /tmp/ex && mkdir /tmp/ex && cd /tmp/ex
git clone --depth=1 https://github.com/idris-lang/Idris2.git "/tmp/ex/Idris2"

# in /tmp/ex/Idris2
git pull origin 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out {
	hint: You have divergent branches and need to specify how to reconcile them.
	hint: You can do so by running one of the following commands sometime before
	hint: your next pull:
	hint:
	hint:   git config pull.rebase false  # merge
	hint:   git config pull.rebase true   # rebase
	hint:   git config pull.ff only       # fast-forward only
	hint:
	hint: You can replace "git config" with "git config --global" to set a default
	hint: preference for all repositories. You can also pass --rebase, --no-rebase,
	hint: or --ff-only on the command line to override the configured default per
	hint: invocation.
	fatal: Need to specify how to reconcile divergent branches.
# }

# in /tmp/ex/Idris2
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out {
	Note: switching to '74b33730b6fea649b15e8d01ab099df9effcc7a0'.
	
	You are in 'detached HEAD' state. You can look around, make experimental
	changes and commit them, and you can discard any commits you make in this
	state without impacting any branches by switching back to a branch.
	
	If you want to create a new branch to retain commits you create, you may
	do so (now or later) by using -c with the switch command. Example:
	
	  git switch -c <new-branch-name>
	
	Or undo this operation with:
	
	  git switch -
	
	Turn off this advice by setting config variable advice.detachedHead to false
	
	HEAD is now at 74b33730 allow manual triggering of build workflows (#3853)
# }
```

Now `git checkout 15a3e4e70843f7a34100f6470c04b791330788df` works.

```sh
rm -rf /tmp/ex && mkdir /tmp/ex && cd /tmp/ex
git clone https://github.com/idris-lang/Idris2.git "/tmp/ex/Idris2"

# in /tmp/ex/Idris2
git checkout 74b33730b6fea649b15e8d01ab099df9effcc7a0

# out (relevant)
HEAD is now at 74b33730b allow manual triggering of build workflows (#3853)
```

This also works.
