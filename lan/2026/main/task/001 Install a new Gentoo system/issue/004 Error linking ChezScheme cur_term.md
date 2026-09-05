---
context_type: issue
status: wontfix
---

Parent: [[lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system]]

Spawned by: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System]]

Spawned in: [[lan/2026/main/task/001 Install a new Gentoo system/entry/002 Quick new Installs for Gentoo System#^spawn-issue-838600|^spawn-issue-838600]]

# Journal

2026-09-04 Wk 36 Fri - 10:27 +03:00

```sh
REPO=cisco/ChezScheme && git clone --filter=blob:none git@github.com:$REPO ~/src/cloned/gh/$REPO

# in /home/lan/src/cloned/gh/cisco/ChezScheme
./configure
make

# out (error, relevant)
/usr/x86_64-pc-linux-gnu/binutils-bin/2.46.0/ld: ta6le/boot/ta6le/libkernel.a(expeditor.o): undefined reference to symbol 'cur_term'
/usr/x86_64-pc-linux-gnu/binutils-bin/2.46.0/ld: /usr/lib64/libtinfo.so.6: error adding symbols: DSO missing from command line
collect2: error: ld returned 1 exit status
link failed
 in build-one
 in loop
 in module->hash
make: *** [Makefile:8: build] Error 1
```

https://github.com/cisco/ChezScheme/blob/main/BUILDING mentions

```
 * Header files and libraries for ncurses   [unless --disable-curses]
 * Header files and libraries for X windows [unless --disable-x11]
```

It's unclear why we will want to use these, so let's disable them.

Also, we can get this through a gentoo package instead.

https://packages.gentoo.org/packages/dev-scheme/chez

