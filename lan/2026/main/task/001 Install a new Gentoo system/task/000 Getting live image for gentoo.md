---
context_type: task
status: done
---

Parent: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned by: [lan/2026/main/task/001 Install a new Gentoo system/001 Install a new Gentoo system](../001%20Install%20a%20new%20Gentoo%20system.md)

Spawned in: [^spawn-task-71c90e](../001%20Install%20a%20new%20Gentoo%20system.md#spawn-task-71c90e)

# Journal

2026-07-21 Wk 30 Tue - 04:43 +03:00

Moving away from

````sh
lsb_release -a

# out {
	No LSB modules are available.
	Distributor ID: Ubuntu
	Description:    Ubuntu 25.04
	Release:        25.04
	Codename:       plucky
# }

uname -a

# out {
	Linux lan-proart 6.14.0-37-generic #37-Ubuntu SMP PREEMPT_DYNAMIC Fri Nov 14 22:10:32 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
# }
````

I choose gentoo to try a system based primarily on building from source. I also want to experiment with different wayland-supported desktop environments, so will use sway. I can look to also configure sway and tmux with our idea of [001 Cubical Tabs](../../../../topic/ideas/000%20Ideas/entry/001%20Cubical%20Tabs/001%20Cubical%20Tabs.md) later on too.

https://wiki.gentoo.org/wiki/Gentoo_Cheat_Sheet

* https://www.gentoo.org/get-started/
* $\to$ https://wiki.gentoo.org/wiki/Handbook:Main_Page

It depends on your CPU architecture which handbook to use.

````sh
arch

# out
x86_64
````

https://wiki.gentoo.org/wiki/Handbook:AMD64

Also, their stance on LLMs is generally restrictive:

* https://wiki.gentoo.org/wiki/Project:Council/AI_policy (https://web.archive.org/web/20260714091749/https://wiki.gentoo.org/wiki/Project:Council/AI_policy)

Whereas ubuntu, where I'm moving from is much more permissive, and seems to actively use LLM tools in development, as implied by https://discourse.ubuntu.com/t/the-future-of-ai-in-ubuntu/81130.

2026-07-21 Wk 30 Tue - 12:03 +03:00

Getting the minimum installation CD: https://www.gentoo.org/downloads/amd64/

To verify, we need to confirm that we match according to this PGP signature: https://distfiles.gentoo.org/releases/amd64/autobuilds/20260712T170110Z/install-amd64-minimal-20260712T170110Z.iso.asc

Register keys (in accordance with https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media):

````sh
gpg --keyserver hkps://keys.gentoo.org --recv-keys 13EBBDBEDE7A12775DFDB1BABB572E0E2D182910
````

Says 1 bad signature for the key, but otherwise imported.

````sh
# in /home/lan/Downloads
wget https://distfiles.gentoo.org/releases/amd64/autobuilds/20260712T170110Z/install-amd64-minimal-20260712T170110Z.iso.asc

# in /home/lan/Downloads
gpg --verify install-amd64-minimal-20260712T170110Z.iso.asc install-amd64-minimal-20260712T170110Z.iso

# out
gpg: Signature made Sun 12 Jul 2026 10:41:09 PM +03
gpg:                using RSA key 534E4209AB49EEE1C19D96162C44695DB9F6043D
gpg: Good signature from "Gentoo Linux Release Engineering (Automated Weekly Release Key) <releng@gentoo.org>" [unknown]
gpg: Signature notation: manu=2,2.5+1.12,2,2
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 13EB BDBE DE7A 1277 5DFD  B1BA BB57 2E0E 2D18 2910
     Subkey fingerprint: 534E 4209 AB49 EEE1 C19D  9616 2C44 695D B9F6 043D
````

As expected in the handbook.

2026-07-21 Wk 30 Tue - 12:15 +03:00

To burn to ISO,

`df` shows that my USB is under `/dev/sda1`.

* https://pendrivelinux.com/create-bootable-usb-from-iso-using-dd/

$\to$

````sh
sudo dd if=/path/to/file.iso of=/dev/sdX bs=4M status=progress oflag=sync
````

$\to$

````sh
sudo dd if=/home/lan/Downloads/install-amd64-minimal-20260712T170110Z.iso of=/dev/sda1 bs=4M status=progress oflag=sync
````

2026-07-21 Wk 30 Tue - 12:24 +03:00

Alrighty. It's time to boot.
