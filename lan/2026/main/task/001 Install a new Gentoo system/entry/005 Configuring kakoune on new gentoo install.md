---
context_type: entry
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md)

Spawned in: [^spawn-entry-58a03e](002%20Quick%20new%20Installs%20for%20Gentoo%20System.md#spawn-entry-58a03e)

# Journal

2026-08-02 Wk 31 Sun - 15:09 +03:00

This requires installation of https://github.com/theimpostor/osc:

````
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/kak/kakrc {
	hook global RegisterModified '"' %{
	    nop %sh{
	        # Concat selections line-by-line
	        eval set -- "$kak_quoted_selections"
	        reg=$1; shift
	        for selection; do
	            reg=$(printf '%s\n%s' "$reg" "$selection")
	        done
	
	        # Convert to OSC52 and send to client's tty
	        client_tty=/proc/$kak_client_pid/fd/0
	        encoded=$(printf %s "$reg" | base64 | tr -d '\n')
	        printf "\e]52;;%s\e\\" "$encoded" > $client_tty
	    }
	}
# }

# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/kak/
./install.sh
````

2026-08-02 Wk 31 Sun - 15:09 +03:00

`kak` is kind of noisy... Maybe we should disable all prompts by default, or pick a less bright theme at least.

https://igor-ramazanov.github.io/doc/pages/options.html

2026-08-04 Wk 32 Tue - 00:13 +03:00

For rust LSP,

https://rust-analyzer.github.io/book/other_editors.html#kakoune
