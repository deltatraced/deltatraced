---
context_type: entry
---
لألأ
Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system#^spawn-entry-232dbc|^spawn-entry-232dbc]]

# What?

One timestamp journal per service installed under `# Journal`. May also have subheaders `## Service`. 

# Journal

## Obsidian

2026-07-27 Wk 31 Mon - 09:01 +03:00

Link obsidian over to `/usr/local/bin`:

```sh
# in /home/lan/data/releases/gh/obsidianmd/obsidian-releases/obsidian-1.12.7
su
ln -s /home/lan/data/releases/gh/obsidianmd/obsidian-releases/obsidian-1.12.7/obsidian /usr/local/bin
```

If you mess up like I did and do `ln -s obsidian /usr/local/bin` and do ls on that it is in red bg and white/gray flashing like a siren that it's a bad link!

But now we can just do win+d obsidian.

## Discord

2026-07-27 Wk 31 Mon - 10:40 +03:00

https://wiki.gentoo.org/wiki/Discord

```sh
# in /etc/portage/package.accept_keywords/net-im/discord {
	net-im/discord ~amd64
# }

# in /etc/portage/package.license/net-im/discord {
	net-im/discord all-rights-reserved
# }

su
emerge --ask net-im/discord
```

OK

## Ripgrep

2026-07-28 Wk 31 Tue - 06:23 +03:00

Installing `ripgrep`,

```sh
cargo install ripgrep
```

We need `/home/lan/.cargo/bin` in `$PATH`.

```sh
# in /home/lan/.bashrc
source $HOME/.path

# in /home/lan/.path
PATH=$PATH:/home/lan/.cargo/bin
```

before/after starting a new terminal pane:

```diff
echo $PATH

# out
-/usr/local/sbin:/usr/local/bin:/usr/bin:/opt/bin:/usr/lib/llvm/22/bin
+/usr/local/sbin:/usr/local/bin:/usr/bin:/opt/bin:/usr/lib/llvm/22/bin:/home/lan/.cargo/bin
```

```sh
rg --version

# out
ripgrep 15.2.0

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

OK

## Tree

2026-07-28 Wk 31 Tue - 22:48 +03:00

Installing `tree`,

https://packages.gentoo.org/packages/app-text/tree

```sh
su
emerge --ask app-text/tree
```

```
2026-07-28 22:50:03 (165 KB/s) - ‘/var/cache/distfiles/unix-tree-2.2.1.tar.bz2.__download__’ saved [56345/56345]

 * unix-tree-2.2.1.tar.bz2 BLAKE2B SHA512 size ;-) ...                                                                                                                                                                                   [ ok ]
```

Interesting wink.

OK
it will even tr
## mgba

2026-07-30 Wk 31 Thu - 09:34 +03:00

Installing `mgba`,

https://packages.gentoo.org/packages/games-emulation/mgba

```sh
equery uses games-emulation/mgba

# out
[ Legend : U - final flag setting for installation]
[        : I - package is installed with flag     ]
[ Colors : set, unset                             ]
 * Found these USE flags for games-emulation/mgba-0.11.0_pre20260101:
 U I
 - - debug                    : Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Backtraces
 - - discord                  : Enable Discord RPC support
 - - elf                      : Enable the use of elf utils via dev-libs/elfutils
 - - ffmpeg                   : Enable ffmpeg/libav-based audio/video codec support
 - - gles2                    : Enable GLES 2.0 (OpenGL for Embedded Systems) support (independently of full OpenGL, see also: gles2-only)
 - - gles3                    : Build OpenGL ES 3.x RenderSystem
 + + gui                      : Enable support for a graphical user interface
 - - libretro                 : Build libretro port
 - - lua                      : Enable Lua scripting support
 - - lua_single_target_lua5-3 : Build for Lua 5.3 only
 - - lua_single_target_lua5-4 : Build for Lua 5.4 only
 + + opengl                   : Add support for OpenGL (3D graphics)
 + + sdl                      : Add support for Simple Direct Layer (media library)
 + - sqlite                   : Add support for sqlite - embedded sql database
 - - test                     : Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently)
