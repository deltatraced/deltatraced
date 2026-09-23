---
context_type: judgment
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned in: [^spawn-jdgmt-1d9210](../000-clusterline-md-getting-started.md#spawn-jdgmt-1d9210)

# Decision

Moving https://codeberg.org/lan22h/clusterline-sb to an organization. The new repository: https://codeberg.org/clusterline/clusterline-sb

This is justified because:

1. There is anticipation that many associated repositories will be created.
1. There is a brand name: clusterline.

# Journal

2026-07-12 Wk 28 Sun - 00:59 +03:00

Now that we have `clusterline-sb`, and plan to possibly have many more plugins and possibly sharing dependencies between them, it makes sense for this to be its own org.

That was fairly easy! Just had to transfer ownership and it just moved to the new place!

2026-07-12 Wk 28 Sun - 01:26 +03:00

Clicking on the old link https://codeberg.org/lan22h/clusterline-sb now redirects to the new https://codeberg.org/clusterline/clusterline-sb

2026-07-12 Wk 28 Sun - 01:28 +03:00

````sh
mkdir -p /home/lan/src/cloned/cb/clusterline
cd /home/lan/src/cloned/cb/clusterline
mv ~/src/cloned/cb/lan22h/clusterline-sb .
cd clusterline-sb
# Modify `.git/config` and replace lan22h with clusterline in [remote "origin"].
````

OK
