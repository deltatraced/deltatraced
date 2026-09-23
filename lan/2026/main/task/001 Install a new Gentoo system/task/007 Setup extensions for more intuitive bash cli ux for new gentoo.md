---
context_type: task
status: todo
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-339ff8](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-339ff8)

# Journal

2026-07-29 Wk 31 Wed - 19:17 +03:00

I considered to have a `history` that checks the return code, so it can be filtered for only successfully run commands. I did run into situations before where the entries in history fill up with my invalid command attempts, which seemed annoying.

this is the history fuzzy search I used before:

````sh
# fhp - fuzzy search history and optionally put the selection in tmux copy buffer
fhp() {
  comm="$( ([ -n "$ZSH_NAME" ] && fc -l 1 || history) | fzf +s --tac | sed -E 's/ *[0-9]*\*? *//' | sed -E 's/\\/\\\\/g')"
  echo $comm
  if [[ $FHP_COPY_TO_TMUX_BUFFER != 0 ]]; then
    echo $comm | tr --d '\n' | tmux loadb -
  fi
}
````

We can keep using this. Knowing a command's error code requires that it is recorded after the fact, and for default history behavior to be overridden. I'm not sure if it is worth it.

2026-07-29 Wk 31 Wed - 19:21 +03:00

For `fkill`, I had this in my old ubuntu system so it should be easy to bring over. Here are some things from there:

````sh
alias tmain='tmux attach -t main || tmux new -s main'
alias ipext='curl ipinfo.io/ip'
````

Renaming from `tma` to `tmain`, which is short for **t**mux **main** so it's easier to remember.

````sh
# fkill - kill processes - list only the ones you can kill.
fkill() {
    local pid 
    if [ "$UID" != "0" ]; then
        pid=$(ps -f -u $UID | sed 1d | fzf -m | awk '{print $2}')
    else
        pid=$(ps -ef | sed 1d | fzf -m | awk '{print $2}')
    fi  

    if [ "x$pid" != "x" ]
    then
        echo $pid | xargs kill -${1:-9}
    fi  
}
````

This is inspired by `fd` I had, but I didn't use much.

````sh
# fda - including hidden directories
fda() {
  local dir
  local init_dir
  init_dir="$(pwd)"
  [ ! -z "$1" ] && cd "$1"
  dir=$(find ${1:-.} -type d 2> /dev/null | fzf +m) && cd "$dir"
  [ $? != 0 ] && cd $init_dir
}
````

$\to$

````sh
# fcd - fuzzy cd to any directory through home or specified directory
fcd() {
  [ -z $1 ] && from_dir="$HOME" || from_dir="$1"
  init_dir="$(pwd)"
  pushd "$from_dir" > /dev/null # go to that directory to list from that position
  dir=$(find $from_dir -type d 2> /dev/null | fzf +m)
  found=$?
  popd -n > /dev/null
  [ $found = 0 ] && cd "$dir" || cd "$init_dir"
}
````

2026-07-29 Wk 31 Wed - 19:54 +03:00

Also some git global configurations:

````sh
[alias]
  lg = log --graph --oneline
  root = rev-parse --show-toplevel
  st = status
  br = branch
  co = checkout

[core]
	editor = vim
[init]
	defaultBranch = main
````

2026-07-29 Wk 31 Wed - 22:00 +03:00

We also need to be able to have dash go to the last command starting with the substring already typed.

https://unix.stackexchange.com/questions/53814/configure-up-arrow-to-browse-through-commands-with-same-initial-characters-rathe#53844

````sh
# in ~/.inputrc
"\e[A": history-search-backward            # arrow up
"\e[B": history-search-forward             # arrow down
````

Ooh that was easy!

2026-07-30 Wk 31 Thu - 00:21 +03:00

````sh
rg --with-filename --column --line-number --no-heading --smart-case 'mypattern' | fzf
````

https://blog.bigfont.ca/navigating-bash-native-grep-with-vim-quickfix/

````sh
PAT="mypattern" && RES=$(rg --color=always --with-filename --column --line-number --no-heading --smart-case "$PAT" | fzf +m --ansi) && vim -q <(echo $RES)
````

Allows us to use vim quickfix when searching files and fuzzy selecting a selection

2026-07-30 Wk 31 Thu - 08:02 +03:00

https://codeberg.org/lan22h/dotfiles

These new dotfiles now should be largely edited in-place, and we get configuration updates automatically through sourcing. But this is still not possible for some files like `~/.gitconfig`

In that case we need to use `./install.sh`

2026-07-30 Wk 31 Thu - 14:51 +03:00

Making a `fvio` for fuzzy vim open. Sure running into a lot of issues here. File names must be quoted, they can contain spaces. You need to quote variables, or they will lose line distinctions. You also need to be careful not to erase dubious spaces like with this file: `'000 Open a PR for  DBML_SQLite and bump version of pydbml.md'` or you end up trying to open the wrong file! You might have to be careful about quoting being preserved in the `vim ...` command itself.

2026-08-02 Wk 31 Sun - 10:01 +03:00

Creating `fvit` to be able to fuzzy search a ctags file.

https://kulkarniamit.github.io/whatwhyhow/howto/use-vim-ctags.html

`vim -t <tag>`

https://bashcommands.com/bash-get-first-column

`awk '{print $1}'` for first column

This works:

````sh
vim -t $(cat tags | grep 'main_:\$' | awk '{print $1}')
````

Vim seems to specify only opening one tag at a time with `-t`.  Though there is a tag stack...

2026-08-02 Wk 31 Sun - 11:49 +03:00

https://wiki.gentoo.org/wiki/Helix

`hx` uses `*.toml` files for configuraion.