```

```
The above constraints are a subset of the following complete expression:
    gui? ( any-of ( gles2 gles3 opengl ) sqlite gles3? ( any-of ( gles2 opengl ) ) ) lua? ( exactly-one-of ( lua_single_target_lua5-3 lua_single_target_lua5-4 ) )
```

```sh
# in /etc/portage/package.use/games-emulation/mgba {
	games-emulation/mgba elf debug lua lua_single_target_lua5-4
	
	# required by virtual/minizip-1.3.1::gentoo[-static-libs]
	# required by games-emulation/mgba-0.11.0_pre20260101::gentoo
	# required by games-emulation/mgba (argument)
	>=sys-libs/zlib-1.3.2-r1 minizip
# }

su
emerge --ask games-emulation/mgba
```

OK

2026-07-30 Wk 31 Thu - 16:56 +03:00

Installing gdb,

https://wiki.gentoo.org/wiki/GDB

```sh
equery uses dev-debug/gdb

# out
[ Legend : U - final flag setting for installation]
[        : I - package is installed with flag     ]
[ Colors : set, unset                             ]
 * Found these USE flags for dev-debug/gdb-17.2:
 U I
 - - babeltrace                      : Enable dev-util/babeltrace support
 + + cet                             : Enable Intel Control-flow Enforcement Technology.
 + - debuginfod                      : Enable debuginfod support via dev-libs/elfutils libdebuginfod
 - - guile                           : Add support for the guile Scheme interpreter
 - - guile_single_target_2-2         : Build only for GNU Guile 2.2.
 + + guile_single_target_3-0         : Build only for GNU Guile 3.0.
 - - lzma                            : Support lzma compression in ELF debug info
 - - multitarget                     : Support all known targets in one gdb binary
 + + nls                             : Add Native Language Support (using gettext - GNU locale utilities)
 + - python                          : Enable support for the new internal scripting language, as well as extended pretty printers
 - - python_single_target_python3_12 : Build for Python 3.12 only
 - - python_single_target_python3_13 : Build for Python 3.13 only
 + + python_single_target_python3_14 : Build for Python 3.14 only
 - - rocm                            : Enable support for AMD GPU debugging via dev-libs/rocdbgapi
 + - server                          : Install the "gdbserver" program (useful for embedded/remote targets)
 - - sim                             : Build gdb's simulators for various hardware platforms. See https://sourceware.org/gdb/wiki/Sim.
 - - source-highlight                : Enable listing highlighting via dev-util/source-highlight
 - - test                            : Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently)
 - - vanilla                         : Do not add extra patches which change default behaviour; DO NOT USE THIS ON A GLOBAL SCALE as the severity of the meaning changes drastically
 + + xml                             : Support parsing XML data files needed (at least) for cpu features, memory maps, and syscall tracing
 - - xxhash                          : Use dev-libs/xxhash to speed up internal hashing.
 - - zstd                            : Enable support for ZSTD compression
```

```sh
# in /etc/portage/package.use/dev-debug/gdb {
	dev-debug/gdb multitarget
# }

su
emerge --ask dev-debug/gdb
```

OK

## ctags

2026-07-30 Wk 31 Thu - 17:37 +03:00

Installing ctags,

https://packages.gentoo.org/packages/dev-util/ctags

```sh
su
emerge --ask dev-util/ctags
```

2026-07-31 Wk 31 Fri - 06:07 +03:00

Getting `python3-pip` and `checkpipe`,

```sh
python3 -m ensurepip

# out (error)
error: externally-managed-environment

× This environment is externally managed
╰─>
    The system-wide Python installation in Gentoo should be maintained
    using the system package manager (e.g. emerge).
```

https://wiki.gentoo.org/wiki/Pip

```
Warning

