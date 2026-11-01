---
context_type: investigation
status: todo
---

Parent: [000 Wiki Proc Personal Notes Cloud](../000%20Wiki%20Proc%20Personal%20Notes%20Cloud.md)

Spawned by: [000 Wiki Proc Personal Notes Cloud](../000%20Wiki%20Proc%20Personal%20Notes%20Cloud.md)

Spawned in: [^spawn-invst-4d0969](../000%20Wiki%20Proc%20Personal%20Notes%20Cloud.md#spawn-invst-4d0969)

Overview: [000 Overview Wiki Proc Personal Notes Cloud](../entry/000%20Overview%20Wiki%20Proc%20Personal%20Notes%20Cloud.md)

# Answer

1. You can get the `$5/mo` plan at https://www.ionos.com/servers/vps for an affordable VPS if you cannot host on your own.

````sh
# For the $5/mo plan with ionos

$ less /proc/meminfo
MemTotal:        1871884 kB

$ nproc
2

$ less /proc/cpuinfo

processor       : 0
vendor_id       : AuthenticAMD
cpu MHz         : 2596.404
cache size      : 512 KB

processor       : 1
vendor_id       : AuthenticAMD
cpu MHz         : 2596.404
cache size      : 512 KB

$ df -H .
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        93G  2.4G   91G   3% /
````

# Journal

2026-06-24 Wk 26 Wed - 14:11 +03:00

One option I looked at for VPS is https://www.liquidweb.com/linux-vps-hosting/, the `$5/mo` tier. It's comparable to obsidian sync's `$4/mo`, and should give us more general capabilities than just syncing for obsidian.

Then once we have a place to host, we can try to setup https://nextcloud.com/home-users/. They also have recommendations for providers.

Obsidian sync offers 1 GB storage for $4/mo (https://obsidian.md/sync)  and 10 GB for $8/mo (though upgradable to 100GB?)

You can get free 8GB, already comes apparently with nextcloud with https://fie.nl.tab.digital (one of their providers I was suggested).

This seems very heavy-handed. Not sure at a first glance how to extend this, it is its own self-complete product.

1. https://www.linuxlinks.com/best-free-open-source-self-hosted-cloud-storage-tools/
   * branch
     1. https://github.com/AtalayaLabs/OxiCloud

Some VPS services,

https://www.ionos.com/servers/vps, https://contabo.com/en/,

2026-06-25 Wk 26 Thu - 15:40 +03:00

https://www.mvps.net/docs/why-free-vps-hosting-is-not-possible gives some warnings about problems with seeking free vps hosting
