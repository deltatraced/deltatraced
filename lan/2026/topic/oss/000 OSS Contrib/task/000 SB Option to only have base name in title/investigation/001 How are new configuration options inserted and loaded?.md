---
context_type: investigation
status: done
---

Parent: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned by: [000 SB Option to only have base name in title](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md)

Spawned in: [^spawn-invst-e3e809](../000%20SB%20Option%20to%20only%20have%20base%20name%20in%20title.md#spawn-invst-e3e809)

# Problem

We would like to be able to insert a new config option in accordance with https://github.com/silverbulletmd/silverbullet/issues/2016. But how can we do this in the source?

# Journal

2026-06-08 Wk 24 Mon - 16:55 +03:00

````ts
  openConfiguration:
    path: ./configuration.ts:openConfiguration
    command:
      name: "Configuration: Open"
````

CONFIG mentions:

````
This page holds configuration for your SilverBullet space. See [[^Library/Std/Config]] for all options and defaults.
````