pip should not be used for package installation outside of a _virtual environment_. Doing so can break parts of the local Python installation. Even using pip install with the `--user` parameter can break things, as packages installed in this way will still be included in sys.path. Accordingly, pip will print an error message if called outside of a virtual environment. See [PEP 668](https://peps.python.org/pep-0668/) and [externally managed environments](https://packaging.python.org/en/latest/specifications/externally-managed-environments/) for details.
```

So use venv:

```sh
mkdir -p ~/.venv/venv_main
python3 -m venv ~/.venv/venv_main
source ~/.venv/venv_main/bin/activate # must use with source
```

Now we can install:

```sh
# in venv_main
pip install checkpipe
pip install --upgrade pip
```

OK

## helix

2026-08-02 Wk 31 Sun - 11:38 +03:00

Installing helix,

https://wiki.gentoo.org/wiki/Helix

```sh
su
emerge --ask app-editors/helix
```

ebuild is at `/var/db/repos/gentoo/app-editors/helix/helix-25.07.1.ebuild`

Now you can open files with `hx`. This editor also supports LSP natively. We just need the corresponding language server in `$PATH`. 

```sh
ln -s /usr/share/helix/runtime ~/.config/helix/runtime
```

--/ 2026-08-02 Wk 31 Sun - 13:04 +03:00 | Disabled

Plugin support is still uncertain, and seems planned towards using a lisp runtime (steel). Project mentions they take a more integrated approach versus katsoune's modular and minimal design philosophy to give more out of the box. This is fine, although so far I've seen it seems easier to write rust plugins for katsoune, so will try that instead for now. I might want to do a clusterline plugin here so it's important I can write it in rust.

Some research at [[004 Setup a new code editor for new gentoo]]

From https://wiki.gentoo.org/wiki/Gentoo_Cheat_Sheet,

```
Warning

There is an `--unmerge` option (`-C`), but this is not recommended and can break the system if not used with caution. This should only ever be used _**if necessary**_, and once properly informed of what it does. This _will_ **break the system**, or other software, if used on some packages. The correct way to remove packages in Gentoo is virtually always with the `--depclean` option, as described above. This may sometimes be useful to temporarily remove a hard block though.
```

```sh
su
emerge --deselect app-editors/helix
emerge --ask --depclean
```

--/

## Kakoune

2026-08-02 Wk 31 Sun - 13:23 +03:00

Installing Kakoune,

https://wiki.gentoo.org/wiki/Kakoune

```sh
# in /etc/portage/package.accept_keywords/app-editors/kakoune {
	app-editors/kakoune ~amd64
# }

su
emerge --ask app-editors/kakoune
```

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/005 Configuring kakoune on new gentoo install]] ^spawn-entry-58a03e

2026-08-02 Wk 31 Sun - 15:40 +03:00

Installing

```sh
go install -v github.com/theimpostor/osc@latest
```

Also disabling go telemetry (which they say is opt-in: https://go.dev/doc/telemetry but this disables completely)

```sh
go telemetry off
```

2026-08-02 Wk 31 Sun - 15:28 +03:00

Installing nvim,

https://wiki.gentoo.org/wiki/Neovim

```sh
emerge --ask app-editors/neovim
```

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/006 Configuring nvim on new gentoo install]] ^spawn-entry-3fc3d7

--/ 2026-08-03 Wk 32 Mon - 19:56 +03:00

We want to upgrade this so that we get `vim.pack`.

```sh
equery keywords app-editors/neovim

# out
Keywords for app-editors/neovim:
             |                             |   u   |
             | a   a   p   a   l   r   s   |   n   |
             | m   r   p   l h o m i s p m | e u s | r
             | d a m p c x p p o i s 3 a 6 | a s l | e
             | 6 r 6 p 6 8 h p n p c 9 r 8 | p e o | p
             | 4 m 4 c 4 6 a a g s v 0 c k | i d t | o
-------------+-----------------------------+-------+-------
   0.11.6-r2 | + ~ + ~ ~ + o o o o ~ o o o | 8 # 0 | gentoo
[I]0.11.7    | + ~ + ~ ~ + o o o o ~ o o o | 8 o   | gentoo
   0.12.0    | ~ ~ ~ ~ ~ ~ o o o o ~ o o o | 8 #   | gentoo
   0.12.1    | ~ ~ ~ ~ ~ ~ o o o o ~ o o o | 8 #   | gentoo
   0.12.2    | ~ ~ ~ ~ ~ ~ o o o o ~ o o o | 8 #   | gentoo
   0.12.3    | ~ ~ ~ ~ ~ ~ o o o o ~ o o o | 8 o   | gentoo
     9999    | o o o o o o o o o o o o o o | 8 o   | gentoo
```

~~Set to `9999` so it updates with the upstream directory.~~ Oops we should read this column-first. We need the keyword `amd64`, which is set as unstable for versions higher than `0.11.7`:

```sh
# in /etc/portage/package.accept_keywords/app-editors/neovim {
	app-editors/neovim ~amd64
# }

# in /etc/portage/package.accept_keywords/dev-lua/luv {
	dev-lua/luv ~amd64
# }

su
emerge --ask app-editors/neovim
```

```sh
equery keywords app-editors/neovim

# out (relevant)
Keywords for app-editors/neovim:
             |                             |   u   |
             | a   a   p   a   l   r   s   |   n   |
             | m   r   p   l h o m i s p m | e u s | r
             | d a m p c x p p o i s 3 a 6 | a s l | e
             | 6 r 6 p 6 8 h p n p c 9 r 8 | p e o | p
             | 4 m 4 c 4 6 a a g s v 0 c k | i d t | o
-------------+-----------------------------+-------+-------
[I]0.12.3    | ~ ~ ~ ~ ~ ~ o o o o ~ o o o | 8 o   | gentoo
```

--/ 

## rust-analyzer

2026-08-03 Wk 32 Mon - 21:44 +03:00

Needed in [[006 Configuring nvim on new gentoo install]].

Installing `rust-analyzer`,

https://wiki.gentoo.org/wiki/Rust

```sh
equery uses dev-lang/rust

# out (relevant)
[ Legend : U - final flag setting for installation]
[        : I - package is installed with flag     ]
[ Colors : set, unset                             ]
 * Found these USE flags for dev-lang/rust-1.95.0:
 U I
 + - abi_x86_32               : 32-bit (x86) libraries
 + - clippy                   : Install clippy, Rust code linter
 + + cpu_flags_x86_sse2       : Use the SSE2 instruction set
 + - doc                      : Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally
 - - llvm_targets_WebAssembly : WebAssembly backend
 - - rust-analyzer            : Install rust-analyzer, A Rust compiler front-end for IDEs (language server)
 - - rust-src                 : Install rust-src, needed by developer tools and for build-std (cross)
 + - rustfmt                  : Install rustfmt, Rust code formatter
 + - system-llvm              : Use the system LLVM installation
```

`rust-analyzer` requires `rust-src`. We will also need web assembly support for some projects we have.

```sh
# in /etc/portage/package.use/dev-lang/rust {
	dev-lang/rust rust-analyzer rust-src llvm_targets_WebAssembly
# }

su
emerge --ask dev-lang/rust
```

`rust-analyzer --version # out { rust-analyzer 1.95.0 }`

## lsp-query

2026-08-05 Wk 32 Wed - 09:05 +03:00

Needed in `dism-exe-notes > c72093 002 Impl struct layout parsing from bn6f inc and parse gdb memory xw log`

https://github.com/RangerMauve/lsp-query

```sh
su
npm install -g @rangermauve/lsp-query
```

Doesn't seem to work. Created an issue: https://github.com/RangerMauve/lsp-query/issues/1

```sh
su
npm uninstall -g @rangermauve/lsp-query
```

## nmap

2026-08-05 Wk 32 Wed - 09:06 +03:00

Installing nmap,

https://wiki.gentoo.org/wiki/Nmap

```sh
su
emerge --ask net-analyzer/nmap
```

## strace

2026-08-05 Wk 32 Wed - 10:57 +03:00

Installing strace,

https://packages.gentoo.org/packages/dev-debug/strace

```sh
su
emerge --ask dev-debug/strace
```

## segoon/lsp-cli

2026-08-05 Wk 32 Wed - 12:11 +03:00

Installing lsp-cli,

https://github.com/segoon/lsp-cli

```sh
cargo install lsp-cli
```

## Fonts

2026-08-06 Wk 32 Thu - 16:41 +03:00

[[006 Install fonts for new gentoo system]]

## Sway Screenshot using slurpshot

2026-08-06 Wk 32 Thu - 16:56 +03:00

https://wiki.gentoo.org/wiki/Sway#Simple_approach:_use_slurpshot

```sh
su
emerge --ask gui-apps/grim gui-apps/wl-clipboard app-misc/jq dev-libs/bemenu gui-apps/slurp
```

Might not need `bemenu` in manual approach?

Configuration:

```sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/sway/.config.d/gui-apps/grim/config {
	set $ps1 Print
	set $ps2 Control+Print
	set $ps3 Alt+Print
	set $ps4 Alt+Control+Print
	set $psf $(xdg-user-dir PICTURES)/ps_$(date +"%Y%m%d%H%M%S").png
	 
	bindsym $ps1 exec grim - | wl-copy
	bindsym $ps2 exec grim -g "$(slurp)" - | wl-copy
	bindsym $ps3 exec grim $psf
	bindsym $ps4 exec grim -g "$(slurp)" $psf
# }
```

This can also be reached as its own command: `slurp`, where you can select a region, and it outputs dimensions

## Sway switching keyboard layout

2026-08-06 Wk 32 Thu - 18:26 +03:00

Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/010 Configuring keyboard layouts and languages for new gentoo]] ^spawn-entry-72fffc


