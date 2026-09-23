---
context_type: investigation
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/task/004 Setup a new code editor for new gentoo](../task/004%20Setup%20a%20new%20code%20editor%20for%20new%20gentoo.md)

Spawned in: [^spawn-invst-cc6f66](../task/004%20Setup%20a%20new%20code%20editor%20for%20new%20gentoo.md#spawn-invst-cc6f66)

# Resolution

````sh
FILES=$(find "$PWD"/my_folder/ -type f) && for file in $FILES; do source $file; done
````

`find` here will recursively get the full path of all files. Then we loop each one and source it.

# Journal

2026-07-28 Wk 31 Tue - 08:51 +03:00

Also, I wanna put my dotfiles in codeberg and immediately configure my system to use the repository files instead of installing-by-copy and maintaining dual copies and ending up being unmotivated to update the dotfiles. It shouldn't be complicated.

For my dotfiles, I really like what portage is doing with things like `/etc/portage/package.use/`. Source each file in alphabetical order for more modular config.

New repo at `/home/lan/src/cloned/cb/lan22h/dotfiles`.

Doing a test for the multiple file sourcing,

````sh
# in lan-proart > /home/lan/src/cloned/cb/lan22h/dotfiles/etc/test
tree -a .

# out
.
├── t1        # Sources from test2 in order
└── test2
    ├── 0     # Prints Hello 0
    └── 1     # Prints Hello 1

2 directories, 3 files
````

https://stackoverflow.com/a/918931 | IFS use

````sh
IFS=' ' read -ra TOKENS <<< "A B C"

echo ${TOKENS[0]} ${TOKENS[1]} ${TOKENS[2]} ${TOKENS[3]} ${TOKENS[-1]} ${TOKENS[-2]} # out {
	A B C C B
# }

echo ${TOKENS[@]} # out {
	A B C
# }

for i in ${TOKENS[@]}; do echo $i; done # out {
	A
	B
	C
# }
````

````
echo ${TOKENS[0]} ${TOKENS[1]} ${TOKENS[2]} ${TOKENS[3]} ${TOKENS[-1]} ${TOKENS[-2]}
     "A"          "B"          "C"          ""           "C"           "B"
````

````sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/test
FILES=$(find "$PWD"/test2/ -type f) && for file in $FILES; do echo $file; done # out {
	/home/lan/src/cloned/cb/lan22h/dotfiles/etc/test/test2/0
	/home/lan/src/cloned/cb/lan22h/dotfiles/etc/test/test2/1
# }

FILES=$(find "$PWD"/test2/ -type f) && for file in $FILES; do source $file; done # out {
	0 Hello
	1 Hello
# }
````

This also works for nested folders. It should suffice for us as we just want to source files in a folder.

````sh
````

2026-07-30 Wk 31 Thu - 07:05 +03:00

Turns out `script_dir=$(dirname "$(readlink -f "$0")")` is not robust against `source`, because `$0` in that case for me is `-bash` and not the script path

https://stackoverflow.com/questions/59895/how-do-i-get-the-directory-where-a-bash-script-is-located-from-within-the-script

Better use `${BASH_SOURCE[0]}`.
