---
context_type: issue
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/task/001 Install a browser on gentoo - librewolf](../task/001%20Install%20a%20browser%20on%20gentoo%20-%20librewolf.md)

Spawned in: [^spawn-issue-2feca8](../task/001%20Install%20a%20browser%20on%20gentoo%20-%20librewolf.md#spawn-issue-2feca8)

# Issue

````sh
su
emerge --ask www-client/librewolf

# out (error, relevant)
Calculating dependencies... done!
Dependency resolution took 1.83 s (backtrack: 0/20).


!!! All ebuilds that could satisfy "www-client/librewolf" have been masked.
!!! One of the following masked packages is required to complete your request:
- www-client/librewolf-153.0_p3::librewolf (masked by: ~amd64 keyword)
- www-client/librewolf-152.0.6_p1::librewolf (masked by: ~amd64 keyword)
- www-client/librewolf-152.0.5_p1::librewolf (masked by: ~amd64 keyword)

For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
````

# Resolution

(1) https://unix.stackexchange.com/questions/9773/in-gentoo-what-is-the-difference-between-amd64-amd64-and-amd64-linux

According to (1),

1. `~` in a keyword signifies unstable. So it seems that `~amd64` would mean this package is unstable for amd64 systems.
1. This can be opted in by modifying `/etc/portage/package.keywords/`, but this was deprecated. Instead use `/etc/portage/package.accept_keywords/`

Here is how to fix this particular one:

````sh
# in /etc/portage/package.accept_keywords/www-client/librewolf {
www-client/librewolf ~amd64
# }

# in /etc/portage/package.accept_keywords/dev-libs/nss {
	>=dev-libs/nss-3.125 ~amd64
# }

su
emerge --ask www-client/librewolf
````

# Journal

2026-07-26 Wk 30 Sun - 17:43 +03:00

````sh
su
emerge --ask app-eselect/eselect-repository
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git
emaint sync -r librewolf
emerge --ask www-client/librewolf
````

But then on the emerge we get

````sh
su
emerge --ask www-client/librewolf

# out (error, relevant)
Calculating dependencies... done!
Dependency resolution took 1.83 s (backtrack: 0/20).


!!! All ebuilds that could satisfy "www-client/librewolf" have been masked.
!!! One of the following masked packages is required to complete your request:
- www-client/librewolf-153.0_p3::librewolf (masked by: ~amd64 keyword)
- www-client/librewolf-152.0.6_p1::librewolf (masked by: ~amd64 keyword)
- www-client/librewolf-152.0.5_p1::librewolf (masked by: ~amd64 keyword)

For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
````

(1) https://unix.stackexchange.com/questions/9773/in-gentoo-what-is-the-difference-between-amd64-amd64-and-amd64-linux

They mention `~` means unstable.

Hmm. It's a keyword and note a use flag? Let's try to see if we can enable it as a use flag:

````sh
# in /etc/portage/package.use/www-client/librewolf
www-client/librewolf ~amd64
````

`dispatch-conf` shows no errors.

We know it validates, since I can edit that above file and add a `HAHA` and it won't like it:

````
--- Invalid atom in /etc/portage/package.use/www-client/librewolf: HAHA
````

but otherwise silent output, so OK by silence.

But still, we get the same `masked by: ~amd64 keyword` issue.

Let's try `/etc/portage/package.keywords` instead since it's mentioned in [(1)](https://unix.stackexchange.com/questions/9773/in-gentoo-what-is-the-difference-between-amd64-amd64-and-amd64-linux) and remove `/etc/portage/package.use/www-client/librewolf`:

````sh
# in /etc/portage/package.keywords/www-client/librewolf
www-client/librewolf ~amd64
````

````sh
su
emerge --ask www-client/librewolf

# out (error, relevant)
!!! All ebuilds that could satisfy ">=dev-libs/nss-3.125" have been masked.
!!! One of the following masked packages is required to complete your request:
- dev-libs/nss-3.126::gentoo (masked by: ~amd64 keyword)
- dev-libs/nss-3.125::gentoo (masked by: ~amd64 keyword)

(dependency required by "www-client/librewolf-153.0_p3::librewolf" [ebuild])
(dependency required by "www-client/librewolf" [argument])
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.
````

We progressed! Let's add the unstable `~` for `nss` too:

````sh
# in /etc/portage/package.keywords/dev-libs/nss
>=dev-libs/nss-3.125 ~amd64
````

2026-07-26 Wk 30 Sun - 18:45 +03:00

````sh
su
emerge --ask www-client/librewolf

# out (relevant)
/usr/lib/python3.14/site-packages/portage/package/ebuild/_config/KeywordsManager.py:89: UserWarning: /etc/portage/package.keywords is deprecated, use /etc/portage/package.accept_keywords instead
  warnings.warn(
````

Change the folder to `accept_keywords` instead.