https://wiki.gentoo.org/wiki/Sway#Switching_Keyboard_Layouts

https://wiki.gentoo.org/wiki/Keyboard_layout_switching

https://wiki.gentoo.org/wiki/Localization/Guide

Find codes with `find /usr/share/keymaps/i386/ | grep 'fr-'`

```sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/sway/config {
	input type:keyboard {
		xkb_layout "us,ar"
		xkb_options "grp:alt_shift_toggle"
	}
# }
```

## Cabal

2026-08-11 Wk 33 Tue - 03:24 +03:00

- https://www.haskell.org/cabal/download.html
- $\to$ https://www.haskell.org/ghcup/

```sh
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

Installs to `~/.ghcup`.

```sh
# in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/bash/.config.d/path.sh {
	$(has_keyword "$MOD_USE" "path-ghcup") && export PATH="$PATH:$HOME/.ghcup/bin"
	$(has_keyword "$MOD_USE" "path-ghcup") && export PATH="$PATH:$HOME/.cabal/bin"
# }

# Add `path-ghcup` to MOD_USE in /home/lan/src/cloned/cb/lan22h/dotfiles/etc/bash/.bashrc
```

## Glirc

2026-08-11 Wk 33 Tue - 03:42 +03:00

https://hackage.haskell.org/package/glirc#readme

```sh
cabal install glirc
```

This one you can exit with `/exit`!

Copy the default config to `~/.config/glirc/config` and modify.

Basics of IRC: https://libera.chat/guides/basics

For TLS, it seems you require `ca-certificates` ([explanation1](https://www.herongyang.com/PKI-Certificate/Linux-ca-certificates-Package-What-Is.html)): https://packages.gentoo.org/packages/app-misc/ca-certificates

```sh
su
emerge --ask app-misc/ca-certificates
```

Generates `/etc/ca-certificates.conf`.

Can be updated with `/usr/sbin/update-ca-certificates`.`

