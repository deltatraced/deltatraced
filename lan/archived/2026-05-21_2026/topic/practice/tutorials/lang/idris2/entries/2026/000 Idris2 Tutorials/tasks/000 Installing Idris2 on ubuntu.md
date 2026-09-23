---
parent: '[[000 Idris2 Website Tutorial]]'
spawned_by: '[[000 Idris2 Website Tutorial]]'
context_type: task
status: wontdo
---

Parent: [000 Idris2 Website Tutorial](../000%20Idris2%20Website%20Tutorial.md)

Spawned by: [000 Idris2 Website Tutorial](../000%20Idris2%20Website%20Tutorial.md)

Spawned in: [^spawn-task-e8e455](../000%20Idris2%20Website%20Tutorial.md#spawn-task-e8e455)

# 1 Journal

2026-01-27 Wk 5 Tue - 09:55 +03:00

[Installation page](https://idris2.readthedocs.io/en/latest/tutorial/starting.html).

Spawn [000 Errors encountered during Idris2 installation on Ubuntu](../entries/000%20Errors%20encountered%20during%20Idris2%20installation%20on%20Ubuntu.md) ^spawn-entry-2ddadc

````sh
sudo apt-get install chezscheme

sudo apt update
sudo apt install libgmp-dev

git clone git@github.com:idris-lang/Idris2.git

# in /home/lan/src/cloned/gh/idris-lang/Idris2
make bootstrap SCHEME=chezscheme
make install
````

This should install to `~/.idris2`

````sh
~/.idris2/bin/idris2

# out
     ____    __     _         ___
    /  _/___/ /____(_)____   |__ \
    / // __  / ___/ / ___/   __/ /     Version 0.8.0-2be43760d
  _/ // /_/ / /  / (__  )   / __/      https://www.idris-lang.org
 /___/\__,_/_/  /_/____/   /____/      Type :? for help

Welcome to Idris 2.  Enjoy yourself!
Main>
````

2026-01-28 Wk 5 Wed - 06:56 +03:00

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/001-idris2-experiments/tut0/hello
cat <(cat << 'EOF'
module Main

main : IO ()
main = putStrLn "Hello world"
EOF
) > hello.idr
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/001-idris2-experiments/tut0/hello
~/.idris2/bin/idris2 hello.idr -o hello
./build/exec/hello

# out
Hello world
````

````sh
echo "export PATH=\$PATH:/home/lan/.idris2/bin" >> ~/.shellrc.local
````

````sh
# in /home/lan/src/cloned/gh/LanHikari22/archived/lan-exp-scripts/files/2026/persistent/001-idris2-experiments/tut0/hello
idris2 hello.idr

Main> :t main
Main.main : IO ()
Main> :q
Bye for now!
````

2026-01-28 Wk 5 Wed - 07:27 +03:00

Also install the vscode extensions [bamboo.idris2-lsp](https://marketplace.visualstudio.com/items?itemName=bamboo.idris2-lsp) and

We will also need [gh idris-community/idris2-lsp](https://github.com/idris-community/idris2-lsp) which requires [gh stefan-hoeck/idris2-pack](https://github.com/stefan-hoeck/idris2-pack).

````sh
bash -c "$(curl -fsSL https://raw.githubusercontent.com/stefan-hoeck/idris2-pack/main/install.bash)"
pack install-app idris2-lsp
````

2026-01-28 Wk 5 Wed - 08:49 +03:00

We can create `hello` instead as a project:

````sh
pack new lib hello
````

This allows `idris2-lsp` to analyze it. We can build with `pack build`.

Though we also wanna execute, so use `app` instead of `lib`:

````sh
pack new app hello
````

Now we can use `pack build` and `pack run`.

2026-09-07 Wk 37 Mon - 10:23 +03:00

status $\to$ wontdo, we're now in gentoo: [002 Quick new Installs for Gentoo System > Idris 2](../../../../../../../../../../../2026/main/task/001%20Install%20a%20new%20Gentoo%20system/entry/002%20Quick%20new%20Installs%20for%20Gentoo%20System.md#idris-2)
