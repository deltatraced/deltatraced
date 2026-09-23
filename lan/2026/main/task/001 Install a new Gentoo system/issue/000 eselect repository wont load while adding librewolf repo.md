---
context_type: issue
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/task/001 Install a browser on gentoo - librewolf](../task/001%20Install%20a%20browser%20on%20gentoo%20-%20librewolf.md)

Spawned in: [^spawn-issue-5ac29f](../task/001%20Install%20a%20browser%20on%20gentoo%20-%20librewolf.md#spawn-issue-5ac29f)

# Issue

````sh
su
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git

# out (error)
!!! Error: Can't load module repository
````

# Resolution

We have not yet emerged `eselect repository`, and so we got the error `!!! Error: Can't load module repository`.

From https://wiki.gentoo.org/wiki/Eselect/Repository,

````sh
su
emerge --ask app-eselect/eselect-repository
````

Now it works:

````sh
su
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git

# out (relevant)
Adding librewolf to /etc/portage/repos.conf/eselect-repo.conf ...
Repository librewolf added
````

# Journal

2026-07-26 Wk 30 Sun - 17:07 +03:00

Tried to run

````sh
su
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git
emaint sync -r librewolf
````

But we get an error:

````sh
su
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git

# out (error)
!!! Error: Can't load module repository
````

https://wiki.gentoo.org/wiki/Eselect/Repository

https://forums.gentoo.org/viewtopic-t-698342-start-0.html

````sh
eselect list-modules

# out
!!! Error: Can't load module list-modules
````

We just don't have these modules it seems.

2026-07-26 Wk 30 Sun - 17:27 +03:00

From https://wiki.gentoo.org/wiki/Eselect/Repository,

````sh
su
emerge --ask app-eselect/eselect-repository
````

2026-07-26 Wk 30 Sun - 17:32 +03:00

````sh
su
eselect repository add librewolf git https://codeberg.org/librewolf/gentoo.git

# out (relevant)
Adding librewolf to /etc/portage/repos.conf/eselect-repo.conf ...
Repository librewolf added
````

OK

About the `list-modules`, we are able to run `eselect modules` instead.