See also:
- https://github.com/glguy/irc-core/wiki/Automatically-authenticating-to-NickServ
- https://modern.ircdocs.horse/ `IRC Protocol Standard`

## 1lab/Mikan

2026-08-22 Wk 34 Sat - 07:59 +03:00

https://codeberg.org/1lab/mikan

```sh
REPO=1lab/mikan && git clone git@codeberg.org:$REPO ~/src/cloned/cb/$REPO
cabal update
cabal install -foptimise-heavily exe:mikan
mikan --setup
```

The recommended use of this is with its agda-mode in emacs. See [[#Emacs]] for installation.

This is probably similar to Agda, although emacs is primary, other modes are possible such as `agda-vim` to try: https://agda.readthedocs.io/en/v2.6.2.2/tools/emacs-mode.html. But as agda-mode is the most supported, we should compare against it.

- https://github.com/derekelkins/agda-vim
- https://github.com/msuperdock/vim-agda

## Emacs

2026-08-22 Wk 34 Sat - 16:35 +03:00

https://wiki.gentoo.org/wiki/GNU_Emacs

```sh
# in /etc/portage/package.use/app-editors/emacs {
	app-editors/emacs gui gtk -X
# }

su
emerge --ask app-editors/emacs
emerge --ask --update --changed-use @world
```

Spawn [[011 Configuring emacs in main gentoo system]] ^spawn-entry-caaa8f

## Agda

2026-08-22 Wk 34 Sat - 18:33 +03:00

https://agda.readthedocs.io/en/v2.6.2.2/getting-started/installation.html#installing-the-agda-and-the-agda-mode-programs

```sh
cabal update
cabal install Agda

# out
Installing   Agda-2.8.0 (exe:agda)
Completed    Agda-2.8.0 (exe:agda)
Symlinking 'agda' to '/home/lan/.cabal/bin/agda'
Symlinking 'agda-mode' to '/home/lan/.cabal/bin/agda-mode'
```


Spawn [[lan/2026/main/task/001 Install a new Gentoo system/entry/012 Configuring Agda]] ^spawn-entry-9b07ea

## Fcitx

2026-08-23 Wk 34 Sun - 01:07 +03:00

Needing this to be able to type in pinyin or other IME keyboard systems.

https://wiki.gentoo.org/wiki/Fcitx

```sh
su
emerge --ask app-i18n/fcitx
```

We also want wubi:

```sh
su
emerge --ask app-i18n/fcitx-chinese-addons
```

2026-08-23 Wk 34 Sun01:34 +03:00

https://wiki.gentoo.org/wiki/Equery

```sh
equery check gtk # out (relevant) { * Checking gui-libs/gtk-4.20.4 ... }
equery check qt # out { !!! No installed packages matching 'qt' }
```

Configure fcitx environmental variables accordingly. In my case: https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#Sway

Let's also get `fcitx-configtool`:

https://packages.gentoo.org/packages/app-i18n/fcitx-configtool

```sh
su
emerge app-i18n/fcitx-configtool
```

## Wofi

2026-08-30 Wk 35 Sun - 02:43 +03:00

https://github.com/SimplyCEO/wofi

```sh
REPO=SimplyCEO/wofi && git clone --depth 1 git@github.com:$REPO ~/src/cloned/gh/$REPO

# in /home/lan/src/cloned/gh/SimplyCEO/wofi
cmake -S . -B build \
  -DCMAKE_INSTALL_PREFIX=/usr/local \
  -DENABLE_RUN=1 \
  -DENABLE_DRUN=1 \
  -DENABLE_DMENU=1
cmake --build build
```

```sh
# in /home/lan/src/cloned/gh/SimplyCEO/wofi
su

cmake --install build

# out
-- Installing: /usr/local/share/man/man1/wofi.1
-- Installing: /usr/local/share/man/man3/wofi.3
-- Installing: /usr/local/share/man/man5/wofi.5
-- Installing: /usr/local/share/man/man7/wofi.7
-- Installing: /usr/local/share/man/man3/wofi-api.3
-- Installing: /usr/local/share/man/man3/wofi-config.3
-- Installing: /usr/local/share/man/man7/wofi-keys.7
-- Installing: /usr/local/share/man/man3/wofi-map.3
-- Installing: /usr/local/share/man/man3/wofi-utils.3
-- Installing: /usr/local/share/man/man3/wofi-widget-builder.3
-- Installing: /usr/local/include/wofi/map.h
-- Installing: /usr/local/include/wofi/utils_g.h
-- Installing: /usr/local/include/wofi/utils.h
-- Installing: /usr/local/include/wofi/widget_builder_api.h
-- Installing: /usr/local/include/wofi/wofi_api.h
-- Installing: /usr/local/bin/wofi
```

